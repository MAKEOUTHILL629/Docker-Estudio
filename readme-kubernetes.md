# Kubernetes

Kubernetes nos provee con herramientas para automatizar la distribución de aplicaciones a un cluster de servidores, asegurando una utilización más eficiente del hardware, comparada con la que podemos conseguir de manera tradicional.

Kubernetes se coloca delante de un conjunto de servidores para que nosotros los veamos como un solo servidor, aunque detrás pueda haber decenas o miles de servidores.

Para conseguir esto, Kubernetes expone una API Restful que podemos utilizar para desplegar de forma sencilla nuestra aplicación, y que hará que el propio sistema se ocupe de arreglar fallos en servidores, de la monitorización de la aplicación, etc. En vez de desplegar nuestra aplicación a todos los servidores, solo tendremos que decidir cuantas instancias de la aplicación deben ejecutarse al mismo tiempo, y Kubernetes se encargará de que eso se cumpla.

La API de Kubernetes nos permite modelar todo el ciclo de vida de las aplicaciones que se ejecutarán en el cluster de servidores. Es una API Restful a la que podemos enviar peticiones describiendo cómo queremos desplegar. Podemos utilizar cualquier cliente HTTP para enviarle peticiones (curl, wget, Postman...), pero el propio Kubernetes trae una herramienta de línea de comandos para facilitar la interacción con la API. Se llama kubectl.

## Hello Word con Kubernetes

Para decirle a Kubernetes que despliegue nuestra aplicación, vamos a ejecutar este comando en nuestra terminal

```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest --port=80
```

listar los pods que se estan ejecutando

```bash
kubectl get pods
```

Para exponer esta aplicación en mi pc, y así poder enviarle peticiones para ver que todo funciona correctamente, nos hace falta este comando

```bash
kubectl port-forward hello-world 8080:80
```

## Pods

Un **Pod** es la unidad básica de ejecución en Kubernetes. Representa una instancia de una aplicación o conjunto de aplicaciones que comparten recursos. Aquí tienes una explicación detallada:

### Características de un Pod

1. **Unidad de Despliegue**:

   - Un Pod es la unidad más pequeña y sencilla que se puede crear y gestionar en Kubernetes.

2. **Contenedores**:

   - Un Pod puede contener uno o varios contenedores que comparten el mismo espacio de red y almacenamiento.

3. **Red Compartida**:

   - Los contenedores dentro de un Pod comparten la misma dirección IP y puerto. Pueden comunicarse entre sí usando `localhost`.

4. **Almacenamiento Compartido**:

   - Los Pods pueden tener volúmenes compartidos que permiten a los contenedores acceder a un sistema de archivos común.

5. **Ciclo de Vida**:

   - Los Pods tienen un ciclo de vida definido, desde su creación hasta su terminación.

6. **Escalabilidad**:
   - Aunque un Pod puede contener múltiples contenedores, generalmente se recomienda tener un solo contenedor por Pod para facilitar la escalabilidad y gestión.

## Uso de Pods

- **Implementación de Aplicaciones**:
  - Los Pods se utilizan para desplegar aplicaciones en un clúster de Kubernetes.
- **Agrupación de Contenedores**:

  - Permiten agrupar contenedores que deben ejecutarse juntos y compartir recursos.

- **Gestión de Estado**:
  - Kubernetes gestiona el estado deseado de los Pods, asegurando que la cantidad correcta esté en ejecución.

## Ejemplo de Pod

Aquí tienes un ejemplo básico de un Pod en YAML:

```yaml
# archivo pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: mi-pod
spec:
  containers:
    - name: mi-contenedor
      image: nginx
```

EJemplo de creacion de un pod por consola

```bash
kubectl create -f .\pod.yaml
```

Simular la creacion de un pod para tenerlo en un archivo yaml

```sh
kubectl run hello-world --image=fiunchinho/codely-docker:latest --restart=Never --port=80 --dry-run=client -o yaml > pod.yaml
```

Obtener información de un pod

