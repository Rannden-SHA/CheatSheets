# Cheat Sheets

Propietario: Adrian Gisbert Cabello

# Comandos en consola Windows:

- Para saber los privilegios de un usuario (por si tenemos la duda de si son el mismo usuario): `getprivs`

# Comandos para terminal Linux:

- Para buscar palabras en un archivo: `grep -r ‘palabra’ archivo.txt`
- Para ver archivos ocultos y los permisos: `ls -la`

- Para obtener todos los puertos encontrados de varias máquinas después de realizar un escaneo con NMAP, podemos usar el siguiente comando: `grep '^[0-9]' tcp_scan.txt | cut -d '/' -f1 | sort -u | xargs | tr ' ' ','`
    - Con ***grep*** filtramos todas las lineas que empiecen por números del 0 al 9.
    - Con ***cut***  y ***-f1***  cortamos hasta que haya un ‘/’ en la línea y nos quedamos con la primera parte (la de la izquierda).
    - Con ***sort -u***  ordenamos los puertos y eliminamos los que estén duplicados.
    - Con ***xargs*** en vez de verlo en formato de columna, lo veremos en formato de una linea.
    - Con tr ‘ ‘ ***‘,’***  cambiaremos todos los carácteres que sean un espacio por una coma.

[Offensive Security](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Offensive%20Security%20154acd6cf6994b70b7894dd5783360c0.md)

[Enumeration](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Enumeration%202093dee5a9924139877a20588a7a8e89.md)

[SQL Injections](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/SQL%20Injections%2067f3a2200da9480a9a0c560b3fc2bb69.md)

[Pivoting](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Pivoting%204774fa8d403845fb869daeb9854c3f03.md)

[Password Hashes](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Password%20Hashes%20aa37e1b128b34498862ad4f980e1f708.md)

[Buffer Overflow](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Buffer%20Overflow%203059001c79d047a2a3820f32ebfa89b3.md)

[Privilege Escalation](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Privilege%20Escalation%2008faa4f9087b4405a932cf432fff614f.md)

[Puertos y Protocolos](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Puertos%20y%20Protocolos%2091ace2cfc8134505bb9bc6b59aa12e9b.md)

[Herramientas y Comandos](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Herramientas%20y%20Comandos%2043dec3ed70cd436b959b1d7c8a4066aa.md)

[File Transfers](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/File%20Transfers%207ca461ad0d064e1285cb340ae9cf11ae.md)

[Diccionarios](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Diccionarios%2063deda74340c44f78edaa8bb43dbb0a2.md)

[CMS](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/CMS%208355e39f70a84d548a6a164b5d097732.md)

[Vulnerabilidades](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Vulnerabilidades%208c7ff7d416e04665a9dc9fd5e02bd0d8.md)

[Reverse Shells](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Reverse%20Shells%20bff6c84076e840b3ae40bf1aa701af33.md)

[Scripts](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Scripts%20fcd4141083cd44b885f177385687c506.md)

[Information Gathering](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Information%20Gathering%20ed5aabd4cd2f41e2b70e1fa79d0477fa.md)

[Antivirus Evasion](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Antivirus%20Evasion%204537a4c798b747dbab22d98ca3c35261.md)

[Powershell Empire](Cheat%20Sheets%2011df9beb3cae4c48addc01074e49867c/Powershell%20Empire%2059d14ea7209e4d378d82322f1c33a434.md)