# Runtime Spring Boot

Imagen Docker que [Vex](https://github.com/jairoprogramador/vex-client) utiliza como runtime para construir y desplegar proyectos Spring Boot generados a partir de sus plantillas.

## Herramientas incluidas

- OpenJDK
- Maven
- Azure CLI
- Terraform
- kubectl
- kubelogin
- Git

## Uso local

### Construir la imagen

```bash
docker build -t vex-runtime-springboot .
```

En Linux, para evitar problemas de permisos con volúmenes montados:

```bash
docker build --build-arg DEV_UID=$(id -u) --build-arg DEV_GID=$(id -g) -t vex-runtime-springboot .
```

### Ejecutar un contenedor

```bash
docker run --rm -it -v "$(pwd):/home/vex/app" vex-runtime-springboot <comando>
```

Reemplaza `<comando>` con cualquier comando de `vexc` (por ejemplo `test`, `deploy`). Sin argumentos, muestra la versión. 

> **Nota:** Estos comandos son solo para usar el Dockerfile de forma independiente. Si utilizas [Vex](https://github.com/jairoprogramador/vex-client), esto no es necesario ya que se gestiona automáticamente.

## Contribuir

1. Haz un fork del repositorio
2. Crea una rama con tu cambio (`git checkout -b mi-cambio`)
3. Haz commit de tus cambios
4. Abre un Pull Request hacia `main`