```sh
kubectl describe pod hello-world
```

Salida del anterior comando

### Descripción del Pod `hello-world`

La salida del comando `kubectl describe pod hello-world` proporciona un resumen detallado del estado y configuración del Pod llamado `hello-world`. Aquí tienes una explicación paso a paso:

#### Información General

- **Name**: `hello-world`

  - Nombre del Pod.

- **Namespace**: `default`

  - Espacio de nombres en el que se encuentra el Pod.

- **Priority**: `0`

  - Prioridad del Pod (predeterminada en este caso).

- **Service Account**: `default`

  - Cuenta de servicio utilizada por el Pod.

- **Node**: `makeouthill/192.168.127.2`

  - Nodo en el que está ejecutándose el Pod, junto con su dirección IP.

- **Start Time**: `Mon, 14 Oct 2024 11:10:28 -0500`

  - Fecha y hora en que se inició el Pod.

- **Labels**: `run=hello-world`

  - Etiquetas asignadas al Pod para facilitar su identificación y gestión.

- **Annotations**: `<none>`

  - Anotaciones adicionales (ninguna en este caso).

- **Status**: `Running`

  - Estado actual del Pod.

- **IP**: `10.42.0.21`
  - Dirección IP asignada al Pod.

## Información de Contenedores

- **Container Name**: `hello-world`

  - Nombre del contenedor dentro del Pod.

- **Container ID**: `docker://2b100427e875...`

  - Identificador único del contenedor.

- **Image**: `fiunchinho/codely-docker:latest`

  - Imagen de Docker utilizada por el contenedor.

- **Image ID**: `docker-pullable://fiunchinho/codely-docker@sha256:...`

  - Identificador de la imagen específica.

- **Port**: `80/TCP`

  - Puerto en el que escucha el contenedor.

- **State**: `Running`

  - Estado actual del contenedor.

  - **Started**: `Mon, 14 Oct 2024 11:10:29 -0500`
    - Fecha y hora en que se inició el contenedor.

- **Ready**: `True`

  - Indica si el contenedor está listo para recibir tráfico.

- **Restart Count**: `0`

  - Número de veces que el contenedor ha sido reiniciado.

- **Environment**: `<none>`

  - Variables de entorno (ninguna en este caso).

- **Mounts**:
  - **/var/run/secrets/kubernetes.io/serviceaccount**: Montaje de volumen para acceso a la API de Kubernetes.

## Condiciones del Pod

- **Initialized**: `True`

  - El Pod ha sido inicializado correctamente.

- **Ready**: `True`

  - El Pod está listo para recibir tráfico.

- **ContainersReady**: `True`

  - Todos los contenedores dentro del Pod están listos.

- **PodScheduled**: `True`
  - El Pod ha sido programado en un nodo.

## Volúmenes

- **kube-api-access-gqpl9**:
  - Tipo: `Projected`
  - Volumen que contiene datos inyectados desde múltiples fuentes, como ConfigMaps y tokens.

## Clase de Calidad de Servicio

- **QoS Class**: `BestEffort`
  - Clase de calidad de servicio del Pod, indicando que no se han solicitado recursos específicos.

## Tolerancias

- **Tolerations**:
  - Permiten que el Pod tolere ciertas condiciones del nodo, como no estar listo o ser inalcanzable, durante un tiempo específico.

## Eventos

- **Scheduled**: El Pod fue asignado exitosamente al nodo `makeouthill`.
- **Pulling**: Se inició la descarga de la imagen `fiunchinho/codely-docker:latest`.
- **Pulled**: La imagen fue descargada exitosamente.
- **Created**: El contenedor `hello-world` fue creado.
- **Started**: El contenedor `hello-world` fue iniciado.

Esta descripción proporciona una visión completa del estado y configuración del Pod `hello-world` en Kubernetes.

## Comandos Kubectl

Comandos y Trucos de `kubectl` para Kubernetes

