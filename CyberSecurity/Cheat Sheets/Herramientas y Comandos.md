# Herramientas y Comandos

Propietario: Adrian Gisbert Cabello

# Comandos para terminal Linux:

- Para buscar palabras en un archivo: `grep -r ‘palabra’ archivo.txt`

- Para ver archivos ocultos y los permisos: `ls -la`

- Para obtener todos los puertos encontrados de varias máquinas después de realizar un escaneo con NMAP, podemos usar el siguiente comando: `grep '^[0-9]' tcp_scan.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','`

Con ***grep*** filtramos todas las lineas que empiecen por números del 0 al 9.

Con ***cut***  y ***-f1***  cortamos hasta que haya un ‘/’ en la línea y nos quedamos con la primera parte (la de la izquierda).

Con ***sort -u***  ordenamos los puertos y eliminamos los que estén duplicados.

Con ***xargs*** en vez de verlo en formato de columna, lo veremos en formato de una linea.

Con tr ‘ ‘ ‘***,’***  cambiaremos todos los carácteres que sean un espacio por una coma.

- Para saber la Release de la distro: `cat /etc/*issue`

- Para saber la versión del Kernel: `uname -r`

# Comandos en consola Windows:

- Para saber los privilegios de un usuario (por si tenemos la duda de si son el mismo usuario): `getprivs`

[NMAP](Herramientas%20y%20Comandos/NMAP.md)

[Metasploit](Herramientas%20y%20Comandos/Metasploit.md)

[Hashes y Desencriptación](Herramientas%20y%20Comandos/Hashes%20y%20Desencriptacio%CC%81n.md)

[Tratamiento de la TTY](Herramientas%20y%20Comandos/Tratamiento%20de%20la%20TTY.md)

[Bases de Datos](Herramientas%20y%20Comandos/Bases%20de%20Datos.md)

[Hydra Fuerza Bruta](Herramientas%20y%20Comandos/Hydra%20Fuerza%20Bruta.md)

[Crackmapexec](Herramientas%20y%20Comandos/Crackmapexec.md)

[SMB Enum](Herramientas%20y%20Comandos/SMB%20Enum.md)

[HTTP](Herramientas%20y%20Comandos/HTTP.md)
