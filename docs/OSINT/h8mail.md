# h8mail

Herramienta de OSINT para buscar si un correo aparece en **brechas de datos (breaches)**, y para encontrar contraseñas o hashes filtrados asociados.

## Uso básico (un correo)

```bash
h8mail -t <CORREO>
```

## Varios correos desde un archivo

```bash
h8mail -t <ARCHIVO_CORREOS>
```

## Usar un archivo de configuración con API keys

h8mail necesita claves de servicios externos (HaveIBeenPwned, Hunter.io, etc.) para dar resultados completos:

```bash
h8mail -t <CORREO> -c <CONFIG.ini>
```

Genera una plantilla de configuración vacía:

```bash
h8mail -g config.ini
```

## Guardar la salida

```bash
h8mail -t <CORREO> -o resultado.csv
```

## Búsqueda local en un dump/breach ya descargado

Útil si tienes un fichero de una filtración y quieres buscar coincidencias sin usar APIs externas:

```bash
h8mail -t <CORREO> --local-breach <ARCHIVO_BREACH>
```

## Modo "loose" (menos filtrado, más resultados)

```bash
h8mail -t <CORREO> --loose
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `-t` | Correo o archivo de correos a buscar |
| `-c` | Archivo de configuración con API keys |
| `-g` | Genera plantilla de config.ini |
| `-o` | Archivo de salida (csv/json/xlsx) |
| `--local-breach` | Busca en un dump local en vez de APIs online |
| `--loose` | Búsqueda menos estricta |
| `-h` | Ayuda |

## Notas

- Sin `-c` con API keys configuradas, los resultados son muy limitados (solo lo que no requiere autenticación).
- Las fuentes y flags disponibles cambian entre versiones; comprueba con `h8mail -h`.
- Si encuentras una contraseña filtrada de un correo tuyo, cámbiala también en cualquier otro sitio donde la reutilices.
