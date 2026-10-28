# dnsenum

Herramienta de enumeración DNS: saca registros del dominio, intenta transferencia de zona (AXFR) y fuerza bruta de subdominios con un diccionario.

## Uso básico

```bash
dnsenum <DOMINIO>
```

Por defecto intenta: obtener registros del dominio (NS, MX, A, etc.), probar transferencia de zona en cada NS, y fuerza bruta de subdominios con su diccionario interno.

## Fuerza bruta de subdominios con diccionario propio

```bash
dnsenum --file <DICCIONARIO> <DOMINIO>
```

## Especificar un servidor DNS concreto

```bash
dnsenum --dnsserver <IP_DNS> <DOMINIO>
```

## Guardar resultados

```bash
dnsenum <DOMINIO> -o resultado.xml
```

## Resolución inversa de un rango de IPs

```bash
dnsenum --reverse -r <RANGO_IP>/24
```

## Número de hilos (más velocidad en fuerza bruta)

```bash
dnsenum --threads <N> --file <DICCIONARIO> <DOMINIO>
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `--file` | Diccionario para fuerza bruta de subdominios |
| `--dnsserver` | Usa un servidor DNS concreto |
| `--reverse` | Resolución inversa (IP → nombre) |
| `-r` | Rango de IPs para resolución inversa |
| `--threads` | Número de hilos en paralelo |
| `-o` | Archivo de salida |
| `--noreverse` | No hace resolución inversa de los resultados |

## Notas

- Si la transferencia de zona (AXFR) tiene éxito contra algún NS, obtienes **todos** los registros del dominio de golpe: mala configuración grave del servidor DNS, muy buena señal a explotar.
- La fuerza bruta solo encuentra subdominios que estén en el diccionario, así que un diccionario más grande (ej. SecLists) da más cobertura pero tarda más.
- Alternativas similares: `dnsrecon`, `fierce`, `amass` (más moderno y completo para subdominios).
