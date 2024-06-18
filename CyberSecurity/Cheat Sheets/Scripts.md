# Scripts

Propietario: Adrian Gisbert Cabello

# Linux

- Descubrir hosts en redes internas en Linux:

```jsx
#!/bin/bash
for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 10.10.0.$i" &>/dev/null && echo "El siguiente host 10.10.0.$i esta activo" &
done; wait
```

- Si la máquina no tiene ping instalado podemos probar el siguiente código:

```jsx
#!/bin/bash
for host in $(seq 1 254); do
	timeout 1 bash -c "echo '' > /dev/tcp/30.30.30.$host/80 &>/dev/null" && echo "[+] HOST - 30.30.30.$host"
done; wait
```

- Port SCAN

```jsx
#!/bin/bash
for port in $(seq 1 65535); do
	timeout 1 bash -c "echo '' > /dev/tcp/<IP>/$port" &>/dev/null && echo "[*] El puerto $port esta abierto" &
done; wait
```

# Windows

- Descubrir hosts en redes internas en Windows:
    
    `ipconfig` # ips
    
    `arp -a` # para ver las interfaces de la red en la cual podemos encontrar nuevos hosts
    
    `hostname` # para ver los hosts que hay
    
- Ping Sweep:

```jsx
for /L %i in (1,1,255) do @ping -n 1 -w 200 10.185.11.%i > nul && echo 10.185.11.%i is up.
```

[Bash Scripting](Scripts%20fcd4141083cd44b885f177385687c506/Bash%20Scripting%2015ebd5df02694f6caa34a249e621346b.md)