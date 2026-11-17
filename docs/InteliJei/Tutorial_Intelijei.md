---
layout: post
title: "Clonar un repositorio de GitLab en Debian y trabajar con él en IntelliJ IDEA"
description: "Guía paso a paso: instalar Git, clonar desde GitLab, abrir el proyecto en IntelliJ, ejecutar tests y trabajar con ramas, commits y push."
tags: [git, gitlab, intellij, debian, tutorial]
---

# Clonar un repositorio de GitLab en Debian y trabajar con él en IntelliJ IDEA

Guía práctica para clonar un proyecto de GitLab en el escritorio de Debian, abrirlo en IntelliJ IDEA, ejecutar los tests y trabajar con Git (commits, push, ramas, merge requests).

## Índice

1. [Requisitos previos](#1-requisitos-previos)
2. [Instalar y configurar Git](#2-instalar-y-configurar-git)
3. [Conectar con GitLab mediante SSH](#3-conectar-con-gitlab-mediante-ssh)
4. [Clonar el repositorio en el escritorio](#4-clonar-el-repositorio-en-el-escritorio)
5. [Abrir el proyecto en IntelliJ IDEA](#5-abrir-el-proyecto-en-intellij-idea)
6. [Ejecutar los tests](#6-ejecutar-los-tests)
7. [Mini tutorial de Git en IntelliJ](#7-mini-tutorial-de-git-en-intellij)
8. [Flujo de trabajo completo con ramas](#8-flujo-de-trabajo-completo-con-ramas)
9. [Resolver conflictos](#9-resolver-conflictos)
10. [Chuleta de comandos Git](#10-chuleta-de-comandos-git)
11. [Problemas frecuentes](#11-problemas-frecuentes)

---

## 1. Requisitos previos

- Debian (o derivada) con acceso a terminal.
- Una cuenta en GitLab con acceso al repositorio.
- IntelliJ IDEA instalado (Community o Ultimate).
- JDK instalado (normalmente IntelliJ puede descargarlo por ti).

Comprueba que tienes `sudo` y conexión a internet.

---

## 2. Instalar y configurar Git

Abre una terminal y ejecuta:

```bash
sudo apt update
sudo apt install git -y
git --version
```

Configura tu identidad (aparecerá en todos tus commits):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_email@ejemplo.com"
```

Usa el mismo email que tienes en tu cuenta de GitLab. Opcionalmente, define `main` como rama inicial por defecto y comprueba la configuración:

```bash
git config --global init.defaultBranch main
git config --global --list
```

---

## 3. Conectar con GitLab mediante SSH

Usar SSH evita tener que escribir usuario y contraseña en cada `push`.

### 3.1 Generar la clave

```bash
ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"
```

Pulsa Enter para aceptar la ruta por defecto (`~/.ssh/id_ed25519`). Puedes poner una contraseña (passphrase) o dejarla vacía.

### 3.2 Copiar la clave pública

```bash
cat ~/.ssh/id_ed25519.pub
```

Copia todo el texto que aparece (empieza por `ssh-ed25519`).

### 3.3 Añadirla a GitLab

1. Entra en GitLab e inicia sesión.
2. Pulsa tu avatar → **Preferences** (Preferencias) → **SSH Keys**.
3. Pega la clave en el campo **Key**, ponle un título (por ejemplo "Debian portátil") y pulsa **Add key**.

### 3.4 Probar la conexión

```bash
ssh -T git@gitlab.com
```

La primera vez te preguntará si confías en el host: escribe `yes`. Deberías ver un mensaje de bienvenida con tu usuario.

> Si tu GitLab es de una empresa o instituto (por ejemplo `gitlab.miinstituto.es`), sustituye `gitlab.com` por ese dominio.

---

## 4. Clonar el repositorio en el escritorio

### 4.1 Obtener la URL

En la página del proyecto en GitLab, pulsa el botón azul **Code** y copia la URL **Clone with SSH**. Tendrá este aspecto:

```
git@gitlab.com:usuario/mi-proyecto.git
```

### 4.2 Ir al escritorio

La carpeta del escritorio se llama `Escritorio` si Debian está en español y `Desktop` si está en inglés. Este comando funciona en ambos casos:

```bash
cd "$(xdg-user-dir DESKTOP)"
pwd
```

### 4.3 Clonar

```bash
git clone git@gitlab.com:usuario/mi-proyecto.git
```

Se creará una carpeta `mi-proyecto` con todo el código y el historial. Entra y comprueba:

```bash
cd mi-proyecto
git status
git branch -a
git log --oneline -5
```

Si quieres clonar directamente una rama concreta:

```bash
git clone -b nombre-rama git@gitlab.com:usuario/mi-proyecto.git
```

---

## 5. Abrir el proyecto en IntelliJ IDEA

### Opción A: desde IntelliJ (recomendada)

1. Abre IntelliJ IDEA.
2. En la pantalla de bienvenida pulsa **Open**.
3. Navega hasta el escritorio y selecciona la carpeta `mi-proyecto`.
4. Pulsa **OK** y elige **Trust Project** cuando lo pregunte.

> Si IntelliJ te detecta un `pom.xml` (Maven) o `build.gradle` (Gradle), abre ese archivo como proyecto. Espera a que termine de descargar las dependencias (barra de progreso abajo a la derecha).

### Opción B: clonar directamente desde IntelliJ

Si prefieres no usar la terminal:

1. En la pantalla de bienvenida pulsa **Get from VCS** (o menú **File → New → Project from Version Control**).
2. En **URL** pega la URL SSH o HTTPS del repositorio.
3. En **Directory** elige la carpeta del escritorio.
4. Pulsa **Clone**.

### Configurar el JDK

1. Ve a **File → Project Structure → Project**.
2. En **SDK** selecciona un JDK (por ejemplo 17 o 21, según lo que pida el proyecto). Si no tienes ninguno, pulsa **Add SDK → Download JDK**.
3. Ajusta el **Language level** al de tu versión.

### Comprobar que Git está activo

Abajo a la izquierda, en la barra inferior, debe aparecer el icono de Git con el nombre de la rama actual. Si no aparece, ve a **File → Settings → Version Control → Directory Mappings** y añade la carpeta del proyecto con VCS **Git**.

---

## 6. Ejecutar los tests

### Con JUnit desde el IDE

- **Un solo test:** abre la clase de test, pulsa la flecha verde junto al método o la clase y elige **Run**.
- **Todos los tests del proyecto:** clic derecho sobre la carpeta `src/test` → **Run 'All Tests'**.
- **Con cobertura:** clic derecho → **Run with Coverage**.
- Atajo para repetir el último test: `Shift + F10`.

El resultado aparece en la ventana **Run**: verde = pasa, rojo = falla (haz clic en el test para ver el error).

### Con Maven o Gradle desde terminal

```bash
# Maven
mvn test

# Gradle
./gradlew test
```

### Con Maven o Gradle desde IntelliJ

Abre la pestaña **Maven** (o **Gradle**) del lateral derecho y haz doble clic en `Lifecycle → test` (Maven) o `Tasks → verification → test` (Gradle).

---

## 7. Mini tutorial de Git en IntelliJ

IntelliJ integra Git en su interfaz. Estas son las ventanas y menús que más vas a usar:

| Qué quieres hacer | Dónde está |
|---|---|
| Ver cambios y hacer commit | Ventana **Commit** (`Ctrl + K`, o icono en la barra izquierda) |
| Hacer push | `Ctrl + Shift + K` o menú **Git → Push** |
| Hacer pull / actualizar | `Ctrl + T` o menú **Git → Update Project** |
| Ver ramas | Barra inferior (nombre de la rama) o widget de rama arriba |
| Ver historial | Ventana **Git** (abajo) → pestaña **Log** |
| Ver diferencias de un archivo | Clic derecho → **Git → Show Diff** (`Ctrl + D`) |
| Deshacer cambios de un archivo | Clic derecho → **Git → Rollback** |

### Colores de los archivos

- **Verde:** archivo nuevo añadido a Git.
- **Azul:** archivo modificado.
- **Rojo / marrón:** archivo sin añadir a Git (no versionado).
- **Gris:** archivo ignorado (`.gitignore`).

### 7.1 Hacer un commit

1. Modifica el código.
2. Abre la ventana **Commit** (`Ctrl + K`).
3. Marca los archivos que quieres incluir.
4. Escribe un mensaje claro en el cuadro de texto, por ejemplo: `Añade validación del email en el registro`.
5. Pulsa **Commit** (solo local) o **Commit and Push** (local + subir a GitLab).

> Los archivos nuevos que salen en rojo deben añadirse: clic derecho → **Git → Add**, o simplemente marcarlos en la ventana Commit.

Equivalente en terminal:

```bash
git add .
git commit -m "Añade validación del email en el registro"
```

### 7.2 Hacer push

1. Menú **Git → Push** (`Ctrl + Shift + K`).
2. Revisa los commits que se van a subir.
3. Pulsa **Push**.

Equivalente en terminal:

```bash
git push
```

Si es la primera vez que subes una rama nueva:

```bash
git push -u origin nombre-de-la-rama
```

### 7.3 Hacer pull (traer cambios de GitLab)

Antes de empezar a trabajar y antes de hacer push, actualiza siempre:

- IntelliJ: **Git → Pull…** o `Ctrl + T`.
- Terminal:

```bash
git pull
```

### 7.4 Crear una rama nueva

1. Pulsa sobre el nombre de la rama (barra inferior o widget superior).
2. Elige **New Branch**.
3. Escribe el nombre, por ejemplo `feature/login`.
4. Deja marcado **Checkout branch** y pulsa **Create**.

Equivalente en terminal:

```bash
git checkout -b feature/login
# o, en versiones modernas:
git switch -c feature/login
```

### 7.5 Cambiar de rama

- IntelliJ: clic en la rama → elige otra → **Checkout**.
- Terminal:

```bash
git switch main
```

> Haz commit (o *stash*) de tus cambios antes de cambiar de rama.

### 7.6 Fusionar una rama (merge)

Para traer a `main` el trabajo de `feature/login`:

1. Cambia a `main` (**Checkout**).
2. Haz **Pull** para actualizarla.
3. En el menú de ramas, selecciona `feature/login` → **Merge 'feature/login' into 'main'**.
4. Haz **Push**.

En terminal:

```bash
git switch main
git pull
git merge feature/login
git push
```

> En equipos es más habitual no hacer el merge a mano, sino crear un **Merge Request** en GitLab (ver el siguiente apartado).

### 7.7 Guardar cambios temporalmente (stash)

Útil si tienes cambios a medias y necesitas cambiar de rama:

- IntelliJ: **Git → Stash Changes…** y luego **Unstash Changes…**
- Terminal:

```bash
git stash
git stash pop
```

---

## 8. Flujo de trabajo completo con ramas

Este es el flujo recomendado para trabajar sin romper nada en `main`:

### Paso 1: actualiza `main`

```bash
git switch main
git pull
```

### Paso 2: crea una rama para tu tarea

```bash
git switch -c feature/nombre-de-la-tarea
```

### Paso 3: trabaja y ejecuta los tests

Programa tu cambio y comprueba que todos los tests pasan (apartado 6).

### Paso 4: commit

```bash
git add .
git commit -m "Descripción clara del cambio"
```

Haz commits pequeños y frecuentes, cada uno con un propósito concreto.

### Paso 5: sube la rama a GitLab

```bash
git push -u origin feature/nombre-de-la-tarea
```

### Paso 6: crea el Merge Request

Tras el push, la terminal muestra un enlace para crear el Merge Request. También puedes ir a GitLab → **Merge requests → New merge request**:

1. Elige como origen `feature/nombre-de-la-tarea` y como destino `main`.
2. Escribe un título y una descripción.
3. Asigna un revisor si procede.
4. Pulsa **Create merge request**.

Cuando se apruebe y se fusione, vuelve a tu máquina:

```bash
git switch main
git pull
git branch -d feature/nombre-de-la-tarea
```

---

## 9. Resolver conflictos

Un conflicto ocurre cuando dos personas modifican las mismas líneas. Git no sabe cuál conservar y te lo pide.

### En IntelliJ

1. Al hacer pull o merge aparece el diálogo **Conflicts**.
2. Pulsa **Merge**.
3. Se abren tres paneles: **tu versión** (izquierda), **resultado** (centro) y **la versión remota** (derecha).
4. Usa las flechas `>>` y `<<` para aceptar cambios de cada lado, o edita el panel central a mano.
5. Pulsa **Apply**.
6. Haz commit para finalizar el merge.

### En terminal

Los conflictos aparecen marcados en el archivo:

```
<<<<<<< HEAD
tu versión
=======
la versión del otro
>>>>>>> feature/otra-rama
```

Edita el archivo dejando solo el código correcto (borra las marcas `<<<<<<<`, `=======`, `>>>>>>>`) y después:

```bash
git add archivo-con-conflicto
git commit
```

---

## 10. Chuleta de comandos Git

| Comando | Qué hace |
|---|---|
| `git clone URL` | Descarga un repositorio |
| `git status` | Muestra el estado de los archivos |
| `git add archivo` / `git add .` | Prepara archivos para el commit |
| `git commit -m "mensaje"` | Guarda los cambios en el historial local |
| `git push` | Sube commits a GitLab |
| `git pull` | Descarga y fusiona cambios de GitLab |
| `git fetch` | Descarga cambios sin fusionarlos |
| `git branch` | Lista ramas locales |
| `git branch -a` | Lista ramas locales y remotas |
| `git switch rama` | Cambia de rama |
| `git switch -c rama` | Crea y cambia a una rama nueva |
| `git merge rama` | Fusiona una rama en la actual |
| `git log --oneline --graph` | Historial resumido con gráfico |
| `git diff` | Muestra cambios sin añadir |
| `git restore archivo` | Descarta cambios de un archivo |
| `git stash` / `git stash pop` | Guarda / recupera cambios temporales |
| `git branch -d rama` | Borra una rama local ya fusionada |
| `git remote -v` | Muestra las URLs remotas |

### Archivo `.gitignore`

Evita subir archivos que no deben versionarse. Para un proyecto Java con IntelliJ:

```
# IntelliJ
.idea/
*.iml
out/

# Maven
target/

# Gradle
.gradle/
build/

# Sistema
.DS_Store
```

> Si un archivo ya está versionado y quieres dejar de seguirlo: `git rm --cached archivo`.

### Buenas prácticas para los mensajes de commit

- Escribe en imperativo y de forma breve: `Corrige el cálculo del IVA`.
- Un commit = un cambio lógico.
- No subas código que no compila o con tests rotos.
- Nunca subas contraseñas, tokens ni archivos `.env`.

---

## 11. Problemas frecuentes

**`Permission denied (publickey)` al clonar o hacer push**
La clave SSH no está añadida a GitLab o no se está usando. Repite el apartado 3 y prueba con `ssh -T git@gitlab.com`.

**`fatal: not a git repository`**
Estás en una carpeta que no es el repositorio. Haz `cd` a la carpeta del proyecto.

**El push es rechazado (`rejected`, `non-fast-forward`)**
Hay commits en GitLab que no tienes en local. Haz `git pull`, resuelve conflictos si los hay, y vuelve a hacer push.

**IntelliJ no muestra la rama ni los botones de Git**
Comprueba **File → Settings → Version Control → Directory Mappings** y asegúrate de que la carpeta tiene VCS **Git**. Verifica también la ruta del ejecutable en **Settings → Version Control → Git** (normalmente `/usr/bin/git`).

**Los tests no se encuentran o no se ejecutan**
Marca `src/test/java` como **Test Sources Root** (clic derecho en la carpeta → **Mark Directory as**) y recarga el proyecto Maven/Gradle.

**Dependencias que no se descargan**
Pulsa el botón **Reload All Maven Projects** (o **Reload Gradle Project**) y revisa tu conexión y el JDK configurado.

**Quiero usar HTTPS en vez de SSH**
Clona con la URL HTTPS. GitLab pedirá un **Personal Access Token** en lugar de la contraseña (créalo en **Preferences → Access Tokens** con el permiso `write_repository`).

---

*¿Algo no funciona o falta algo? Abre un issue o propón un cambio en el repositorio de este blog.*
