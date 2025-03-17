# Prueba técnica Craftech - Pt. 2

Instrucciones para despliegue de la aplicación.

#### Desarrollo
1. Clonar repositorio en el equipo local

```bash
git clone link
```

2. Ubicarse en la raíz del proyecto

```bash
cd prueba-craftech
```

3. Levantar servicios

```bash
docker compose up
```

#### Aclaraciones:
* Ante cualquier cambio de models, si fuera necesario correr nuevamente las migraciones puede hacerse sin bajar los servicios. Desde otra terminal:

```bash
docker compose exec api ./manage.py migrate
```
* De la misma manera puede ejecutarse cualquier comando correspondiente el manage.py

```bash
docker compose exec api ./manage.py <comando>
```

#### Producción

##### Se propone:
* Un cluster de kubernetes configurado con secrets correspondientes, todo en namespace "craftech".

* Un pipeline (similar a pt. 3) que buildee las imagenes especificadas en los archivos Dockerfile.prod con tag basado en versionado semántico, actualice la version de la imagen en el Helm chart y haga upgrade del deploy via SSH.

##### Despliegue:
1. Acceder al servidor via SSH:

```bash
ssh <usuario>@<ip/host>
```

2. Agregar repositorio a Helm

```bash
helm repo add craftech oci://ghcr.io/fmolina02/prueba-craftech
```

3. Verificar que se haya agregado correctamente

```bash
helm search repo cracftech
```

4. Instalar despliegue

```bash
helm install craftech/prueba-craftech -n craftech
```

5. Verificar que se haya instalado correctamente

```bash
kubectl get pods -n craftech
```

## Explicación

Se determinan dos ambientes: desarrollo y producción. 

En el ambiente de desarrollo se cumple el requerimiento del despliegue de todos los servicios con un unico docker-compose, herramienta efectiva para el desarrollo, pero que no cubre ciertos aspectos como recuperación o escalado para producción. Para este ambiente las imagenes corresponden a los archivos Dockerfile de cada proyecto.

En el ambiente de producción la construcción de las imagenes cambia. Se utilizan servidores de aplicación, nginx en el caso de react y gunicorn en el caso de django. Además se busca aligerar las imagenes con versiones slim. Se incluyen las especificaciones para generar un chart de Helm instalable en kubernetes facilmente.