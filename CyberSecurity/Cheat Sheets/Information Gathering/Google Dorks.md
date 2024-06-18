# Google Dorks

Propietario: Adrian Gisbert Cabello

Esta sección es para buscar links indexados en Google con información sensible:

Para ver como era la **página hace años** buscamos en [https://web.archive.org](https://web.archive.org/)

Para buscar **urls con datos indexados** podemos probar a buscar en el buscador:

- Contraseñas: `inurl:password.txt`
- Para buscar subdominios en ine.com: `site:*ine.com`
- Para buscar en todos los subdominios de ine.com archivos .doc: `site:*.ine.com filetype:doc`
- Para buscar en todos los subdominios de ine.com paginas con el título admin: `site:*ine.com intitle:admin`

También podemos mirar en Exploit DB búsquedas que estén indexadas: [https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)

Otra herramienta muy útil es `theHarvester` si no la tenemos la instalamos, podemos hacer una búsqueda de emails, usuarios, contraseñas que estén relacionadas con un a web:

`theHarvester -d zonetransfer.me -b google,linkedin,yahoo,dnsdumpster,duckduckgo,crtsh`