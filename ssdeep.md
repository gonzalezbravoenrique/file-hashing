# ¿Qué es SSDEEP?

`SSDEEP` es una herramienta de fuzzy hashing utilizada en ciberseguridad y malware analysis para detectar archivos similares, aunque no sean exactamente iguales.

A diferencia de:

- MD5
- SHA1
- SHA256

que cambian completamente si un solo byte se modifica, `SSDEEP` permite medir similitud entre archivos.

---

# ¿Qué es un fuzzy hash?

Un fuzzy hash es un hash "difuso" que permite:

- Comparar similitud
- Detectar variantes de malware
- Relacionar archivos parecidos
- Encontrar versiones modificadas

---

# Diferencia con SHA256

## SHA256

Si cambias un solo byte:

```text
El hash cambia completamente
```

---

## SSDEEP

Si el archivo sigue siendo muy parecido:

```text
El hash será similar
```

Y puede calcularse un porcentaje de similitud.

---

# ¿Para qué sirve SSDEEP?

Muy utilizado en:

- Malware Analysis
- DFIR
- Threat Hunting
- Threat Intelligence
- SOC
- IOC Correlation

---

# Casos de uso

## Detectar variantes de malware

Dos malware pueden:

- Tener distinto SHA256
- Pero mismo comportamiento
- Y alta similitud SSDEEP

---

## Detectar documentos modificados

Ejemplo:

- `factura_original.pdf`
- `factura_modificada.pdf`

Aunque no sean idénticos, SSDEEP puede detectar similitud.

---

# Instalación en Kali Linux

Normalmente ya viene instalado.

Si no:

```bash
(kali㉿kali)-[~]
$ sudo apt install ssdeep
```

---

# Generar fuzzy hash

## Sintaxis

```bash
ssdeep archivo
```

---

## Ejemplo

```bash
(kali㉿kali)-[~/malware]
$ ssdeep malware.exe
```

Resultado:

```text
ssdeep,1.1--blocksize:hash:hash,filename
3072:abcdefghijklm:mnopqrstuv,"malware.exe"
```

---

# Comparar archivos

## Sintaxis

```bash
ssdeep -k archivo1 archivo2
```

---

## Ejemplo

```bash
(kali㉿kali)-[~/malware]
$ ssdeep -k malware1.exe malware2.exe
```

Resultado:

```text
94
```

---

# Interpretación

| Valor | Significado |
|---|---|
| 0 | Sin similitud |
| 1-30 | Similitud baja |
| 30-70 | Bastante similares |
| 70-100 | Muy similares |

---

# Comparación recursiva

## Generar hashes de carpeta

```bash
(kali㉿kali)-[~/samples]
$ ssdeep -r .
```

---

## Comparar con hashes previos

```bash
(kali㉿kali)-[~/samples]
$ ssdeep -m hashes.txt nueva_muestra.exe
```

---

# Diferencia entre hashes normales y fuzzy hashing

| Tipo | Detecta similitud |
|---|---|
| MD5 | ❌ |
| SHA256 | ❌ |
| SSDEEP | ✅ |

---

# Limitaciones

SSDEEP no es perfecto:

- Puede fallar con archivos muy pequeños
- Malware empaquetado puede evadirlo
- Cambios grandes reducen similitud

---

# Alternativas modernas

Actualmente también se utilizan:

- TLSH
- sdhash
- LZJD

Porque son más robustos frente a evasión.

---

# Uso en malware analysis

SSDEEP permite:

- Agrupar malware
- Detectar campañas relacionadas
- Hunting de variantes
- Clustering de muestras
- Relacionar amenazas

---

# Herramientas relacionadas

## Hash SHA256

```bash
(kali㉿kali)-[~]
$ sha256sum archivo
```

---

## Obtener IMPHASH

```bash
(kali㉿kali)-[~]
$ peframe malware.exe
```

---

## Detectar tipo de archivo

```bash
(kali㉿kali)-[~]
$ file archivo
```

---

# Conclusión

`SSDEEP` es una herramienta de fuzzy hashing que permite detectar similitud entre archivos, algo muy útil en malware analysis y threat intelligence.

A diferencia de SHA256, puede identificar variantes relacionadas incluso cuando los archivos no son idénticos.
