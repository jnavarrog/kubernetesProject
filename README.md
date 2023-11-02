Proyecto del curso Computación en la nube con virtualización liviana (Kubernetes)

Autor: Juan Navarro

Docentes: Edgar Magana, Eduardo Grampin

---

# Introducción
Este proyecto consiste en el despliegue mediante Kubernetes de un sistema de almacenamiento en la nube (Nextcloud) junto con su base de datos (MySQL). Además se le agrega un sistema de monitoreo de datos del nodo utilizando Grafana, Prometheus y Node-Exporter.

El despliegue se puede realizar de dos formas, utilizando Kustomization o utilizando Helm. La opción con Helm permite editar facilmente algunos parametros de la configuración por lo que se recomienda utilizarla.

# Sistemas utilizados

- Nextcloud[1] como sistema de almacenamiento en la nube.
- MySQL[2] como base de datos.
- Grafana[3] como visualizador de datos de gestión.
- Prometheus[4] como recolector de datos.
- Node Exporter[5] como agente de Prometheus.

## Nextcloud

El despliegue de Nextcloud consta de un deployment, un service, un persitent volume y un persistent volume claim. En el deployment como configuración a destacar, se utilizó un configmap para pasarle al contenedor los datos de conexión a la base de datos y se agrego un livenessProbe. El service se configura como LoadBalancer para poder acceder al puerto 80 del contenedor mediante un puerto configurable en localhost.

## MySQL

Al igual que Nextcloud consiste en un deployment, un service, un persitent volume y un persistent volume claim. La configuración es la estándar. También se utilizó un configmap con el nombre de la base de datos que debe crear y la contraseña de root.

## Grafana

Para Grafana se utilizó un deployment y un service. En el deployment se configuró un livenessProbe y mediante el configmap se le pasa el usuario y contraseña por defecto. En el service se crea el LoadBalancer que permite acceder al sitio desde el exterior (localhost) en un puerto configurable.

## Prometheus

Prometheus se despliega utilizando un deployment donde se agrega un livenessProbe y se le pasan dos argumentos (archivo de configuración y ruta de la base de datos) al container, y un service del tipo LoadBalancer para acceder al sistema desde el exterior (localhost) en un puerto configurable. Mediante el configmap edita el archivo de configuración para indicarle a Prometheus que debe incluir al Node Exporter como fuente de datos.

## Node Exporter

El agente node exporter se despliega como un DaemonSet ya que como su función es recopilar datos del nodo, es útil que se asegure su ejecución en todos los nodos. Se le configuran limites de consumo tanto de CPU como de memoria. Además expone su servicio mediante un servicio del tipo Cluster IP.

## Parámetros

| Parámetro                   | Descripción                                                | Valor por defecto         |
|-----------------------------|------------------------------------------------------------|---------------------------|
| nexcloud.mysql_host         | Contenedor donde se encuentra la base de datos             | mysql-service             |
| nextcloud.mysql_user        | Usuario de la basde datos                                  | nextcloud                 |
| nextcloud.mysql_password    | Contraseña de la base de datos                             | nextcloud                 |
| nextcloud.pv_size           | Tamaño del PV                                              | 5Gi                       |
| nextcloud.pv_path           | Path del PV                                                |                           |
| nextcloud.pvc_size          | Tamaño del PVC                                             | 5Gi                       |
| nextcloud.image             | Imagen utilizada                                           | nextcloud:stable          |
| nextcloud.replicas          | Pantidad de replicas del pod                               | 1                         |
| nextcloud.externalport      | Puerto del host al cual se redirige el puerto del servicio | 80                        |
| mysql.mysql_root_password   | Contraseña del usuario root                                | nextcloud                 |
| mysql.mysql_database        | Base de datos a crear                                      | nextcloud                 |
| mysql.image                 | Image utilizada                                            | mysql:8.2.0               |
| mysql.pv_size               | Tamaño del PV                                              | 1Gi                       |
| mysql.pv_path               | Path del PV                                                |                           |
| mysql.pvc_size              | Tamaño del PVC                                             | 1Gi                       |
| prometheus.image            | Imagen utilizada                                           | prom/prometheus:v2.47.2   |
| prometheus.externalport     | Puerto del host al cual se redirige el puerto del servicio | 8081                      |
| grafana.default_user        | Usuario por defecto                                        | admin                     |
| grafana.default_password    | Contraseña por defecto                                     | admin                     |
| grafana.image               | Imagen utilizada                                           | grafana/grafana:10.2.0    |
| grafana.externalport        | Puerto del host al cual se redirige el puerto del servicio | 8080                      |
| node_exporter.image         | Imagen utilizada                                           | prom/node-exporter:v1.6.1 |
| node_exporter.cpulimit      | Limite de utilización de CPU                               | 2                         |
| node_exporter.request       | Cantidad de CPU solicitado                                 | 1                         |
| node_exporter.memorylimit   | Limite de utilización de memoria                           | 1000Mi                    |
| node_exporter.memoryrequest | Cantidad de memoria solicitada                             | 500Mi                     |
|                             |                                                            |                           |


#Referencias

[1]https://nextcloud.com/es/

[2]https://www.mysql.com/

[3]https://grafana.com/

[4]https://prometheus.io/

[5]https://github.com/prometheus/node_exporter
