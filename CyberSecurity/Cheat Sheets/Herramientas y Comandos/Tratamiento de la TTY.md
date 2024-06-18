# Tratamiento de la TTY

Propietario: Adrian Gisbert Cabello

- `/bin/bash -i`		(Para Metasploit)

- `python -c 'import pty; pty.spawn("/bin/bash")'`

- `script /dev/null -c bash` → CTRL + Z → `stty raw -echo; fg` → `reset` → `xterm` →		`export TERM=xterm` → `export SHELL=bash` → `stty rows 38 columns 116`

You can get the **number** of **rows** and **columns** executing **`stty -a`**
