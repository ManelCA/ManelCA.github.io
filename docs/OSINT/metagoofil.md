# Metagoofil

Herramienta de **OSINT** que busca en **Google** documentos públicos de un dominio (PDF, DOC, XLS, PPT...) y los **descarga**. Después se analizan sus metadatos para obtener información sensible.

## ¿Para qué sirve?

- Encontrar documentos públicos de una organización.
- Descargarlos en una carpeta local.
- Sacar de ellos **nombres de usuario, emails, software y versiones, rutas internas, impresoras**, etc.

Esa información sirve para preparar ataques de ingeniería social o fuerza bruta durante la fase de reconocimiento.

## Cómo funciona

```
Búsqueda en Google (site:dominio filetype:pdf)  →  Descarga de ficheros  →  Análisis de metadatos
```

> 💡 En las versiones actuales de Metagoofil **solo busca y descarga**. Para ver los metadatos hay que usar otra herramienta, como `exiftool` (o **MetaDetective**).

## Instalación

```bash
# Kali / Debian / Ubuntu
sudo apt install metagoofil

# Desde GitHub
git clone https://github.com/opsdisk/metagoofil.git
cd metagoofil
pip3 install -r requirements.txt
python3 metagoofil.py -h
```

## Comandos básicos

```bash
# Buscar y descargar PDFs y DOCX de un dominio
metagoofil -d ejemplo.com -t pdf,docx -l 50 -n 20 -o resultados/ -w

# Solo guardar la lista de enlaces, sin descargar
metagoofil -d ejemplo.com -t pdf -l 100 -f enlaces.txt

# Con pausa entre búsquedas para evitar bloqueos
metagoofil -d ejemplo.com -t pdf,xls -l 50 -e 30 -o resultados/ -w
```

## Parámetros útiles

| Parámetro | Qué hace |
|---|---|
| `-d` | Dominio objetivo |
| `-t` | Tipos de fichero separados por comas (`pdf,doc,xls,ppt,docx,xlsx,pptx`) |
| `-l` | Máximo de resultados de búsqueda |
| `-n` | Máximo de ficheros a descargar |
| `-o` | Carpeta donde se guardan los ficheros |
| `-w` | Descarga los ficheros (sin esto solo lista) |
| `-f` | Guarda los enlaces encontrados en un fichero |
| `-e` | Pausa (segundos) entre búsquedas |
| `-r` | Número de hilos de descarga |
| `-u` | User-Agent personalizado |

> Los parámetros pueden variar según la versión. Comprueba con `metagoofil -h`.

## Analizar los metadatos descargados

```bash
# Ver los metadatos de un fichero
exiftool documento.pdf

# Solo autor, creador y software de todos los ficheros
exiftool -Author -Creator -Producer -Software resultados/*
```

## Limitaciones

- Depende de Google: puede aparecer **CAPTCHA** o bloqueos si haces muchas búsquedas.
- Solo encuentra ficheros que Google tenga indexados.
- Las versiones actuales no analizan metadatos por sí mismas.

## Alternativas

- **MetaDetective**: scrapea la web directamente y analiza los metadatos.
- **FOCA**: similar, con interfaz gráfica (solo Windows).
- **ExifTool**: extrae metadatos de ficheros locales.

## Enlaces

- GitHub: <https://github.com/opsdisk/metagoofil>
