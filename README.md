# Tienda de Perritos - Capa de Base de Datos

Este repositorio contiene las configuraciones de infraestructura como código y los scripts de inicialización para la capa de persistencia de datos relacionales, basada en un motor de base de datos MySQL.

## Contenido del Repositorio
* `/sql`: Scripts de inicialización de esquemas, tablas y datos base para el sistema.
* `Dockerfile`: Imagen personalizada basada en la distribución oficial de MySQL.
* `/.github/workflows/deploy.yml`: Flujo de despliegue/actualización del microservicio de datos.
* `/k8s`: Manifiestos específicos de Kubernetes (`deployment.yaml`, `service.yaml`, `secret.yaml`).

## Funcionamiento e Inserción en la Red
Para mitigar vectores de ataque y cumplir con las mejores prácticas de arquitectura cloud, la base de datos reside de manera aislada en la **Subred Privada 2** (Capa de Datos).

* **Aislamiento Perimetral:** No posee direccionamiento IP público ni conexión directa a Internet.
* **Control de Accesos (Security Groups):** El tráfico entrante está restringido a nivel de red y de clúster. Únicamente se permiten peticiones en el puerto `3306` que provengan explícitamente desde los Pods residentes en la capa de Backend.

## Resiliencia y Ciclo de Vida
Aunque la base de datos opera de manera contenerizada dentro del clúster de **Amazon EKS**, el orquestador garantiza la tolerancia a fallos. En caso de una degradación del nodo o de la zona de disponibilidad primaria, Kubernetes redistribuye y levanta el Pod de la base de datos en la zona de disponibilidad alterna de forma autónoma, garantizando la continuidad de la persistencia del negocio.
