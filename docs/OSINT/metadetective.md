# MetaDetective

Herramienta en **Python 3** para **descargar ficheros de una web** (scraping) y **analizar sus metadatos**. Pensada para OSINT y pentesting. Nació como alternativa a Metagoofil, que ya no analiza metadatos por sí sola.

## ¿Para qué sirve?

1. **Rastrear** una web y encontrar ficheros descargables (PDF, DOCX, XLSX, imágenes...).
2. **Descargarlos** a una carpeta local.
3. **Extraer sus metadatos**: autores, usuarios, software usado, fechas, rutas internas, etc.
4. **Exportar** los resultados (JSON, HTML...).

No usa Google, así que evita CAPTCHAs y bloqueos.

## Instalación

Necesita `exiftool` instalado.

```bash
sudo apt install libimage-exiftool-perl   # exiftool en Kali/Debian/Ubuntu

# Opción 1: pip
pip install metadetective

# Opción 2: clonar el repositorio
git clone https://github.com/franckferman/MetaDetective.git
cd MetaDetective
python3 src/MetaDetective/MetaDetective.py -h

# Opción 3: Docker
docker pull franckferman/metadetective
docker run --rm franckferman/metadetective -h
```

## Comandos básicos

### 1. Scraping (descargar ficheros de una web)

```bash
# Ver qué ficheros hay, sin descargar
python3 MetaDetective.py --scraping --scan --url https://ejemplo.com/

# Descargar ficheros a una carpeta
python3 MetaDetective.py --scraping --url https://ejemplo.com/ --download-dir ~/loot/

# Solo ciertos tipos, navegando 2 niveles de enlaces
python3 MetaDetective.py --scraping --url https://ejemplo.com/ \
  --download-dir ~/loot/ --depth 2 --extensions pdf docx xlsx
```

### 2. Analizar metadatos

```bash
# Una carpeta entera
python3 MetaDetective.py -d ~/loot/

# Uno o varios ficheros
python3 MetaDetective.py -f informe.pdf documento.docx

# Exportar a JSON o HTML
python3 MetaDetective.py -d ~/loot/ -e json -o ~/resultados/
python3 MetaDetective.py -d ~/loot/ -e html -o ~/resultados/
```

## Parámetros útiles

| Parámetro | Qué hace |
|---|---|
| `--scraping` | Activa el modo scraping |
| `--url` | URL objetivo |
| `--scan` | Solo lista los ficheros, no descarga |
| `--download-dir` | Carpeta donde se guardan los ficheros |
| `--depth` | Niveles de enlaces a seguir (por defecto `0`: solo la URL indicada) |
| `--extensions` | Filtra por tipo de fichero (`pdf docx xlsx`...) |
| `--threads` | Descargas en paralelo |
| `--rate` | Máximo de peticiones por segundo |
| `--follow-extern` | Sigue enlaces a dominios externos |
| `-d` / `-f` | Analiza un directorio / ficheros concretos |
| `-e` | Formato de exportación (`json`, `html`...) |
| `-o` | Carpeta de salida de los resultados |

## Flujo típico

```bash
# 1. Descargar
python3 MetaDetective.py --scraping --url https://ejemplo.com/ \
  --download-dir ~/loot/ --depth 2 --extensions pdf docx xlsx

# 2. Analizar y exportar
python3 MetaDetective.py -d ~/loot/ -e html -o ~/resultados/
```

## Formatos soportados

- **Documentos:** PDF, DOCX, ODT, XLS, XLSX, PPTX, ODP, RTF, CSV, XML
- **Imágenes:** JPEG, PNG, TIFF, GIF, SVG, PSD, HEIC...
- **Correo:** EML, MSG, PST, OST
- **Vídeo:** MP4, MOV

## Enlaces

- GitHub: <https://github.com/franckferman/MetaDetective>
- PyPI: <https://pypi.org/project/MetaDetective/>
