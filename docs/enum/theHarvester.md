# theHarvester

Herramienta de OSINT para recopilar correos, subdominios, hosts e IPs de fuentes públicas.

## Uso básico

```bash
theHarvester -d <DOMINIO> -b all
```

## Elegir una fuente concreta

```bash
theHarvester -d <DOMINIO> -b crtsh
theHarvester -d <DOMINIO> -b duckduckgo
theHarvester -d <DOMINIO> -b hackertarget
```

## Limitar resultados

```bash
theHarvester -d <DOMINIO> -b duckduckgo -l 200
```

## Guardar la salida

Genera archivos `.json` y `.xml` con el nombre que indiques:

```bash
theHarvester -d <DOMINIO> -b all -f resultado
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `-d` | Dominio objetivo |
| `-b` | Fuente de datos (`all` para todas) |
| `-l` | Límite de resultados |
| `-f` | Archivo de salida |
| `-h` | Ayuda con todas las opciones |

## Fuentes con API key

Algunas fuentes (Shodan, VirusTotal, Hunter, etc.) necesitan una clave. Se configuran en el archivo `api-keys.yaml` de theHarvester. La ruta varía según la instalación, y puedes comprobarla con `theHarvester -h` o en su documentación.

## Notas

- Las fuentes disponibles cambian entre versiones. Lista las tuyas con `theHarvester -h`.
- Extrae los subdominios del resultado para pasarlos a otras herramientas (nmap, httpx...).
