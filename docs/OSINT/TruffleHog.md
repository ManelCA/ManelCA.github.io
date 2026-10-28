# TruffleHog

Herramienta para encontrar **secretos filtrados** (API keys, tokens, contraseñas, claves privadas) en repositorios git, sistemas de archivos, imágenes Docker, buckets S3, etc. Además puede **verificar** si el secreto sigue activo.

!!! tip "Importante: --no-update"
    Al arrancar, TruffleHog intenta comprobar si hay una versión nueva. Si no hay conexión o falla esa comprobación, puede no funcionar. Añadir `--no-update` evita ese paso.

## Repositorio git (remoto)

```bash
trufflehog --no-update git https://github.com/<USUARIO>/<REPO>.git
```

## Repositorio git (local)

```bash
trufflehog --no-update git file://./<RUTA_REPO>
```

## GitHub (repo, usuario u organización)

```bash
trufflehog --no-update github --repo=https://github.com/<USUARIO>/<REPO>
trufflehog --no-update github --org=<ORGANIZACION>
```

Para repos privados o evitar límites de la API, exporta un token:

```bash
export GITHUB_TOKEN=<TOKEN>
```

## Sistema de archivos

```bash
trufflehog --no-update filesystem <RUTA>
```

## Imagen Docker

```bash
trufflehog --no-update docker --image <IMAGEN>:<TAG>
```

## Solo secretos verificados

Reduce mucho el ruido mostrando solo los que siguen activos:

```bash
trufflehog --no-update git <REPO> --only-verified
```

## Sin verificar (offline)

No hace peticiones a los servicios para comprobar los secretos:

```bash
trufflehog --no-update git <REPO> --no-verification
```

## Guardar la salida en JSON

```bash
trufflehog --no-update git <REPO> --json > resultado.json
```

## Limitar el análisis

```bash
trufflehog --no-update git <REPO> --branch <RAMA>
trufflehog --no-update git <REPO> --since-commit <HASH>
trufflehog --no-update git <REPO> --max-depth 50
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `--no-update` | No comprueba actualizaciones |
| `--only-verified` | Muestra solo secretos confirmados como activos |
| `--no-verification` | No verifica los secretos |
| `--json` | Salida en JSON |
| `--branch` | Analiza una rama concreta |
| `--since-commit` | Analiza desde un commit |
| `--max-depth` | Limita la profundidad del historial |
| `--concurrency` | Número de workers |
| `--fail` | Devuelve código de salida distinto de 0 si encuentra secretos (útil en CI) |
| `-h` | Ayuda |

## Notas

- Las opciones pueden variar entre versiones. Compruébalas con `trufflehog --help` y `trufflehog git --help`.
- Si un secreto aparece como no verificado, no significa que sea inofensivo: puede estar caducado, o ser de un servicio que TruffleHog no sabe comprobar.
- En un caso real, un secreto filtrado debe **rotarse**, no solo borrarse del repo.
