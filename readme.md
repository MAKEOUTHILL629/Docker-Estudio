# Docker: De 0 a deployment

Docker es una plataforma que permite desarrollar, enviar y ejecutar aplicaciones dentro de contenedores. Se basa en dos tecnologías existentes en Linux:

- **Namespaces**: Controlan qué puede ver un proceso dentro del sistema, como el listado de procesos o el sistema de archivos.
- **CGroups**: Limitan la cantidad de recursos (CPU, memoria, etc.) que un proceso puede utilizar.

## Comandos Básicos de Docker

### Visualización de Logs

- Para listar los logs de un contenedor:

  ```bash
  docker logs [ID-CONTENEDOR]
  ```

- Ver logs en tiempo real:

  ```bash
  docker logs -f [ID-CONTENEDOR]
  ```

- Detener un contenedor:

  ```bash
  docker stop [ID-CONTENEDOR]
  ```

- Iniciar un contenedor detenido:

  ```bash
  docker start [ID-CONTENEDOR]
  ```

- Eliminar todos los contenedores:
  ```bash
  docker rm $(docker ps -a -q)
  ```

### Comandos Adicionales

- Listar contenedores activos:
  ```bash
  docker ps
  ```
- Listar todos los contenedores:
  ```bash
  docker ps -a
  ```
- Crear y ejecutar un contenedor:

  ```bash
  docker run [OPCIONES] [IMAGEN]
  ```

- Eliminar una imagen:
  ```bash
  docker rmi [ID-IMAGEN]
  ```
- Acceder a un contenedor en ejecución:
  ```bash
  docker exec -it [ID-CONTENEDOR] /bin/bash
  ```

### Gestion de imagenes

- Listar imágenes descargadas:
  ```bash
  docker images
  ```
- Construir una imagen desde un Dockerfile:
  ```bash
  docker build -t [NOMBRE-IMAGEN] .
  ```

### Redes y Volúmenes

- Crear una red de Docker:
  ```bash
  docker network create [NOMBRE-RED]
  ```
- Listar todas las redes:
  ```bash
  docker network ls
  ```
- Crear un volumen:
  ```bash
  docker volume create [NOMBRE-VOLUMEN]
  ```
- Listar todos los volúmenes:
  ```bash
  docker volume ls
  ```

### Trucos Útiles

- Eliminar imágenes no utilizadas:
  ```bash
  docker image prune
  ```
- Eliminar todos los contenedores detenidos:
  ```bash
  docker container prune
  ```
- Eliminar todos los volúmenes no utilizados:
  ```bash
  docker volume prune
  ```
- Ver logs en tiempo real:

  ```bash
  docker logs -f [ID-CONTENEDOR]

  ```

- Eliminar todos los recursos no utilizados (contenedores, redes, imágenes y volúmenes):
  ```bash
  docker system prune
  ```
- Ver uso de espacio en disco por Docker:
  ```bash
  docker system df
  ```
- Renombrar un contenedor:
  ```bash
  docker rename [ID-CONTENEDOR] [NUEVO-NOMBRE]
  ```
- Copiar archivos desde/hacia un contenedor:
  ```bash
  docker cp [CONTENEDOR]:/ruta/origen /ruta/destino
  ```

## Docker File

# Instrucciones del Dockerfile

Un archivo Dockerfile es un script de texto que contiene una serie de instrucciones para construir una imagen de Docker. Aquí tienes una explicación de las instrucciones más comunes:

1. **FROM**:

   - Define la imagen base desde la cual se construirá la nueva imagen.
   - Ejemplo: `FROM ubuntu:latest` utiliza la última versión de Ubuntu como base.

2. **MAINTAINER** (obsoleto, usa LABEL):

   - Indica quién es el autor o mantenedor de la imagen.
   - Ejemplo: `LABEL maintainer="tu_email@example.com"`

3. **RUN**:

   - Ejecuta comandos en la imagen durante el proceso de construcción.
   - Ejemplo: `RUN apt-get update` actualiza el índice de paquetes en una imagen basada en Ubuntu o Debian.

