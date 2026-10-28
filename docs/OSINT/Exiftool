# ExifTool

Herramienta para leer, escribir y borrar **metadatos** de archivos (imágenes, PDFs, documentos Office, audio, vídeo...). Muy usada en OSINT para sacar info oculta de ficheros públicos (geolocalización GPS, autor, software usado, fechas, impresoras, usuarios de red...).

## Ver todos los metadatos de un archivo

```bash
exiftool <ARCHIVO>
```

## Varios archivos / carpeta entera

```bash
exiftool <CARPETA>/
exiftool -r <CARPETA>/
```

`-r` hace la búsqueda recursiva en subcarpetas.

## Extraer solo coordenadas GPS

```bash
exiftool -gps:all <ARCHIVO>
```

Convertirlas directamente a un link de Google Maps:

```bash
exiftool -c "%.6f" -gpslatitude -gpslongitude <ARCHIVO>
```

## Ver un campo concreto

```bash
exiftool -Author <ARCHIVO>
exiftool -Software <ARCHIVO>
exiftool -CreateDate <ARCHIVO>
```

## Salida en JSON (útil para parsear con scripts)

```bash
exiftool -j <ARCHIVO>
```

## Borrar todos los metadatos

```bash
exiftool -all= <ARCHIVO>
```

Esto crea una copia de seguridad `<ARCHIVO>_original`. Para no dejarla:

```bash
exiftool -all= -overwrite_original <ARCHIVO>
```

## Modificar un campo

```bash
exiftool -Author="<NOMBRE>" <ARCHIVO>
```

## Comparar metadatos entre dos archivos

```bash
exiftool -a -G1 -s <ARCHIVO1> <ARCHIVO2>
```

## Parámetros principales

| Flag | Qué hace |
|------|----------|
| `-r` | Recursivo en carpetas |
| `-gps:all` | Muestra solo datos GPS |
| `-j` | Salida en JSON |
| `-all=` | Borra todos los metadatos |
| `-overwrite_original` | No deja copia `_original` al editar |
| `-a` | Muestra tags duplicados |
| `-G1` | Agrupa por categoría |
| `-s` | Nombres de tag cortos