| Descripción                                                           | Comando                                                   | Ejemplo de Uso                                        |
| --------------------------------------------------------------------- | --------------------------------------------------------- | ----------------------------------------------------- |
| Obtener una lista de todos los Pods en todos los espacios de nombres. | `kubectl get pods --all-namespaces`                       | `kubectl get pods --all-namespaces`                   |
| Ver detalles completos de un Pod específico.                          | `kubectl describe pod <pod-name>`                         | `kubectl describe pod my-pod`                         |
| Aplicar configuraciones desde un archivo YAML o JSON.                 | `kubectl apply -f <file>`                                 | `kubectl apply -f deployment.yaml`                    |
| Eliminar un recurso especificado en un archivo de configuración.      | `kubectl delete -f <file>`                                | `kubectl delete -f service.yaml`                      |
| Escalar un despliegue a un número específico de réplicas.             | `kubectl scale deployment <name> --replicas=<num>`        | `kubectl scale deployment my-deployment --replicas=3` |
| Ver los registros de un contenedor dentro de un Pod.                  | `kubectl logs <pod-name> -c <container-name>`             | `kubectl logs my-pod -c my-container`                 |
| Ejecutar un comando en un contenedor dentro de un Pod.                | `kubectl exec <pod-name> -- <command>`                    | `kubectl exec my-pod -- ls /app`                      |
| Obtener la configuración del clúster y contexto actual.               | `kubectl config view`                                     | `kubectl config view`                                 |
| Cambiar el contexto actual del clúster.                               | `kubectl config use-context <context>`                    | `kubectl config use-context my-cluster`               |
| Reenviar un puerto desde un Pod a tu máquina local.                   | `kubectl port-forward <pod-name> <local-port>:<pod-port>` | `kubectl port-forward my-pod 8080:80`                 |
| Ver el estado de un despliegue.                                       | `kubectl rollout status deployment/<name>`                | `kubectl rollout status deployment/my-deployment`     |
| Verificar qué recursos están utilizando más CPU o memoria.            | `kubectl top pod`                                         | `kubectl top pod`                                     |
| Obtener eventos recientes en el clúster.                              | `kubectl get events`                                      | `kubectl get events`                                  |
| Ver la versión del cliente y del servidor de Kubernetes.              | `kubectl version`                                         | `kubectl version`                                     |
| Obtener una lista de todos los espacios de nombres.                   | `kubectl get namespaces`                                  | `kubectl get namespaces`                              |
| Crear un nuevo espacio de nombres.                                    | `kubectl create namespace <name>`                         | `kubectl create namespace my-namespace`               |
| Aplicar cambios de forma interactiva a recursos en ejecución.         | `kubectl edit <resource>/<name>`                          | `kubectl edit deployment/my-deployment`               |
| Obtener la lista de nodos en el clúster.                              | `kubectl get nodes`                                       | `kubectl get nodes`                                   |
| Obtener información detallada sobre un nodo específico.               | `kubectl describe node <node-name>`                       | `kubectl describe node my-node`                       |

Esta tabla ofrece una visión general de algunos de los comandos más útiles y trucos para trabajar con `kubectl` en Kubernetes.

## Recursos

Al definir un Pod en Kubernetes, es crucial especificar los recursos que cada contenedor dentro del Pod puede utilizar. Esto incluye CPU y memoria. Aquí te explico cómo funcionan:

### Tipos de Recursos

1. **Requests (Solicitudes)**:

   - Representan la cantidad mínima de recursos garantizados para el contenedor.
   - Kubernetes utiliza estas solicitudes para decidir en qué nodo programar el Pod.

2. **Limits (Límites)**:
   - Indican el máximo de recursos que un contenedor puede utilizar.
   - Si un contenedor intenta usar más recursos de los especificados en los límites, Kubernetes puede restringir su uso.

### Especificación de Recursos