4. **CMD**:

   - Especifica el comando por defecto que se ejecutará cuando se inicie un contenedor a partir de la imagen.
   - Ejemplo: `CMD ["echo", "Hello, World!"]` ejecuta el comando `echo` cuando se inicia el contenedor.

5. **LABEL**:

   - Añade metadatos a la imagen, como información sobre el autor o la versión.
   - Ejemplo: `LABEL version="1.0"`

6. **EXPOSE**:

   - Indica que el contenedor escucha en un puerto específico en tiempo de ejecución.
   - Ejemplo: `EXPOSE 80` indica que el contenedor escucha en el puerto 80.

7. **ENV**:

   - Establece variables de entorno dentro de la imagen.
   - Ejemplo: `ENV PATH=/usr/local/bin:$PATH` añade un directorio al PATH.

8. **ADD**:

   - Copia archivos o directorios desde el host al sistema de archivos de la imagen. También puede descomprimir archivos tar.
   - Ejemplo: `ADD archivo.tar.gz /app` copia y descomprime `archivo.tar.gz` en el directorio `/app`.

9. **COPY**:

   - Similar a ADD, pero solo copia archivos o directorios. No descomprime archivos.
   - Ejemplo: `COPY . /app` copia el contenido del directorio actual a `/app` en la imagen.

10. **ENTRYPOINT**:

    - Configura un contenedor para que se ejecute como un ejecutable. Se puede combinar con CMD.
    - Ejemplo: `ENTRYPOINT ["python", "app.py"]` ejecuta `python app.py` al iniciar el contenedor.

11. **VOLUME**:

    - Crea un punto de montaje para volúmenes externos, permitiendo que los datos persistan.
    - Ejemplo: `VOLUME /data` crea un volumen en `/data`.

12. **USER**:

    - Especifica el usuario que ejecutará los comandos dentro del contenedor.
    - Ejemplo: `USER appuser` cambia al usuario `appuser`.

13. **WORKDIR**:

    - Establece el directorio de trabajo para las instrucciones RUN, CMD, ENTRYPOINT, COPY y ADD.
    - Ejemplo: `WORKDIR /app` cambia al directorio `/app`.

14. **ARG**:
    - Define variables que se pueden pasar en tiempo de construcción.
    - Ejemplo: `ARG app_version=1.0` define una variable `app_version`.

Cada instrucción tiene su propósito específico y se utilizan en conjunto para definir cómo se debe construir y configurar una imagen de Docker.

## Diferencias entre CMD y ENTRYPOINT en Dockerfile

En un Dockerfile, las instrucciones `CMD` y `ENTRYPOINT` se utilizan para especificar los comandos que se ejecutarán cuando un contenedor se inicie a partir de la imagen. Aunque pueden parecer similares, tienen diferencias importantes:

### CMD

- **Propósito**: Define el comando por defecto que se ejecutará cuando el contenedor se inicie.
- **Sobrescribible**: Los comandos especificados por `CMD` pueden ser sobrescritos al ejecutar el contenedor con argumentos adicionales.
- **Formato**: Puede especificarse en forma de lista (exec form) o cadena de texto (shell form).
  - **Exec form**: `CMD ["executable", "param1", "param2"]`
  - **Shell form**: `CMD command param1 param2`
- **Uso común**: Se utiliza cuando se desea proporcionar un comando por defecto que el usuario pueda cambiar fácilmente.

### ENTRYPOINT

- **Propósito**: Configura un contenedor para que se ejecute como un ejecutable específico.
- **No sobrescribible**: Los comandos especificados por `ENTRYPOINT` no son fácilmente sobrescribibles mediante argumentos al ejecutar el contenedor, aunque se pueden añadir argumentos adicionales.
- **Formato**: Generalmente se especifica en forma de lista (exec form).
  - **Exec form**: `ENTRYPOINT ["executable", "param1", "param2"]`
