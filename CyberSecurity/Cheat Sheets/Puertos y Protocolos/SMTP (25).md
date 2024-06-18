# SMTP (25)

Propietario: Adrian Gisbert Cabello

Este puerto es para usuarios de correo

- Para enumerar usuarios: `smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.X.X`
- Para explotar un servicio SMTP de Linux, usar el exploit haraka en Metasploit: `search type:exploit name:haraka`