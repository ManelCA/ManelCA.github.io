# John the Ripper

Herramienta para crackear contraseñas y hashes mediante fuerza bruta, diccionario y reglas.

```bash
hashid <HASH>
john --list=formats | grep -i <FORMATO>
```

## Ataque de diccionario básico

```bash
john --wordlist=<DICCIONARIO> <ARCHIVO_HASHES>
```

Diccionario clásico (si tienes seclists/rockyou instalado):

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt <ARCHIVO_HASHES>
```

## Especificar el formato del hash

Si no lo detecta bien solo, fuérzalo:

```bash
john --format=<FORMATO> --wordlist=<DICCIONARIO> <ARCHIVO_HASHES>
```

Ejemplos de formatos comunes: `raw-md5`, `raw-sha1`, `nt`, `sha512crypt`, `bcrypt`, `krb5tgs` (Kerberoasting), `krb5asrep` (AS-REP Roasting).

## Ataque con reglas

Aplica mutaciones al diccionario (mayúsculas, números al final, leetspeak...):

```bash
john --wordlist=<DICCIONARIO> --rules <ARCHIVO_HASHES>
```

## Modo incremental (fuerza bruta)

```bash
john --incremental <ARCHIVO_HASHES>
```

## Ver las contraseñas crackeadas

John guarda el progreso en `~/.john/john.pot`, así que puedes cerrar y relanzar sin perder lo encontrado:

```bash
john --show <ARCHIVO_HASHES>
```

## Preparar archivos para John

Convierte archivos a un formato que John entienda:

```bash
# zip protegido con contraseña
zip2john <ARCHIVO.zip> > hash.txt

# PDF protegido
pdf2john <ARCHIVO.pdf> > hash.txt

# Clave SSH privada
ssh2john <ID_RSA> > hash.txt

# /etc/shadow + /etc/passwd
unshadow <passwd> <shadow> > hash.txt
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `--wordlist=` | Diccionario a usar |
| `--rules` | Aplica reglas de mutación |
| `--format=` | Fuerza el tipo de hash |
| `--incremental` | Modo fuerza bruta |
| `--show` | Muestra lo ya crackeado |
| `--fork=N` | Usa N procesos en paralelo |
| `--list=formats` | Lista todos los formatos soportados |

## Notas

- `john.pot` acumula resultados; si un hash ya salió una vez no hace falta volver a crackearlo.
- Para hashes NTLM de Windows suele ser más rápido usar `--format=nt`.
- Si tienes GPU, `hashcat` suele ir más rápido que John; John destaca por su detección automática y comodidad con wordlists/reglas.
