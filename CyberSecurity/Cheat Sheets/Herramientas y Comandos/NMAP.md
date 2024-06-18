# NMAP

Propietario: Adrian Gisbert Cabello

Para hacer un escaneo de redes IP: `nmap -sn 192.168.X.X/24`

También podemos usar netdiscovery:

Primer escaneo cuando obtenemos una o más redes IP: `sudo nmap -sS --min-rate 5000 -p- --open 192.168.X.X,Y,Z -vvv -oN tcp_scan.txt`

Se pueden añadir más de una IP si estan en el mismo rango con la ***','*** seguido de las terminaciones de las otras IP. El comando ***-sS*** es para enumerar servicios de los puertos que están corriendo, el ***min rate*** lo dejamos a 5000 para un escaneo más rápido (no aplicable en entornos reales y en escaneos que queramos que se hagan de una manera más silenciosa), el comando ***open*** para que nos muestre solo los puertos que estén abiertos y el parámetro ***oN*** es para guardar los resultados en un formato NMAP a un archivo .txt, triple verbose para ver el escaneo en tiempo real.

Segundo escaneo una vez ya sabemos los puertos abiertos: `sudo nmap -p21,22,80,445 -sSCV --open 192.168.X.X,Y,Z -Pn -vvv -oN full_scan.txt`

Se pueden añadir más de un puerto separándolos con la ***','*** . El comando ***-sCV*** nos permite enumerar versiones que están corriendo en los puertos, ***-Pn*** para que no haga resolución DNS y así ser más rápido. Triple verbose para ver el escaneo en tiempo real.

Scripts de NMAP para el puerto 445 (SMB):

- Ver los protocolos:	`nmap -p445 --script smb-protocols 192.168.X.X`
- Ver posibles credenciales: `nmap -p445 --script smb-security-mode 192.168.X.X`
- Ver directorios: `nmap -p445 --script smb-enum-shares 192.168.X.X`
- Ver usuarios: `nmap -p445 --script smb-enum-users 192.168.X.X`
- Ver sesiones: `nmap -p445 --script smb-enum-sessions 192.168.X.X`
- Saber si es vulnerable a Eternalblue: `nmap -sV -p 445 --script=smb-vuln-ms17-010 192.168.X.X`
- `nmap -p445 --script smb-enum-shares --script-args smbusername=admninistrator,smbpassword=smbserver_771 192.168.X.X`

Scripts de NMAP para el puerto 21 (FTP):

- Para entrar con fuerza bruta: `nmap 192.168.X.X --script ftp-brute --script-args userdb=/usr/share/metasploit-framework/data/wordlists/unix_users.txt -p 21`
- Listar directorios mediante conexión anónima: `nmap 192.168.X.X -p 21 --script ftp-anon`

Scripts de NMAP para el puerto 80 (HTTP):

`nmap 192.168.X.X -sV -p 80 --script http-enum`

`nmap 192.168.X.X -sV -p 80 --script http-headers`

`nmap 192.168.X.X -sV -p 80 --script http-methods --script-args http-methods.url-path#/webdav/`

Scripts de NMAP para el puerto 3306 (MySQL):

- Para saber si podemos entrar sin contraseña: `nmap 192.168.X.X -sV -p 3306 --script=mysql-empty-password`
- Para ver información: `nmap 192.168.X.X -sV -p 3306 --script=mysql-empty-info`
- Para: `nmap 192.168.X.X -sV -p 3306 --script=mysql-users --script-args=¨mysqluser='root',mysqlpass=''¨`
- Para ver bases de datos: `nmap 192.168.X.X -sV -p 3306 --script=mysql-databases --script-args=¨mysqluser='root',mysqlpass=''¨`
- Para ver directorios: `nmap 192.168.X.X -sV -p 3306 --script=mysql-variables --script-args=¨mysqluser='root',mysqlpass=''¨`
- Para ver un archivo: `nmap 192.168.X.X -sV -p 3306 --script=mysql-audit --script-args=¨mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'¨`
- Para dumpear hashes: `nmap 192.168.X.X -sV -p 3306 --script=mysql-dump-hashes --script-args=¨username='root',password=''¨`
- Para windows:
    
    → Info: `nmap 192.168.X.X -p 1433 --script ms-sql-info`
    
    → Info: `nmap 192.168.X.X -p 1433 --script ms-sql-ntlm-info --script-args msssql.instance-port=1433`
    
    → Fuerza bruta: `nmap 192.168.X.X -p 1433 --script ms-sql-brute --script-args userdb=/users.txt,passdb=/pass.txt`
    
    → Saber si podemos entrar sin contraseña: `nmap 192.168.X.X -p 1433 --script ms-sql-empty-password`
    
    → Dumpear toda la base de datos con credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-empty-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query=¨SELECT * FROM master..syslogins¨ -oN output.txt`
    
    → Dumpear hashes con unas credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria`
    
    → Ejecución remota de comandos con credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd=¨COMANDO¨`
    
    → Leer un archivo: `nmap 192.168.X.X -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd=¨type c:**\**flag.txt¨`
    

El comando -PA es parea hacer el escaneo de puertos más silencioso sin enviar tantos paquetes a la red

El comando -sU es para escanear puertos UDP

El comando -sA

El comando -f specificallu designed for firewall evasion by splitting large packets into smaller fragments (comon in windows)
