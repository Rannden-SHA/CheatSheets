# Bash Scripting

Propietario: Adrian Gisbert Cabello

### Introducción al Bash Scripting

- Un script de Bash es un archivo de texto plano que contiene una serie de comandos que se ejecutan como si se hubieran escrito en un terminal.
- Normalmente, los scripts de Bash tienen una extensión opcional .sh para facilitar su identificación.
- Comienzan con `#!/bin/bash` y deben tener permisos de ejecución.

```bash
#!/bin/bash
# Hello World Bash Script
echo "Hello World!"
```

Para ejecutar el script:

```bash
chmod +x hello-world.sh
./hello-world.sh
```

### Variables

- Declaración de una variable: `variable=value`
- Referenciar una variable: `echo $variable`

```bash
first_name=Good
last_name=Hacker
echo $first_name $last_name
```

- Comillas simples vs. dobles:
    - Comillas simples (`'`): interpretan todo literalmente.
    - Comillas dobles (`"`): permiten la expansión de variables.

```bash
greeting='Hello World'
echo $greeting
greeting2="New $greeting"
echo $greeting2
```

- Sustitución de comandos:
    - Sintaxis moderna: `$(comando)`
    - Sintaxis antigua: ``comando``

```bash
user=$(whoami)
echo $user
```

### Argumentos

- Los scripts pueden recibir argumentos en la línea de comandos.
- Los argumentos se referencian como `$1`, `$2`, etc.

```bash
#!/bin/bash
echo "El primer argumento es $1"
echo "El segundo argumento es $2"
```

Para ejecutar:

```bash
chmod +x arg.sh
./arg.sh arg1 arg2
```

- Variables especiales:
    - `$0`: Nombre del script.
    - `$#`: Número de argumentos.
    - `$@`: Todos los argumentos.
    - `$?`: Estado de salida del último comando.
    - `$$`: ID del proceso del script.
    - `$USER`: Nombre del usuario que ejecuta el script.
    - `$HOSTNAME`: Nombre del host.
    - `$RANDOM`: Número aleatorio.
    - `$LINENO`: Número de línea actual en el script.

### Entrada del Usuario

- Capturar entrada del usuario con `read`.

```bash
#!/bin/bash
echo "Introduce tu nombre:"
read nombre
echo "Hola, $nombre"
```

Para entrada silenciosa:

```bash
#!/bin/bash
read -p 'Usuario: ' usuario
read -sp 'Contraseña: ' password
echo
echo "Usuario: $usuario, Contraseña: $password"
```

### Estructuras de Control

**If-Else:**

```bash
#!/bin/bash
read -p "Introduce tu edad: " edad
if [ $edad -lt 18 ]; then
  echo "Eres menor de edad."
else
  echo "Eres mayor de edad."
fi
```

**Elif:**

```bash
#!/bin/bash
read -p "Introduce tu edad: " edad
if [ $edad -lt 18 ]; then
  echo "Eres menor de edad."
elif [ $edad -lt 60 ]; then
  echo "Eres adulto."
else
  echo "Eres senior."
fi
```

**Operadores Booleanos:**

- AND (`&&`): Ejecuta el segundo comando solo si el primero tiene éxito.
- OR (`||`): Ejecuta el segundo comando solo si el primero falla.

```bash
#!/bin/bash
if [ $USER == "kali" ] && [ $HOSTNAME == "kali" ]; then
  echo "Usuario y host coinciden."
fi
```

### Bucles

**For:**

```bash
#!/bin/bash
for i in {1..10}; do
  echo "Número: $i"
done
```

**While:**

```bash
#!/bin/bash
contador=1
while [ $contador -le 10 ]; do
  echo "Contador: $contador"
  ((contador++))
done
```

### Funciones

- Definición y llamada a funciones.

```bash
#!/bin/bash
mi_funcion () {
  echo "Hola desde la función."
}
mi_funcion
```

- Funciones con argumentos:

```bash
#!/bin/bash
mi_funcion () {
  echo "Argumento 1: $1"
  echo "Argumento 2: $2"
}
mi_funcion "arg1" "arg2"
```

- Retornar valores:

```bash
#!/bin/bash
retorna_valor () {
  return $RANDOM
}
retorna_valor
echo "El valor retornado es $?"
```

### Ejemplos Prácticos

**Descargar Página Web y Extraer Subdominios:**

```bash
#!/bin/bash
wget www.megacorpone.com -O index.html
grep "href=" index.html | cut -d '"' -f 2 | grep "megacorpone.com"
```

**Ping Sweep:**

```bash
#!/bin/bash
for ip in $(seq 1 254); do
  ping -c 1 10.11.1.$ip | grep "64 bytes" &
done
```

**Descargar Exploits de Exploit-DB:**

```bash
#!/bin/bash
for e in $(searchsploit afd windows -w -t | grep http | cut -f 2 -d "|"); do
  exp_name=$(echo $e | cut -d "/" -f 5)
  url=$(echo $e | sed 's/exploits/raw/')
  wget -q --no-check-certificate $url -O $exp_name
done
```

### Depuración

- Usar `x` en el shebang para imprimir comandos y resultados:

```bash
#!/bin/bash -x
```

- Usar `set -e` para que el script salga si cualquier comando falla:

```bash
set -e
```