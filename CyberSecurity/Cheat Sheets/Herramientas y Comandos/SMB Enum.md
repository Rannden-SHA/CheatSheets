# SMB Enum

Propietario: Adrian Gisbert Cabello

- Enum4linux
    - Para enumerarlo todo de SMB: `enum4linux 192.168.X.X`
    - También podemos usar ***enum4linux*** para enumerar TODA la información: `enum4linux -a 192.168.X.X`
    - Para enumerar información autenticándose con un usuario: `enum4linux -a -u admin -p password 192.168.X.X`
- SMB Map
    - Podemos ver que acciones puede hacer un usuario con privilegios root con ***smbmap***:		`smbmap -H 192.168.X.X -u ¨¨ -p ¨¨` ******(si no sabemos usuarios y contraseñas lo dejamos vacío)
    - Para hacer un RCE con smbmap: `smbmap -H 192.168.X.X -u USUARIO -p CONTRASEÑA -x 'COMANDO'` (por ejemplo ipconfig)
    - Para descargar un archivo con smbmap: `smbmap -H IP -u USR -p PASS --download 'C$**\**flag.txt'`
    - Para subir un archivo con smbmap: `smbmap -H IP -u USR -p PASS --upload '/home/kali/pwned.php' 'C$**\**pwned.php'`
- SMB Client
    
    Para enumerar directorios y navegar a través de ellos podemos usar ***smbclient***:
    
    - Entrar como usuario anónimo: `smbclient //192.168.X.X/anonymous`
    - Ver directorios: `smbclient -N -L 192.168.X.X` ******| `crackmapexec smb 10.0.2.4 -u bogo -p bogo --shares`
    - Para entrar en un directorio: `smbclient //192.168.X.X/nombre_directorio` si nos pide contraseña podemos dejarla en blanco (si previamente hemos visto un usuario guest) y si no la acepta hay que meter la credencial
    - Para entrar en un directorio con un usuario: `smbclient //10.0.2.6/helios -U helios`
    
- RPC Client
    
    También podemos usar ***rpcclient***:
    
    - Entrar mediante rpcclient: `rpcclient -U ¨¨ 192.168.X.X` ******Si no ponemos ningún nombre de usuario entre las comillas nos autentificaremos de manera anónima, dejando la contraseña en blanco.
    - Entrar mediante rpcclient con una null session: `rpcclient -U ¨¨ 192.168.X.X -N`
    - Ayuda: `help`
    - Información del sistema: `srvinfo`
    - Enumerar grupos: `enumdomgroups`
    - Enumerar usuarios: `enumdomusers`
    - Enumerar dominios: `enumdomains`
    - Saber cuántos usuarios hay: `querydominfo`
    - Enumerar redes: `netshareenumall`
    - Saber el SID de un usuario: `lookupnames <nombre de usuario>`