Los recursos se definen en la sección `resources` del manifiesto del contenedor dentro de un Pod. Aquí tienes un ejemplo en YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mi-pod
spec:
  containers:
    - name: mi-contenedor
      image: nginx
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
```

## Health Checks Automáticos en Kubernetes

Kubernetes proporciona mecanismos para verificar automáticamente la salud de las aplicaciones mediante **liveness probes** y **readiness probes**. Estos checks aseguran que las aplicaciones estén funcionando correctamente y listas para recibir tráfico.

### Tipos de Probes

1. **Liveness Probe**:

   - Verifica si la aplicación está viva y funcionando.
   - Si la liveness probe falla, Kubernetes reinicia el contenedor.
   - Útil para detectar deadlocks o situaciones en las que la aplicación está colgada.

2. **Readiness Probe**:
   - Indica si la aplicación está lista para recibir tráfico.
   - Si la readiness probe falla, el Pod se desmarca del servicio y no recibe tráfico.
   - Útil para asegurar que el contenedor esté completamente inicializado antes de recibir solicitudes.

### Métodos de Probes

Kubernetes soporta varios métodos para realizar health checks:

- **HTTP GET**:

  - Realiza una solicitud HTTP GET a un endpoint específico del contenedor.
  - Ejemplo:

    ```yaml
    httpGet:
      path: /health
      port: 8080
    ```

- **TCP Socket**:

  - Intenta abrir una conexión TCP en un puerto específico.
  - Ejemplo:

    ```yaml
    tcpSocket:
      port: 8080
    ```

- **Command Execution**:

  - Ejecuta un comando dentro del contenedor. Si el comando devuelve un código de estado 0, el check es exitoso.
  - Ejemplo:

    ```yaml
    exec:
      command:
        - cat
        - /tmp/healthy
    ```

## Ejemplo de Configuración de Probes

Aquí tienes un ejemplo de configuración de liveness y readiness probes en un Pod:

```yaml
apiVersion: v1
kind: Pod
meta
  name: mi-pod
spec:
  containers:
  - name: mi-contenedor
    image: myapp
    livenessProbe:
      httpGet:
        path: /health
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
    readinessProbe:
      httpGet:
        path: /ready
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 5
```

### Explicación del Ejemplo

- livenessProbe:

  - Realiza un HTTP GET en /health cada 5 segundos después de un retraso inicial de 10 segundos.

- readinessProbe:

  - Realiza un HTTP GET en /ready cada 5 segundos después de un retraso inicial de 5 segundos.

## Kubernetes Service Discovery

El **Service Discovery** en Kubernetes es el mecanismo que permite a las aplicaciones encontrar y comunicarse con otros servicios dentro del clúster. Kubernetes proporciona varias formas de implementar el descubrimiento de servicios, asegurando que las aplicaciones puedan interactuar de manera eficiente y dinámica.

### Componentes Clave

1. **Services**:

   - Un Service en Kubernetes es un recurso que expone una aplicación en ejecución como un conjunto de Pods, proporcionando una única dirección IP y un nombre DNS para acceder a ellos.

2. **Endpoints**:
   - Los Endpoints son objetos que almacenan las direcciones IP de los Pods asociados a un Service, permitiendo que el tráfico se dirija correctamente.

### Métodos de Service Discovery

1. **Environment Variables**:

   - Kubernetes inyecta variables de entorno en los Pods, que contienen información sobre los servicios disponibles.
   - Ejemplo: `MY_SERVICE_SERVICE_HOST` y `MY_SERVICE_SERVICE_PORT`.

2. **DNS-Based Service Discovery**:
   - Kubernetes utiliza un sistema DNS interno para resolver los nombres de los servicios a sus direcciones IP.
   - Ejemplo: Un Service llamado `my-service` en el espacio de nombres `default` se resuelve como `my-service.default.svc.cluster.local`.

### Tipos de Servicios

1. **ClusterIP**:

   - Proporciona un acceso interno dentro del clúster usando una dirección IP interna.

2. **NodePort**:

   - Expone el servicio en un puerto estático en cada nodo del clúster, permitiendo el acceso desde fuera del clúster.

3. **LoadBalancer**:

   - Crea un balanceador de carga externo utilizando el proveedor de nube subyacente para exponer el servicio al mundo exterior.

4. **ExternalName**:
   - Mapea un Service a un nombre DNS externo, redirigiendo el tráfico a servicios externos.

## Ejemplo de Configuración de un Service

Aquí tienes un ejemplo de un Service YAML que utiliza `ClusterIP`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

### Explicación del Ejemplo

- selector:
  - Asocia el Service con los Pods etiquetados con app: MyApp.
- ports:
  - Define el puerto del Service (80) y el puerto del contenedor al que apunta (9376).

Lstar todos los services

````sh
    kubectl get services
    ```