- **Uso común**: Se utiliza cuando se desea que el contenedor se comporte como un ejecutable específico y no se desee que el usuario cambie el comando principal.

### Combinación de CMD y ENTRYPOINT

- **Uso conjunto**: `ENTRYPOINT` y `CMD` pueden usarse juntos. En este caso, `CMD` se utiliza para proporcionar argumentos por defecto para el `ENTRYPOINT`.
  - Ejemplo:
    ```dockerfile
    ENTRYPOINT ["python"]
    CMD ["app.py"]
    ```
  - En este ejemplo, `python` es el ejecutable y `app.py` es el argumento por defecto, que puede ser sobrescrito al ejecutar el contenedor.

En resumen, `CMD` es más flexible y se utiliza para comandos por defecto, mientras que `ENTRYPOINT` se utiliza para definir el comportamiento principal del contenedor.

## Multi-Stage Builds en Dockerfiles

Los **Multi-Stage Builds** son una técnica en Docker que permite reducir el tamaño de las imágenes y mejorar la eficiencia del proceso de construcción. Se utilizan múltiples etapas en un Dockerfile para separar el proceso de construcción en diferentes fases, lo que permite copiar solo los artefactos necesarios en la imagen final.

## ¿Cómo funcionan?

1. **Definición de múltiples etapas**: Cada etapa comienza con una instrucción `FROM`, y cada una puede usar una imagen base diferente.
2. **Construcción de artefactos**: Las etapas iniciales se utilizan para compilar y construir los artefactos necesarios, como binarios o archivos ejecutables.
3. **Copia de artefactos**: Solo los artefactos necesarios se copian a la etapa final, lo que resulta en una imagen más pequeña y limpia.
4. **Optimización de recursos**: Al no incluir herramientas de compilación y dependencias en la imagen final, se reduce el tamaño y se mejora la seguridad.

## Ejemplo de uso

```dockerfile
# Primera etapa: Compilación
FROM golang:1.16 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Segunda etapa: Imagen final
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

#### Explicación del ejemplo

- Primera etapa (builder):
  - Usa una imagen de Go para compilar una aplicación.
  - Compila el binario myapp usando el comando go build.
- Segunda etapa:
  - Usa una imagen base más ligera (alpine).
  - Copia solo el binario myapp desde la etapa builder.
  - Define el comando por defecto para ejecutar el binario.

#### Ventajas de Multi-Stage Builds

Reducción del tamaño de la imagen: Al incluir solo los archivos necesarios, se obtienen imágenes más pequeñas.
Mejora de la seguridad: Menos herramientas y librerías en la imagen final reducen la superficie de ataque.
Facilidad de mantenimiento: Separar las etapas de construcción y ejecución facilita la gestión del Dockerfile.
Los Multi-Stage Builds son especialmente útiles en aplicaciones que requieren compilación o procesamiento previo antes de ser ejecutadas.

## Volúmenes en Docker

Los volúmenes en Docker se utilizan para persistir datos, compartiendo datos entre contenedores o entre el contenedor y el host. Son una parte esencial para gestionar datos en contenedores de manera eficiente.

## ¿Qué son los volúmenes?

- **Persistencia de datos**: Permiten que los datos sobrevivan a la eliminación de un contenedor.
- **Compartición de datos**: Facilitan el intercambio de datos entre múltiples contenedores.
- **Gestión de datos**: Separan el ciclo de vida de los datos del ciclo de vida del contenedor.

## Tipos de volúmenes

1. **Volúmenes gestionados por Docker**:

   - Se crean y gestionan por Docker.
   - Se almacenan en un directorio específico en el host.
   - Ejemplo de creación: `docker volume create my_volume`

2. **Montajes de bind**:
   - Vinculan un directorio o archivo específico del host al contenedor.
   - Más flexibles pero requieren especificar rutas absolutas.
   - Ejemplo de uso: `-v /host/path:/container/path`

## Ejemplos de uso

### Crear y usar un volumen gestionado por Docker

```bash
# Crear un volumen
docker volume create my_volume

