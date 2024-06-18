# SSH (22)

Propietario: Adrian Gisbert Cabello

- Buscamos exploits con la versión del SSH
- Comando para fuerza bruta con hydra: `hydra -L /usr/share/metasploit-framework/data/wordlists/unix_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ssh://192.168.X.X`
- Comando para fuerza bruta con nmap:	`nmap 192.168.X.X -p 22 --script ssh-brute --script-args userdb=/home/kali/usuarios.txt`
- Comando para enumerar los algoritmos y saber que algoritmos usar para crear llaves SSH: 	`nmap 192.168.X.X -p 22 --script ssh2-enum-algos`
- Comando para enumerar el hostkey: `nmap 192.168.X.X -p 22 --script ssh-hostkey --script-args ssh_hostkey=full`
- Comando para ver el método de autentificación para un usuario: `nmap 192.168.X.X -p 22 --script ssh-auth-methods --script-args="ssh.user=student"`
- Para saber la versión de SSH: 	`nc 192.168.X.X 22`
- Para loggearse por medio de SSH: `ssh usuario@192.168.X.X` → darle al enter y luego nos pedirá la contraseña
- Para logearse a través de la consola de  metasploit: `search ssh_login`

- TÚNELES SSH
    
    Reverse  SSH port forwarding specifies that the given port on the remote server  host is to be forwarded to the given host and port on the local side.
    
    - **L** is a local tunnel (YOU <-- CLIENT). If a site was blocked, you can forward the traffic to a server you own and view it. For example, if imgur was blocked at work, you can do **ssh -L 9000:imgur.com:80 user@example.com.** Going to localhost:9000 on your machine, will load imgur traffic using your other server.
    - **R** is a remote tunnel (YOU --> CLIENT). You forward your traffic to the  other server for others to view. Similar to the example above, but in  reverse.
    
    Answer the questions below
    
    We will use a tool called **ss** to investigate sockets running on a host.
    
    If we run **ss -tulpn** it will tell us what socket connections are running
    
    | Argument | Description |
    | --- | --- |
    | -t | Display TCP sockets |
    | -u | Display UDP sockets |
    | -l | Displays only listening sockets |
    | -p | Shows the process using the socket |
    | -n | Doesn't resolve service names |
    
    How many TCP sockets are running?
    
    We can see that a service running on  port 10000 is blocked via a firewall rule from the outside (we can see  this from the IPtable list). However, Using an SSH Tunnel we can expose  the port to us (locally)!
    
    From our local machine, run **ssh -L 10000:localhost:10000 <username>@<ip>**
    
    Once complete, in your browser type "localhost:10000" and you can access the newly-exposed webserver.