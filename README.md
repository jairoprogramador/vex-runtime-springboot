# Runtime de FastDeploy para Java y Azure

Este proyecto proporciona una imagen Docker que sirve como un entorno de ejecución (`runtime`) para `fastdeploy`. Esta imagen está diseñada para automatizar despliegues de aplicaciones Java en Microsoft Azure, e incluye todas las herramientas y configuraciones necesarias para un flujo de trabajo de CI/CD moderno.

## ¿Qué es FastDeploy?

`fastdeploy` es una herramienta de línea de comandos (CLI) que simplifica y automatiza el proceso de despliegue de aplicaciones. Este runtime Docker empaqueta `fastdeploy` junto con todas sus dependencias para proporcionar un entorno consistente y reproducible.

## Características

La imagen Docker incluye el siguiente software preinstalado:

- **Base:** Ubuntu 22.04
- **Java:** OpenJDK 17
- **Build:** Apache Maven
- **Cloud:** Azure CLI
- **Contenedores:** Cliente de Docker
- **Infraestructura como Código:** Terraform
- **Kubernetes:** `kubectl` y `kubelogin`
- **Herramientas:** Git, curl, unzip, wget

## Requisitos

- [Docker](https://docs.docker.com/get-docker/) instalado en tu sistema.

## Construcción de la Imagen

Para construir la imagen Docker en tu máquina local, clona este repositorio y ejecuta el siguiente comando en la raíz del proyecto:

```bash
docker build -t fastdeploy .
```

### Nota para usuarios de Linux

Para evitar problemas de permisos con los volúmenes montados, puedes pasar el GID (Group ID) de tu usuario del anfitrión durante la construcción de la imagen. Esto asegura que el usuario dentro del contenedor tenga los mismos permisos de grupo.

```bash
docker build --build-arg DEV_GID=$(id -g) -t fastdeploy .
```

## Uso

El `ENTRYPOINT` de la imagen es el ejecutable `fd` de `fastdeploy`. Por defecto, si no se especifican comandos, ejecutará `fd --version`.

### Ejecución del Contenedor

Para ejecutar un contenedor, puedes usar el siguiente comando. Se recomienda montar el directorio de tu aplicación en `/home/fastdeploy/app` dentro del contenedor, que es el directorio de trabajo por defecto.

```bash
docker run --rm -it -v "/ruta/a/tu/app:/home/fastdeploy/app" fastdeploy <comando>
```

- Reemplaza `/ruta/a/tu/app` con la ruta absoluta al directorio de tu proyecto en tu máquina.
- Reemplaza `<comando>` con el comando de `fastdeploy` que deseas ejecutar (por ejemplo, `test`, `supply`, `deploy`).

### Ejemplo: Ejecutar un despliegue

```bash
docker run --rm -it \
  -v "/path/to/my/java-app:/home/fastdeploy/app" \
  -v "$HOME/.azure:/home/fastdeploy/.azure" \
  -e "FASTDEPLOY_HOME=/home/fastdeploy/.fd" \
  fastdeploy deploy
```

**Desglose del comando:**

- `-v "/path/to/my/java-app:/home/fastdeploy/app"`: Monta el código fuente de tu aplicación en el directorio de trabajo del contenedor.
- `-v "$HOME/.azure:/home/fastdeploy/.azure"`: Monta tu configuración de Azure CLI para que no necesites iniciar sesión cada vez.
- `-e "FASTDEPLOY_HOME=/home/fastdeploy/.fd"`: Configura el directorio home de `fastdeploy` dentro del contenedor para almacenar configuraciones o caché.

### Acceder al Shell del Contenedor

Para obtener una sesión de shell interactiva dentro del contenedor y usar las herramientas instaladas:

```bash
docker run --rm -it -v "/ruta/a/tu/app:/home/fastdeploy/app" fastdeploy test
```

Esto te dará acceso a un shell `bash` dentro del contenedor, donde puedes ejecutar `mvn`, `az`, `kubectl`, etc. directamente.
