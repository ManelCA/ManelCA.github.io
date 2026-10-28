# nmap

Escáner de red para descubrir hosts, puertos abiertos, servicios, versiones y vulnerabilidades mediante scripts (NSE).

## Descubrimiento de hosts (ping sweep)

```bash
nmap -sn <RANGO_IP>/24
```

## Escaneo rápido de todos los puertos TCP

```bash
nmap -p- --min-rate 5000 -T4 <IP> -oN puertos_todos.txt
```

## Escaneo de versiones y scripts por defecto sobre puertos encontrados

```bash
nmap -sC -sV -p <PUERTOS> <IP> -oN detalle.txt
```

## Escaneo UDP (puertos más comunes, suele ser lento)

```bash
nmap -sU --top-ports 100 <IP>
```

## Detección de sistema operativo

```bash
nmap -O <IP>
```

## Usar un script NSE concreto

```bash
nmap --script <NOMBRE_SCRIPT> -p <PUERTO> <IP>
```

Ejemplos: `vuln`, `smb-enum-shares`, `http-title`, `ftp-anon`.

## Guardar en todos los formatos a la vez

```bash
nmap -sC -sV -p- <IP> -oA resultado
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `-sn` | Solo descubrimiento de hosts (sin puertos) |
| `-p-` | Todos los puertos (1-65535) |
| `-p` | Puertos concretos o rango |
| `--min-rate` | Paquetes por segundo mínimos (más velocidad) |
| `-T4` | Plantilla de timing agresiva |
| `-sC` | Scripts por defecto |
| `-sV` | Detección de versión de servicios |
| `-sU` | Escaneo UDP |
| `-O` | Detección de sistema operativo |
| `-A` | Equivale a `-sC -sV -O` + traceroute |
| `-oN/-oX/-oG/-oA` | Guarda salida en normal/XML/grepable/todos |
| `-Pn` | No hace ping previo (trata el host como vivo) |

## Notas

- Flujo típico: primero `-p-` rápido para no perder puertos, luego `-sC -sV` solo sobre los puertos encontrados (más rápido y detallado).
- Si el host no responde al ping, prueba con `-Pn`.
- `--script vuln` puede ser ruidoso y lento; úsalo cuando ya tengas identificados los servicios.
