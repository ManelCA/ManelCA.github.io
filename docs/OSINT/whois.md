# whois

Consulta información de registro de un dominio o IP: propietario, servidor registrador, fechas de creación/expiración, servidores DNS, datos de contacto (si no están ocultos por privacidad).

## Consulta básica de un dominio

```bash
whois <DOMINIO>
```

## Consulta de una IP

```bash
whois <IP>
```

Muestra a qué organización/ISP pertenece el rango (útil para saber si una IP es de un proveedor cloud, hosting, etc.).

## Forzar un servidor whois concreto

```bash
whois -h <SERVIDOR_WHOIS> <DOMINIO>
```

## Filtrar un campo concreto

```bash
whois <DOMINIO> | grep -i "registrar"
whois <DOMINIO> | grep -i "name server"
whois <DOMINIO> | grep -i "creation date"
```

## Notas

- Muchos dominios usan protección de privacidad (WHOIS privacy/proxy), por lo que los datos de contacto reales pueden no aparecer.
- Útil al inicio del reconocimiento: registrador, fecha de creación/expiración y servidores DNS (NS) dan pistas sobre la infraestructura del objetivo.
- Para IPs, el whois te dice el rango (CIDR) y la organización dueña, útil para saber si está en AWS, Azure, un hosting concreto, etc.
- Si `whois` no está instalado: `apt install whois`.
