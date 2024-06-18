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

[NMAP](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/NMAP%20bf7ba4a8bcd141aebe8d90ce2d09687e.md)

[Metasploit](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Metasploit%208f767f4ca730444a8b8922b0263baef1.md)

[Hashes y Desencriptación](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Hashes%20y%20Desencriptacio%CC%81n%2024f8e5e98a3549c390ddbc691890c372.md)

[Tratamiento de la TTY](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Tratamiento%20de%20la%20TTY%2000409ac3213244199854bef2b7b37585.md)

[Bases de Datos](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Bases%20de%20Datos%20cf40196b1ba44dc283d882cd729e2c5e.md)

[Hydra Fuerza Bruta](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Hydra%20Fuerza%20Bruta%2022e000b587ab4f02b4e18de9622a95db.md)

[Crackmapexec](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/Crackmapexec%20f47dd5aba832497a8ec94f5331e78a1e.md)

[SMB Enum](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/SMB%20Enum%20714db793a28948419d42d53a6793622d.md)

[HTTP](Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa/HTTP%20e63fc23db8204f569f8afa16d8beb7d8.md)