# ¿Qué es Authentihash?

`Authentihash` es un tipo especial de hash utilizado en ejecutables PE de Windows (`.exe`, `.dll`) que ignora la firma digital Authenticode del archivo.

Se usa principalmente en:

- Malware Analysis
- Threat Intelligence
- DFIR
- SOC
- Threat Hunting

---

# Problema de los hashes tradicionales

Si un ejecutable firmado cambia únicamente en:

- Su firma digital
- Certificado
- Timestamp

Entonces:

```text
MD5 / SHA256 cambian completamente
```

Aunque el binario real sea exactamente el mismo.

---

# ¿Qué hace Authentihash?

`Authentihash`:

- Ignora la firma Authenticode
- Hashea solo el contenido real del ejecutable
- Permite comparar binarios firmados correctamente

---

# ¿Qué es Authenticode?

Authenticode es el sistema de firma digital de Microsoft utilizado para:

- Validar integridad
- Verificar el editor
- Garantizar autenticidad

Muy usado en:

- Drivers
- Software legítimo
- Malware firmado robado

---

# Diferencia entre SHA256 y Authentihash

| Tipo | Incluye firma digital |
|---|---|
| SHA256 | ✅ Sí |
| Authentihash | ❌ No |

---

# Ejemplo práctico

## Archivo original

```text
programa.exe
```

SHA256:

```text
AAA111
```

Authentihash:

```text
BBB222
```

---

## Mismo binario con otra firma

```text
programa_firmado.exe
```

SHA256:

```text
CCC333
```

Authentihash:

```text
BBB222
```

---

# ¿Por qué es útil?

Permite detectar:

- Mismo malware firmado de forma distinta
- Reempaquetados con nuevos certificados
- Reutilización de binarios
- Cambios únicamente en firma digital

---

# Uso en malware analysis

Muy útil para:

- Clustering de malware
- IOC correlation
- Threat Intelligence
- Attribution
- Hunting de variantes

---

# Obtener Authentihash con LIEF

## Instalar

```bash
pip install lief
```

---

## Script ejemplo

```python
import lief

binary = lief.parse("malware.exe")

print(binary.authentihash(lief.PE.ALGORITHMS.SHA_256))
```

---

# Obtener Authentihash con pefile

`pefile` no lo soporta directamente.

Normalmente se usa:

- LIEF
- YARA
- capa
- herramientas DFIR avanzadas

---

# Relación con YARA

YARA puede utilizar `authentihash` para detectar malware similar.

Ejemplo conceptual:

```yara
pe.authentihash() == "HASH"
```

---

# Diferencia entre hashes PE

| Tipo | Qué analiza |
|---|---|
| SHA256 | Archivo completo |
| IMPHASH | Tabla de imports |
| SSDEEP | Similitud |
| Authentihash | PE ignorando firma digital |

---

# Uso en Threat Intelligence

Permite:

- Agrupar malware firmado
- Detectar reutilización
- Correlacionar campañas
- Comparar ejecutables firmados

---

# Limitaciones

Authentihash:

- Solo funciona con PE de Windows
- No detecta similitud como SSDEEP
- Cambia si cambia el binario real

---

# Herramientas relacionadas

## Obtener SHA256

```bash
(kali㉿kali)-[~]
$ sha256sum malware.exe
```

---

## Obtener IMPHASH

```bash
(kali㉿kali)-[~]
$ peframe malware.exe
```

---

## Obtener fuzzy hash

```bash
(kali㉿kali)-[~]
$ ssdeep malware.exe
```

---

## Ver firma digital PE

```bash
(kali㉿kali)-[~]
$ osslsigncode verify malware.exe
```

---

# Conclusión

`Authentihash` es un hash especializado para ejecutables PE de Windows que ignora la firma digital Authenticode.

Es muy útil en malware analysis y threat intelligence para detectar binarios idénticos aunque hayan sido firmados de manera diferente.