# Usar el volumen en un contenedor
docker run -d -v my_volume:/app/data my_image



# Usar un montaje de bind

# Vincular un directorio del host al contenedor

docker run -d -v /host/data:/container/data my_image

# Dockerfile con volúmenes
# Define un punto de montaje para un volumen
VOLUME /app/data

```

## Docker Compose

Docker Compose es una herramienta para definir y ejecutar aplicaciones Docker multi-contenedor. Utiliza un archivo YAML para configurar los servicios de la aplicación, lo que permite gestionar fácilmente aplicaciones complejas.

## ¿Qué es Docker Compose?

- **Definición de servicios**: Permite definir múltiples servicios en un solo archivo `docker-compose.yml`.
- **Automatización**: Facilita la construcción, el despliegue y la gestión de aplicaciones multi-contenedor.
- **Orquestación**: Coordina la ejecución de múltiples contenedores como una sola aplicación.

## Estructura básica de un archivo `docker-compose.yml`

```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### Comandos más importantes

- docker-compose up: Inicia y ejecuta todos los servicios definidos en el archivo docker-compose.yml.
  ```bash
  docker-compose up
  ```
- docker-compose down: Detiene y elimina los contenedores, redes y volúmenes creados por up.
  ```bash
  docker-compose down
  ```
- docker-compose build: Construye o reconstruye los servicios.
  ```bash
  docker-compose build
  ```
- docker-compose pull: Descarga las imágenes para los servicios definidos.
  ```bash
  docker-compose pull
  ```
- docker-compose logs: Muestra los logs de los servicios.
  ```bash
  docker-compose logs
  ```
- docker-compose exec: Ejecuta un comando en un contenedor en ejecución.

  ```bash
  docker-compose exec web bash
  ```

- docker-compose scale: Escala un servicio a un número específico de contenedores (en versiones anteriores a la 3.x).
  ```bash
  docker-compose scale web=3
  ```

### Trucos y consejos

- Variables de entorno: Utiliza un archivo .env para configurar variables de entorno que se pueden referenciar en docker-compose.yml.
- Volúmenes: Define volúmenes en docker-compose.yml para persistir datos.

  ```yaml
  volumes:
    - db_/var/lib/mysql
  ```

- Redes personalizadas: Define redes para aislar y conectar servicios.
  ```yaml
  networks:
    my_network:
  ```
- Reinicio automático: Configura políticas de reinicio para mantener los contenedores en funcionamiento.
  ```yaml
  restart: always
  ```
- Anulación de configuración: Usa múltiples archivos docker-compose.override.yml para anular configuraciones específicas en diferentes entornos.

### Ejemplo de archivo docker-compose.yaml

# Archivo `docker-compose.yml`

Un archivo `docker-compose.yml` es un archivo de configuración en formato YAML que define los servicios, redes y volúmenes necesarios para ejecutar una aplicación multi-contenedor con Docker Compose.

## Ejemplo de `docker-compose.yml`

```yaml
version: '3.8'  # Especifica la versión de Docker Compose a utilizar

services:  # Define los servicios de la aplicación
  web:  # Nombre del servicio
    image: nginx  # Imagen de Docker a utilizar
    ports:
      - "80:80"  # Mapea el puerto 80 del host al puerto 80 del contenedor
    volumes:
      - ./webdata:/usr/share/nginx/html  # Monta un volumen desde el host al contenedor
    networks:
      - frontend  # Conecta el servicio a la red 'frontend'

  db:  # Otro servicio llamado 'db'
    image: mysql:5.7  # Imagen de Docker específica para MySQL
    environment:  # Variables de entorno para el contenedor
      MYSQL_ROOT_PASSWORD: example  # Establece la contraseña del root de MySQL
    volumes:
      - db_data:/var/lib/mysql  # Volumen para persistir datos de la base de datos
    networks:
      - backend  # Conecta el servicio a la red 'backend'

volumes:  # Define volúmenes para la persistencia de datos
  db_data:

networks:  # Define redes para la comunicación entre servicios
  frontend:
  backend:
```

