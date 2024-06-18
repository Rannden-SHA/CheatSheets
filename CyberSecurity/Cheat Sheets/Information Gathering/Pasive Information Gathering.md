# Pasive Information Gathering

Propietario: Adrian Gisbert Cabello

### 1. Introducción a la Recopilación de Información Pasiva

La recopilación de información pasiva (también conocida como Inteligencia de Fuentes Abiertas u OSINT) es el proceso de recopilar información disponible públicamente sobre un objetivo, generalmente sin ninguna interacción directa con ese objetivo. Este proceso es cíclico en lugar de lineal, ya que el "próximo paso" depende de lo que se encuentre en los pasos anteriores, creando "ciclos" de procesos .

### 2. Herramientas y Técnicas de Recopilación de Información Pasiva

### 2.1. Notas

- Mantener notas detalladas y bien formateadas es crucial para recuperar información más tarde y para realizar búsquedas adicionales .

### 2.2. Website Recon

- Revisar el sitio web del cliente para obtener información básica.
- Ejemplo:
    
    ```bash
    wget -r -l 2 -P recon/ https://www.megacorpone.com/
    ```
    
    - Analizar los correos electrónicos y cuentas de Twitter de los empleados listados en la página "Acerca de" .

### 2.3. Whois Enumeration

- Utilizar la herramienta `whois` para obtener información de registro de dominio.
    
    ```bash
    whois megacorpone.com
    ```
    

### 2.4. Google Hacking

- Utilizar operadores avanzados de Google para encontrar información sensible.
    - Ejemplo de operadores:
        - `site:megacorpone.com filetype:pdf`
        - `intitle:"index of" "parent directory" site:megacorpone.com`

### 2.5. Netcraft

- Utilizar Netcraft para obtener información sobre los servidores y tecnologías utilizadas por el dominio.
    - URL: [Netcraft](https://searchdns.netcraft.com/)
    - Ejemplo de uso:
        
        ```bash
        curl -s "https://searchdns.netcraft.com/?restriction=site+contains&host=megacorpone.com" | grep -E 'megacorpone.com'
        ```
        

### 2.6. Recon-ng

- Un framework modular para la recopilación de información web.
    
    ```bash
    recon-ng
    ```
    
    - Ejemplo de módulo:
        
        ```bash
        use recon/domains-hosts/bing_domain_web
        set SOURCE megacorpone.com
        run
        ```
        

### 2.7. Open-Source Code

- Buscar repositorios de código abierto relacionados con el objetivo en plataformas como GitHub.
    
    ```bash
    git clone https://github.com/megacorpone/project.git
    grep -r "password" project/
    ```
    

### 2.8. Shodan

- Utilizar Shodan para buscar dispositivos conectados a Internet asociados con el dominio.
    - URL: [Shodan](https://www.shodan.io/)
    - Ejemplo de búsqueda:
        
        ```bash
        shodan search --fields ip_str,port,org,hostnames "megacorpone.com"
        ```
        

### 2.9. Security Headers Scanner

- Utilizar herramientas como Security Headers para analizar las cabeceras de respuesta HTTP.
    - URL: [Security Headers](https://securityheaders.com/)
    - Ejemplo de uso:
        
        ```bash
        curl -I https://www.megacorpone.com
        ```
        

### 2.10. SSL Server Test

- Utilizar herramientas como SSL Labs para probar la configuración SSL del servidor.
    - URL: SSL Labs
    - Ejemplo de uso:
        
        ```bash
        sslyze --regular www.megacorpone.com
        ```
        

### 2.11. Pastebin

- Buscar información sensible publicada en Pastebin.
    - URL: [Pastebin](https://pastebin.com/)
    - Ejemplo de uso:
        
        ```bash
        curl -s "https://pastebin.com/search?q=megacorpone.com" | grep -E 'megacorpone.com'
        ```
        

### 2.12. Recopilación de Información de Usuarios

- Obtener información sobre los empleados de la organización objetivo.
    - Ejemplo con `theHarvester`:
        
        ```bash
        theHarvester -d megacorpone.com -b google
        ```
        

### 2.13. Herramientas de Redes Sociales

- Utilizar herramientas como Social-Searcher para buscar información en redes sociales.
    - URL: [Social-Searcher](https://www.social-searcher.com/)
    - Ejemplo de uso:
        
        ```bash
        curl -s "https://www.social-searcher.com/social-buzz/?q=megacorpone" | grep -E 'megacorpone'
        ```
        

### 3. Marcos de Recopilación de Información

### 3.1. OSINT Framework

- Un marco que incluye herramientas y sitios web de recopilación de información en una ubicación central.
    - URL: [OSINT Framework](https://osintframework.com/)

### 3.2. Maltego

- Herramienta de minería de datos muy poderosa que ofrece una combinación interminable de herramientas y estrategias de búsqueda.
    - URL: [Maltego](https://www.paterva.com/)
    - Ejemplo de uso:
        
        ```bash
        maltego
        ```