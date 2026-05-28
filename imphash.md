# ¿Qué es un IMPHASH?

`imphash` (Import Hash) es un tipo especial de hash utilizado en malware analysis y análisis forense para identificar ejecutables de Windows según las librerías y funciones que importan.

No hashea el archivo completo.

En su lugar, genera un hash basado en:

- DLLs importadas
- APIs utilizadas
- Orden de importación

---

# ¿Para qué sirve?

El `imphash` permite:

- Relacionar muestras de malware similares
- Detectar familias de malware
- Identificar variantes
- Hacer Threat Hunting
- Buscar muestras relacionadas en VirusTotal

Aunque dos malware sean diferentes binariamente, pueden compartir el mismo `imphash` si usan las mismas funciones importadas.

---

# ¿Cómo funciona?

Los ejecutables PE de Windows (`.exe`, `.dll`) contienen una tabla llamada:

```text
Import Address Table (IAT)
```

Ahí aparecen funciones importadas como:

```text
kernel32.CreateFileA
user32.MessageBoxA
ws2_32.socket
```

El `imphash`:

1. Extrae las importaciones
2. Las normaliza
3. Genera un hash MD5 sobre esa lista

---

# Ejemplo conceptual

## Malware A

Importa:

```text
kernel32.CreateFileA
kernel32.WriteFile
ws2_32.socket
```

---

## Malware B

Importa exactamente las mismas funciones.

Aunque:

- Cambie el código
- Cambie el empaquetado
- Cambie el tamaño

Ambos pueden compartir el mismo `imphash`.

---

# Obtener el imphash en Linux

## Con `pefile` en Python

```bash
pip install pefile
```

---

## Script ejemplo

```python
import pefile

pe = pefile.PE("malware.exe")
print(pe.get_imphash())
```

---

# Obtener imphash con `peframe`

```bash
peframe malware.exe
```

Resultado:

```text
Imphash: 5f3d9c2a4b6d8e7f1234567890abcdef
```

---

# Obtener imphash con `lief`

```python
import lief

binary = lief.parse("malware.exe")
print(binary.imphash())
```

---

# Relación con VirusTotal

VirusTotal utiliza `imphash` para:

- Agrupar muestras similares
- Relacionar campañas
- Detectar malware reutilizado

---

# Diferencia entre hash normal e imphash

| Tipo | Qué hashea |
|---|---|
| MD5 / SHA256 | Archivo completo |
| IMPHASH | Tabla de importaciones PE |

---

# Ejemplo práctico

## SHA256

Si cambia 1 byte del archivo:

```text
SHA256 cambia completamente
```

---

## IMPHASH

Si el malware sigue usando las mismas APIs:

```text
IMPHASH puede mantenerse igual
```

---

# Uso en malware analysis

El `imphash` es muy útil para:

- Clustering de malware
- Attribution
- Threat Intelligence
- YARA Rules
- IOC correlation
- Hunting en SIEM

---

# Limitaciones

No siempre es fiable porque:

- Malware empaquetado puede alterar imports
- Algunas muestras usan carga dinámica (`LoadLibrary`, `GetProcAddress`)
- Cambios pequeños en imports modifican el imphash

---

# Herramientas relacionadas

## Detectar tipo de archivo

```bash
(kali㉿kali)-[~]
$ file malware.exe
```

---

## Ver imports PE

```bash
(kali㉿kali)-[~]
$ objdump -x malware.exe
```

---

## Extraer strings

```bash
(kali㉿kali)-[~]
$ strings malware.exe
```

---

# Conclusión

El `imphash` es una técnica utilizada en malware analysis para identificar y relacionar ejecutables PE de Windows según sus importaciones.

Es especialmente útil para detectar:

- Familias de malware
- Variantes similares
- Reutilización de código
- Campañas maliciosas relacionadas