Crear un servicio

```sh
 kubectl expose pod/hello-world --type=NodePort --port=8080
````

## Deployments en Kubernetes

Un **Deployment** en Kubernetes es un recurso que proporciona una forma declarativa de gestionar aplicaciones. Permite definir el estado deseado de las aplicaciones y Kubernetes se encarga de mantener ese estado, realizando actualizaciones y escalado de manera automática.

### Características de un Deployment

1. **Declaración del Estado Deseado**:

   - Permite especificar el número de réplicas, la imagen del contenedor y otras configuraciones.

2. **Actualizaciones Declarativas**:

   - Facilita la actualización de aplicaciones mediante el uso de estrategias como rolling updates para minimizar el tiempo de inactividad.

3. **Escalado Automático**:

   - Permite escalar el número de réplicas hacia arriba o hacia abajo según sea necesario.

4. **Autocorrección**:
   - Kubernetes monitorea continuamente el estado del Deployment y realiza correcciones si es necesario para garantizar que el estado actual coincida con el estado deseado.

### Componentes de un Deployment

- **ReplicaSet**:

  - Un Deployment gestiona uno o más ReplicaSets, que son responsables de mantener un número específico de Pods en ejecución.

- **Pods**:
  - Los Pods son las instancias de las aplicaciones que se ejecutan en el clúster.

### Ejemplo de Configuración de un Deployment

Aquí tienes un ejemplo básico de un Deployment en YAML:

```yaml
apiVersion: apps/v1
kind: Deployment
meta
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:1.21
        ports:
        - containerPort: 80
```

#### Explicación del Ejemplo

- replicas:
  - Define el número de réplicas de Pods que se deben ejecutar (3 en este caso).
- selector:
  - Utiliza etiquetas para identificar los Pods gestionados por el Deployment.
- template:
  - Especifica el Pod que se debe crear, incluyendo contenedores, imágenes y puertos.

#### Estrategias de Actualización

- Rolling Update:
  - Actualiza gradualmente las instancias de la aplicación para minimizar el tiempo de inactividad.
- Recreate:
  - Elimina todos los Pods existentes antes de crear los nuevos. Puede causar tiempo de inactividad.

Crear un deployment

```sh
kubectl create deployment <name> --image=<image> --port=<port> --dry-run=client -o yaml > deployment.yaml
```

Listar los deployments

```sh
kubectl get deployments
```

Modificar la cantidad de replicas del pod

    ```sh
    kubectl scale deployment my-deployment --replicas=5
    ```

## Ingress en Kubernetes

El **Ingress** en Kubernetes es un recurso que gestiona el acceso externo a los servicios dentro de un clúster, generalmente HTTP y HTTPS. Proporciona reglas de enrutamiento para que las solicitudes lleguen a los servicios correctos.

### Componentes Clave del Ingress

1. **Ingress Resource**:

   - Especifica las reglas de enrutamiento para el tráfico entrante. Define cómo se debe dirigir el tráfico a los servicios internos.

2. **Ingress Controller**:
   - Es un componente que implementa las reglas definidas en el recurso Ingress. Escucha cambios en los recursos Ingress y configura un balanceador de carga para gestionar el tráfico.

### Características del Ingress

- **Enrutamiento Basado en Host y Ruta**:

  - Permite dirigir el tráfico a diferentes servicios basándose en el nombre de host o la ruta de la URL.

- **Terminación SSL/TLS**:

  - Soporta la terminación de SSL/TLS para gestionar el tráfico HTTPS.

- **Balanceo de Carga**:

  - Proporciona balanceo de carga para distribuir el tráfico entre las instancias de los servicios.

- **Reescritura de URL**:
  - Permite modificar las URL de las solicitudes entrantes antes de dirigirlas a los servicios.

## Ejemplo de Configuración de Ingress

Aquí tienes un ejemplo básico de un recurso Ingress en YAML:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /app1
            pathType: Prefix
            backend:
              service:
                name: app1-service
                port:
                  number: 80
          - path: /app2
            pathType: Prefix
            backend:
              service:
                name: app2-service
                port:
                  number: 80
```

