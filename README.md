# Pi-hole + Unbound

Este proyecto proporciona una configuración lista para usar de **Pi-hole** junto con **Unbound** utilizando Docker Compose.

## 📌 Puntos del Proyecto

* **Pi-hole**: Actúa como un sumidero de DNS (DNS sinkhole) a nivel de red para proteger tus dispositivos, bloqueando anuncios y rastreadores sin necesidad de instalar software cliente.
* **Unbound**: Es un servidor DNS recursivo, con validación, seguro y con caché. Al usar Pi-hole junto con Unbound, evitas depender de resolutores DNS de terceros (como Google o Cloudflare), mejorando tu privacidad al realizar consultas DNS directamente a los servidores raíz (root servers).
* **Docker Compose**: Orquesta ambos servicios de manera sencilla y automatizada, garantizando que Pi-hole inicie después de Unbound (`depends_on`) y que puedan comunicarse entre sí.
* **Persistencia de Datos**: Utiliza volúmenes de Docker (`pihole_data_volume`) para asegurar que la configuración de Pi-hole, las listas de bloqueo y el historial de consultas persistan tras reiniciar los contenedores.
* **Configuración Personalizada**: Incluye un volumen montado desde el directorio `files/unbound/unbound.conf` hacia el contenedor, permitiendo administrar la configuración del servidor Unbound fácilmente.

## 🚀 Cómo Levantar el Servicio Localmente

### Prerrequisitos
* Tener instalado [Docker](https://docs.docker.com/get-docker/) y [Docker Compose](https://docs.docker.com/compose/install/).
* Asegurarte de que el puerto `53` (TCP/UDP) no esté en uso en tu máquina (en algunos sistemas Linux, el servicio `systemd-resolved` ocupa este puerto por defecto y debe deshabilitarse o reconfigurarse).

### Pasos a seguir

1. **Clonar el repositorio:**
   Si aún no lo has hecho, clona el repositorio y entra al directorio:
   ```bash
   git clone <url-del-repositorio>
   cd pihole
   ```

2. **Configurar variables de entorno:**
   En el directorio `docker-compose`, crea un archivo llamado `.env`. El `docker-compose.yml` está configurado para leer este archivo y pasar las variables a Pi-hole.
   
   Crea el archivo `docker-compose/.env` con tu configuración preferida, por ejemplo:
   ```env
   # Configura tu zona horaria
   TZ=America/Argentina/Buenos_Aires
   # Contraseña para acceder al panel web de administración de Pi-hole
   WEBPASSWORD=tu_contraseña_segura
   ```

3. **Levantar los servicios:**
   Ingresa al directorio de Docker Compose y ejecuta el comando para levantar los contenedores en segundo plano (`-d`):
   ```bash
   cd docker-compose
   docker-compose up -d
   ```

4. **Acceder al panel de administración:**
   Una vez que los contenedores estén en ejecución, puedes ingresar al panel web de Pi-hole abriendo tu navegador en:
   👉 `http://localhost:80/admin` o `http://<IP_DE_TU_MAQUINA>/admin`
   
   *(Inicia sesión utilizando la contraseña que configuraste en `WEBPASSWORD`)*.

5. **Configurar tus dispositivos:**
   Para comenzar a filtrar tráfico, cambia la configuración de red de tu router (o de cada dispositivo individual) para que utilicen la **dirección IP de la máquina** que ejecuta este Docker como su servidor DNS primario.

### 🛑 Detener el Servicio
Para detener y eliminar los contenedores (tus configuraciones y listas de bloqueo se mantendrán guardadas en los volúmenes), ejecuta:
```bash
docker-compose down
```
