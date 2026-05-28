# Cómo generar hashes de archivos en Linux

En Linux puedes generar hashes de cualquier archivo utilizando diferentes algoritmos criptográficos.

Los más utilizados son:

- MD5
- SHA1
- SHA256
- SHA512

---

# ¿Para qué sirve hashear un archivo?

Los hashes permiten:

- Verificar integridad
- Detectar modificaciones
- Comparar archivos
- Identificar malware
- Validar descargas
- Trabajar en análisis forense

---

# SHA256 (recomendado)

Es el algoritmo más utilizado actualmente.

## Sintaxis

```bash
sha256sum archivo
```

## Ejemplo

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum muestra3unknown
```

Resultado:

```bash
5d41402abc4b2a76b9719d911017c592e7d8f8ab6e5d3b3f6f5f4e3d2c1b0a99  muestra3unknown
```

---

# MD5

Más antiguo y menos seguro, pero aún usado para comprobaciones rápidas.

## Sintaxis

```bash
md5sum archivo
```

## Ejemplo

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ md5sum muestra3unknown
```

---

# SHA1

Actualmente considerado débil criptográficamente.

## Sintaxis

```bash
sha1sum archivo
```

## Ejemplo

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha1sum muestra3unknown
```

---

# SHA512

Más robusto y largo que SHA256.

## Sintaxis

```bash
sha512sum archivo
```

## Ejemplo

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha512sum muestra3unknown
```

---

# Hashear todos los archivos de una carpeta

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum *
```

---

# Guardar hashes en un fichero

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum * > hashes.txt
```

---

# Verificar integridad de archivos

## 1. Crear fichero de hashes

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum * > hashes.txt
```

---

## 2. Verificar posteriormente

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum -c hashes.txt
```

Resultado:

```bash
muestra1unknown: OK
muestra2unknown: OK
```

Si un archivo cambia:

```bash
muestra3unknown: FAILED
```

---

# Uso en ciberseguridad y malware analysis

Los hashes son fundamentales en:

- SOC
- Threat Hunting
- Malware Analysis
- DFIR
- IOC Management

Permiten:

- Identificar muestras maliciosas
- Comparar malware
- Buscar archivos en VirusTotal
- Detectar alteraciones
- Compartir indicadores de compromiso (IOC)

---

# Buscar un hash en VirusTotal

Ejemplo:

1. Generar hash:

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ sha256sum malware.exe
```

2. Copiar el hash
3. Buscarlo en VirusTotal

Si el archivo ya fue analizado anteriormente, VirusTotal mostrará información sobre él.

---

# Herramientas relacionadas

## Obtener información del archivo

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ file archivo
```

---

## Ver strings internas

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ strings archivo
```

---

## Ver cabecera hexadecimal

```bash
(kali㉿kali)-[~/Downloads/archivos/unknown]
$ xxd archivo | head
```

---

# Conclusión

El hashing es una técnica fundamental en Linux y ciberseguridad para verificar integridad, identificar archivos y analizar posibles amenazas.

El algoritmo más recomendado actualmente para uso general es SHA256 mediante:

```bash
sha256sum archivo
```
