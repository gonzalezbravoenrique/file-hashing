# Cómo identificar qué algoritmo de hash se ha utilizado

Cuando te proporcionan un hash, normalmente puedes identificar el algoritmo observando:

- Longitud del hash
- Formato
- Caracteres utilizados

---

# Método más común: longitud del hash

Cada algoritmo genera hashes con una longitud característica.

| Algoritmo | Longitud | Ejemplo |
|---|---|---|
| MD5 | 32 caracteres | `5d41402abc4b2a76b9719d911017c592` |
| SHA1 | 40 caracteres | `2aae6c35c94fcfb415dbe95f408b9ce91ee846ed` |
| SHA256 | 64 caracteres | `9f86d081884c7d659a2feaa0c55ad015...` |
| SHA512 | 128 caracteres | `cf83e1357eefb8bd...` |

---

# Ejemplos prácticos

## MD5

```bash
5d41402abc4b2a76b9719d911017c592
```

### Características

- 32 caracteres
- Hexadecimal

---

## SHA1

```bash
2aae6c35c94fcfb415dbe95f408b9ce91ee846ed
```

### Características

- 40 caracteres
- Hexadecimal

---

## SHA256

```bash
9f86d081884c7d659a2feaa0c55ad015
3bf4f1b2b0b822cd15d6c15b0f00a08
```

### Características

- 64 caracteres
- Muy utilizado actualmente

---

## SHA512

```bash
cf83e1357eefb8bdf1542850d66d8007
d620e4050b5715dc83f4a921d36ce9ce
47d0d13c5d85f2b0ff8318d2877eec2f
63b931bd47417a81a538327af927da3e
```

### Características

- 128 caracteres
- Mucho más largo

---

# Herramientas automáticas en Linux

## hashid

Kali Linux incluye una herramienta muy útil:

```bash
hashid HASH
```

## Ejemplo

```bash
(kali㉿kali)-[~]
$ hashid 5d41402abc4b2a76b9719d911017c592
```

Resultado:

```bash
Analyzing '5d41402abc4b2a76b9719d911017c592'
[+] MD2
[+] MD5
[+] NTLM
[+] Domain Cached Credentials
```

---

# Otra herramienta: hash-identifier

```bash
hash-identifier
```

Luego pegas el hash.

---

# Ejemplo con SHA256

```bash
(kali㉿kali)-[~]
$ hashid 9f86d081884c7d659a2feaa0c55ad0153bf4f1b2b0b822cd15d6c15b0f00a08
```

Resultado:

```bash
[+] SHA-256
[+] Haval-256
```

---

# Limitaciones

Algunos algoritmos comparten la misma longitud.

Por ejemplo:

- MD5
- NTLM

Ambos usan 32 caracteres.

Por eso las herramientas suelen mostrar varias posibilidades.

---

# Identificación manual rápida

| Longitud | Posible algoritmo |
|---|---|
| 32 | MD5 / NTLM |
| 40 | SHA1 |
| 56 | SHA224 |
| 64 | SHA256 |
| 96 | SHA384 |
| 128 | SHA512 |

---

# Uso en ciberseguridad

Identificar hashes es útil en:

- Pentesting
- Password Cracking
- DFIR
- Threat Hunting
- Malware Analysis

Porque permite:

- Saber qué algoritmo romper
- Elegir diccionarios adecuados
- Configurar Hashcat o John the Ripper

---

# Ejemplo con Hashcat

Si detectas que un hash es MD5:

```bash
hashcat -m 0 hash.txt rockyou.txt
```

Si es SHA256:

```bash
hashcat -m 1400 hash.txt rockyou.txt
```

---

# Conclusión

La forma más rápida de identificar un hash es:

1. Mirar su longitud
2. Utilizar herramientas como `hashid`
3. Comparar el formato hexadecimal

En Kali Linux, la herramienta más práctica suele ser:

```bash
hashid HASH
```