### Explicación por linea

- version: '3.8': Define la versión del archivo de configuración de Docker Compose que se está utilizando. Esto determina las características disponibles y cómo se interpretan las configuraciones.
- services:: Agrupa todos los servicios que forman parte de la aplicación. Cada servicio se ejecuta en su propio contenedor.
- web:: Define un servicio llamado web.
  - image: nginx: Especifica que el servicio web utilizará la imagen nginx.
  - ports:: Mapea puertos del contenedor al host. Aquí, el puerto 80 del contenedor se expone como el puerto 80 en el host.
  - volumes:: Monta un volumen, permitiendo que el directorio ./webdata en el host se sincronice con /usr/share/nginx/html en el contenedor.
  - networks:: Conecta el servicio a una red llamada frontend.
- db:: Define un servicio llamado db.
  - image: mysql:5.7: Utiliza la imagen mysql versión 5.7.
  - environment:: Define variables de entorno, como MYSQL_ROOT_PASSWORD, para configurar el contenedor.
  - volumes:: Usa un volumen llamado db_data para persistir datos en /var/lib/mysql.
  - networks:: Conecta el servicio a una red llamada backend.
- volumes:: Define volúmenes nombrados, en este caso db_data, que se utilizan para persistir datos más allá del ciclo de vida de los contenedores.
- networks:: Define redes personalizadas, frontend y backend, que permiten la comunicación entre los servicios.

## Redes en Docker

Las redes en Docker permiten que los contenedores se comuniquen entre sí y con el mundo exterior. Docker proporciona varias opciones de red para facilitar la configuración y gestión de la conectividad de los contenedores.

## Tipos de redes en Docker

1. **Bridge (puente)**:
   - Es la red por defecto para contenedores en un host único.
   - Permite que los contenedores en el mismo host se comuniquen.
   - Los contenedores pueden conectarse a la red usando el nombre del contenedor como un alias DNS.

2. **Host**:
   - El contenedor comparte la pila de red del host.
   - No hay aislamiento de red entre el contenedor y el host.
   - Útil para situaciones que requieren un alto rendimiento de red.

3. **Overlay**:
   - Permite la comunicación entre contenedores en diferentes hosts.
   - Utilizado en entornos de Docker Swarm o Kubernetes.
   - Facilita la creación de redes distribuidas.

4. **None**:
   - Desconecta el contenedor de cualquier red.
   - Útil para contenedores que no necesitan conectividad de red.

5. **Macvlan**:
   - Asigna una dirección MAC a cada contenedor, como si fueran dispositivos de red físicos.
   - Útil para integrarse con redes físicas y configuraciones avanzadas.

## Ejemplo de uso de redes en Docker Compose

```yaml
version: '3.8'
services:
  app:
    image: my_app_image
    networks:
      - frontend
      - backend

  db:
    image: mysql
    networks:
      - backend

networks:
  frontend:
  backend:
```

### Explicación del ejemplo

- services:: Define los servicios de la aplicación, cada uno con su configuración de red.

- app:: Un servicio que se conecta a dos redes: frontend y backend.
  - Esto permite que app se comunique con otros servicios en ambas redes.
- db:: Un servicio que solo se conecta a la red backend.
  - Esto aísla db de la red frontend, limitando la comunicación solo a los servicios en backend.
- networks:: Define las redes frontend y backend.
  - Se crean automáticamente si no existen.


### Comandos importantes de red en Docker

- Crear una red:
  ```bash
  docker network create my_network
  ```
- Listar redes:
  ```bash
  docker network ls
  ```
- Inspeccionar una red:
  ```bash
  docker network inspect my_network
  ```
- Conectar un contenedor a una red:
  ```bash
  docker network connect my_network my_container
  ```
- Desconectar un contenedor de una red:
  ```bash
  docker network disconnect my_network my_container
  ```

  
