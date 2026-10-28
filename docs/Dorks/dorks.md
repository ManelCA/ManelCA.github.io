# Google Dorks

Operadores de búsqueda avanzada para encontrar información expuesta de un dominio: archivos, paneles de login, subdominios, errores, documentos, etc.

## Operadores básicos

```text
site:<DOMINIO>
```
Limita resultados a un dominio.

```text
inurl:<PALABRA>
```
Palabra en la URL.

```text
intitle:<PALABRA>
```
Palabra en el título de la página.

```text
filetype:<EXTENSION>
```
Busca por tipo de archivo (pdf, xls, sql, env, log...).

```text
intext:<PALABRA>
```
Palabra en el contenido de la página.

```text
-<PALABRA>
```
Excluye resultados con esa palabra.

## Combos útiles

Buscar paneles de login:

```text
site:<DOMINIO> inurl:login OR inurl:admin OR inurl:panel
```

Buscar documentos filtrados:

```text
site:<DOMINIO> filetype:pdf OR filetype:xlsx OR filetype:docx
```

Buscar archivos de configuración / credenciales expuestas:

```text
site:<DOMINIO> filetype:env OR filetype:log OR filetype:sql
```

Buscar subdominios indexados:

```text
site:*.<DOMINIO> -site:www.<DOMINIO>
```

Buscar páginas de error (dan pistas de tecnología/backend):

```text
site:<DOMINIO> intext:"warning" OR intext:"error" OR intext:"exception"
```

## robots.txt — primera parada siempre

Antes de nada, mira `https://<DOMINIO>/robots.txt`. Es un archivo **público** pensado para decirle a los buscadores qué NO deben indexar, pero eso mismo lo convierte en un mapa de rutas que el propio sitio quiere ocultar.

```bash
curl -s https://<DOMINIO>/robots.txt
```

Qué buscar en él:

- Líneas `Disallow: /ruta` → carpetas o paneles que no quieren que se indexen (admin, backups, APIs internas, paneles de debug...). Visítalas directamente.
- Líneas `Sitemap:` → suelen apuntar a un `sitemap.xml` con **todas** las URLs del sitio, indexadas o no.
- Comentarios (`#`) → a veces dejan notas internas del desarrollador.

Ejemplo de lo que te puedes encontrar:

```text
User-agent: *
Disallow: /admin/
Disallow: /backup/
Disallow: /api/internal/
Sitemap: https://<DOMINIO>/sitemap.xml
```

Aquí ya tendrías tres rutas interesantes que probar a mano, y un sitemap completo que revisar.

Para sacar el sitemap también por dorks:

```text
site:<DOMINIO> filetype:xml inurl:sitemap
```

## Notas

- Si `site:` no devuelve nada, Google puede no tener el dominio indexado; prueba con Bing (`site:` también funciona ahí) o busca directamente subdominios con herramientas tipo `amass`/`crt.sh`.
- Los dorks dependen del índice de Google en ese momento: resultados viejos pueden ya no existir (usa la caché o Wayback Machine para verlo igualmente).
- Existen bases de datos de dorks ya hechos (ej. Google Hacking Database, GHDB) para buscar por tecnología o CMS concretos.
