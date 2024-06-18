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

[Offensive Security](Cheat%20Sheets%/Offensive%20Security.md)

[Enumeration](Cheat%20Sheets%/Enumeration.md)

[SQL Injections](Cheat%20Sheets%/SQL%20Injections.md)

[Pivoting](Cheat%20Sheets%/Pivoting.md)

[Password Hashes](Cheat%20Sheets%/Password%20Hashes.md)

[Buffer Overflow](Cheat%20Sheets%/Buffer%20Overflow.md)

[Privilege Escalation](Cheat%20Sheets%/Privilege%20Escalation.md)

[Puertos y Protocolos](Cheat%20Sheets%/Puertos%20y%20Protocolos.md)

[Herramientas y Comandos](Cheat%20Sheets%/Herramientas%20y%20Comandos.md)

[File Transfers](Cheat%20Sheets%/File%20Transfers.md)

[Diccionarios](Cheat%20Sheets%/Diccionarios.md)

[CMS](Cheat%20Sheets%/CMS.md)

[Vulnerabilidades](Cheat%20Sheets%/Vulnerabilidades.md)

[Reverse Shells](Cheat%20Sheets%/Reverse%20Shells.md)

[Scripts](Cheat%20Sheets%/Scripts.md)

[Information Gathering](Cheat%20Sheets%/Information%20Gathering.md)

[Antivirus Evasion](Cheat%20Sheets%/Antivirus%20Evasion.md)

[Powershell Empire](Cheat%20Sheets%/Powershell%20Empire%2059d14ea7209e4d378d82322f1c33a434.md)
