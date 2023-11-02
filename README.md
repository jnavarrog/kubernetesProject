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



#Referencias

[1]https://nextcloud.com/es/

[2]https://www.mysql.com/

[3]https://grafana.com/

[4]https://prometheus.io/

[5]https://github.com/prometheus/node_exporter