### Explicación del Ejemplo

- annotations:

  - Configura características específicas del controlador Ingress, como la reescritura de URL.

- rules:

  - Define las reglas de enrutamiento. En este ejemplo, el tráfico a example.com/app1 se dirige a app1-service y example.com/app2 a app2-service.

- host:

  - Especifica el dominio que el Ingress gestionará.

- path:

  - Define la ruta que se debe utilizar para el enrutamiento.

- backend:

  - Especifica el servicio al que se debe dirigir el tráfico y el puerto del servicio.

### Ingress Controllers

Para que el recurso Ingress funcione, necesitas un Ingress Controller. Algunos de los más comunes son:

- NGINX Ingress Controller:

  - Popular y ampliamente utilizado. Ofrece muchas características y personalizaciones.

- Traefik:

  - Ofrece integración sencilla con Kubernetes y características avanzadas como gestión de certificados.

- HAProxy:

  - Proporciona un rendimiento robusto y características avanzadas de enrutamiento.

Como crear un ingress en kubernetes

```sh
kubectl create ingress hello-world --rule="codely-k8s.com/*=hello-world:80"
```

## ConfigMaps y Secrets en Kubernetes

En Kubernetes, **ConfigMaps** y **Secrets** son recursos que permiten gestionar la configuración de las aplicaciones de manera separada del código. Esto facilita la gestión y actualización de configuraciones y credenciales de forma segura y eficiente.

## ConfigMaps

### ¿Qué es un ConfigMap?

Un **ConfigMap** es un objeto que permite almacenar pares clave-valor para configurar aplicaciones. Se utiliza para almacenar configuraciones no confidenciales.

### Uso de ConfigMaps

- **Configuración de Variables de Entorno**:

  - Se pueden inyectar como variables de entorno en los contenedores.

- **Archivos de Configuración**:

  - Se pueden montar como archivos en el sistema de archivos del contenedor.

- **Comandos y Argumentos**:
  - Se pueden utilizar para definir comandos y argumentos en los contenedores.

### Ejemplo de ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
meta
  name: my-config
data:
  database_url: "mongodb://localhost:27017"
  log_level: "debug"
```

### Uso en el POD

```yaml
apiVersion: v1
kind: Pod
meta
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image
    env:
    - name: DATABASE_URL
      valueFrom:
        configMapKeyRef:
          name: my-config
          key: database_url
```

### Secrets

Un Secret es similar a un ConfigMap, pero está diseñado para almacenar información sensible, como contraseñas, tokens y claves. Los datos en un Secret están codificados en base64.

#### Uso de Secrets

- **Variables de Entorno**:
  - Se pueden inyectar como variables de entorno en los contenedores.
- **Archivos de Configuración**:
  - Se pueden montar como archivos en el sistema de archivos del contenedor.
- **Acceso a la API**:
  - Se pueden utilizar para autenticar con la API de Kubernetes.

### Ejemplo de Secret

```yaml
apiVersion: v1
kind: Secret
meta
  name: my-secret
type: Opaque
data:
  username: YWRtaW4=   # "admin" en base64
  password: cGFzc3dvcmQ= # "password" en base64

```

### Uso en el POD

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-image
      env:
        - name: USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
```
