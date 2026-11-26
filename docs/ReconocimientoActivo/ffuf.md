# ffuf (Fuzz Faster U Fool)

Herramienta de línea de comandos para hacer **fuzzing web**: probar muchísimas palabras de una lista (wordlist) contra una web para descubrir **directorios, ficheros, subdominios o parámetros** que no están enlazados. Está escrita en Go, por eso es muy rápida.

## ¿Cómo funciona?

Todo gira en torno a la palabra clave **`FUZZ`**. ffuf sustituye `FUZZ` por cada línea de la wordlist y mira cómo responde el servidor:

```
http://sitio.com/FUZZ   →   http://sitio.com/admin
                            http://sitio.com/login
                            http://sitio.com/backup
                            ...
```

Si la respuesta es interesante (por ejemplo `200 OK`), te lo muestra.

## Instalación

```bash
# Kali / Debian / Ubuntu
sudo apt install ffuf

# Comprobar que funciona
ffuf -h
```

## Tu primer comando

```bash
ffuf -u http://192.168.1.109/FUZZ -w /usr/share/dirb/wordlists/common.txt
```

| Parte | Qué hace |
|---|---|
| `-u` | URL objetivo. Donde pongas `FUZZ` se probarán las palabras |
| `-w` | Wordlist (lista de palabras a probar) |

## Opciones básicas

### Qué probar

| Opción | Qué hace | Ejemplo |
|---|---|---|
| `-e` | Añade extensiones a cada palabra (con punto) | `-e .pdf,.docx,.php` |
| `-t` | Número de hilos (velocidad). Por defecto 40 | `-t 100` |
| `-recursion` | Entra en los directorios que encuentra | `-recursion -recursion-depth 2` |
| `-rate` | Máximo de peticiones por segundo | `-rate 100` |
| `-p` | Pausa entre peticiones (segundos) | `-p 0.1` |

### Qué mostrar (filtros)

Los filtros evitan que te llenes de ruido.

| Opción | Qué hace | Ejemplo |
|---|---|---|
| `-mc` | **Muestra** solo estos códigos HTTP | `-mc 200,301,403` |
| `-fc` | **Oculta** estos códigos HTTP | `-fc 404` |
| `-fs` | Oculta respuestas de este tamaño (bytes) | `-fs 1234` |
| `-fw` | Oculta respuestas con este nº de palabras | `-fw 20` |
| `-fl` | Oculta respuestas con este nº de líneas | `-fl 5` |
| `-ac` | Autocalibración: filtra solo las respuestas "falsas" | `-ac` |

### Salida

| Opción | Qué hace | Ejemplo |
|---|---|---|
| `-o` | Guarda el resultado en un fichero | `-o resultado.json` |
| `-of` | Formato de salida (`json`, `csv`, `html`...) | `-of csv` |
| `-c` | Colores en la terminal | `-c` |
| `-v` | Modo detallado (muestra la URL completa) | `-v` |

## Cómo leer la salida

```
admin                   [Status: 301, Size: 316, Words: 20, Lines: 10]
robots.txt              [Status: 200, Size: 273, Words: 15, Lines: 8]
secreto                 [Status: 403, Size: 277, Words: 20, Lines: 10]
```

- **Status**: código HTTP. `200` existe, `301/302` redirección, `403` existe pero prohibido, `401` pide login.
- **Size / Words / Lines**: tamaño de la respuesta. Útil para detectar respuestas repetidas y filtrarlas.

## Ejemplos útiles

### 1. Descubrir directorios

```bash
ffuf -u http://192.168.1.109/FUZZ -w /usr/share/dirb/wordlists/common.txt
```

### 2. Descubrir ficheros de ofimática

```bash
ffuf -u http://192.168.1.109/pentestucha/recopilacion/FUZZ \
  -w /usr/share/dirb/wordlists/common.txt \
  -e .pdf,.docx,.odt,.xlsx \
  -t 100 -mc 200,301,403
```

### 3. Búsqueda recursiva

```bash
ffuf -u http://192.168.1.109/FUZZ -w /usr/share/dirb/wordlists/common.txt \
  -recursion -recursion-depth 2
```

### 4. Descubrir subdominios / virtual hosts

```bash
ffuf -u http://sitio.com -H "Host: FUZZ.sitio.com" \
  -w /usr/share/wordlists/dirb/common.txt -fs 1234
```

`-H` añade una cabecera. Cambia `-fs 1234` por el tamaño de la respuesta "normal" para ocultarla.

### 5. Descubrir parámetros GET

```bash
ffuf -u "http://sitio.com/pagina.php?FUZZ=test" -w parametros.txt -fs 1234
```

### 6. Probar un formulario POST

```bash
ffuf -u http://sitio.com/login.php -X POST \
  -d "usuario=admin&password=FUZZ" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -w passwords.txt -fc 401
```

`-X` indica el método y `-d` los datos que se envían.

## Varias wordlists a la vez

Usa nombres personalizados en lugar de `FUZZ`:

```bash
ffuf -u http://sitio.com/DIR/FICHERO \
  -w dirs.txt:DIR -w ficheros.txt:FICHERO
```

## Wordlists recomendadas (Kali)

| Ruta | Uso |
|---|---|
| `/usr/share/dirb/wordlists/common.txt` | Rápida, buena para empezar (~4.600 palabras) |
| `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` | Más completa, tarda más |
| `/usr/share/seclists/` | Colección enorme (`sudo apt install seclists`) |

## Consejos

- Empieza con una wordlist **pequeña** y amplía después.
- Si todo devuelve `200` con el mismo tamaño, usa `-ac` o `-fs` para filtrar el ruido.
- Más hilos = más rápido, pero también más ruido y riesgo de errores. Con `-t 50` a `-t 100` suele ir bien en laboratorio.
- Guarda siempre el resultado con `-o` para no perderlo.

## Siguientes pasos para ampliar

- Combinar filtros (`-fc`, `-fs`, `-fw`) para afinar resultados.
- Usar cabeceras y cookies con `-H` (por ejemplo para webs con sesión iniciada).
- Fuzzing de APIs con `-X POST` y datos JSON.
- Pasar el tráfico por Burp Suite con `-replay-proxy http://127.0.0.1:8080`.

## Enlaces

- GitHub: <https://github.com/ffuf/ffuf>
