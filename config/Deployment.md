# DESPLIEGUE A PRODUCCIÓN

El despliegue de la aplicación se realiza en un servidor de producción en DigitalOcean, utilizando Docker y Docker Compose para construir y ejecutar los contenedores necesarios para la aplicación.

## Requisitos

A continuación se detallan los requisitos necesarios para desplegar la aplicación en un servidor de producción.

### 1. Hardware Requerido

- Servidor con al menos **2 GB de RAM** y **50 GB de almacenamiento**.
- Sistema Operativo **Ubuntu 20.04 LTS**.
- Acceso **SSH** al servidor.

### 2. Software Requerido

- [Docker](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Nginx](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)

## Pasos

A continuación se detallan los pasos necesarios para desplegar la aplicación en un servidor de producción.

### 1. Accede al Servidor

Conéctate al servidor a través de SSH:

```bash
ssh root@your_server_ip
```

### 2. Instalación de Docker y Docker Compose

Instala Docker y Docker Compose en el servidor.

#### Docker

```bash
# Actualiza el sistema
sudo apt-get update
sudo apt-get upgrade

# Instala los paquetes necesarios
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# Agrega la clave GPG de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Agrega el repositorio de Docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Instala Docker
sudo apt-get update
sudo apt-get install docker-ce

# Verifica la instalación de Docker
sudo systemctl status docker
```

#### Docker Compose

```bash
# Descarga Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Da permisos de ejecución
sudo chmod +x /usr/local/bin/docker-compose

# Verifica la instalación
docker-compose --version
```

### 3. Clona el Repositorio

Clona el repositorio de la aplicación en el servidor:

```bash
git clone --recurse-submodules https://github.com/Teelakreiste/ofsiass.git
```

### 4. Navega a la Carpeta del Proyecto

Una vez clonado, dirígete a la carpeta del proyecto:

```bash
cd ofsiass
```

### 5. Configura las Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto para almacenar las variables de entorno necesarias. Puedes utilizar el archivo de ejemplo `.env.example` como referencia. A continuación, se muestra un ejemplo de cómo puede lucir tu archivo `.env`:

```env
# Variables de base de datos
DB_NAME=sistema_solicitudes
DB_USER=root
DB_PASSWORD=test
DB_HOST=mysql # Nombre del servicio de MySQL en Docker
DB_PORT=3306

# Variables de la API
PORT=3000
SECRET_KEY=secretkey

# Configuración de correo
MAIL_HOST=smtp.example.com
MAIL_PORT=123
MAIL_USER=example@example.com
MAIL_PASS=examplepassword

# URL de la aplicación web
WEB_URL=https://example.com

# Variables de MySQL
MYSQL_ROOT_PASSWORD=test
MYSQL_DATABASE=sistema_solicitudes
MYSQL_USER=user
MYSQL_PASSWORD=12345678
```

Asegúrate de ajustar estos valores según tus configuraciones específicas.

### 6. Construye y Ejecuta los Contenedores

Construye y ejecuta los contenedores de la aplicación utilizando Docker Compose:

```bash
docker-compose up -d --build
```

### 7. Configuración del Firewall

Si estás utilizando un firewall en tu servidor, asegúrate de permitir el tráfico en los puertos necesarios para la aplicación. Por defecto, la aplicación utiliza el puerto 80 para el tráfico HTTP y el puerto 443 para el tráfico HTTPS.

```bash
# Habilita el firewall
sudo ufw enable

# Permite el tráfico en los puertos necesarios
sudo ufw allow 22    # SSH
sudo ufw allow 80    # HTTP
sudo ufw allow 443   # HTTPS
sudo ufw allow 9383  # API
sudo ufw allow 4201  # Aplicación Angular
```

### 8. Configuración del Nombre de Dominio (Opcional)

Si deseas usar un dominio para acceder a tu aplicación:

1. Apunta tu dominio a la dirección IP de tu Droplet utilizando tu proveedor de dominio.
2. Configura un servidor web como Nginx para redirigir las solicitudes al puerto correcto.

### 9. Instalación y Configuración de Nginx

Para redirigir tu dominio a la aplicación web que está corriendo en Docker, necesitarás configurar un servidor web como Nginx que actuará como un proxy inverso. A continuación, se detallan los pasos para instalar y configurar Nginx en tu servidor.

#### Paso 1: Instalar Nginx

1. **Accede a tu Droplet** usando SSH:

   ```bash
   ssh root@your_droplet_ip
   ```

2. **Instala Nginx**:

   ```bash
   sudo apt-get update
   sudo apt-get install nginx
   ```

3. **Verifica que Nginx esté corriendo**:

   ```bash
   sudo systemctl status nginx
   ```

   Deberías ver que el servicio está activo (running).

#### Paso 2: Configurar Nginx

1. **Crea un archivo de configuración para tu dominio**:

   Ve al directorio de configuraciones de Nginx:

   ```bash
   cd /etc/nginx/sites-available/
   ```

   Crea un nuevo archivo de configuración, por ejemplo, `mi_dominio.conf`:

   ```bash
   sudo nano mi_dominio.conf
   ```

2. **Agrega la siguiente configuración** en el archivo:

   ```nginx
   server {
       listen 80;
       server_name tu_dominio.com www.tu_dominio.com;

       location / {
           proxy_pass http://localhost:4201; # Cambia este puerto al puerto de tu aplicación Angular
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   Asegúrate de reemplazar `tu_dominio.com` con tu dominio real y ajustar el puerto de `proxy_pass` al puerto donde tu aplicación web está corriendo.

3. **Habilita la configuración del sitio**:

   Crea un enlace simbólico en el directorio `sites-enabled`:

   ```bash
   sudo ln -s /etc/nginx/sites-available/mi_dominio.conf /etc/nginx/sites-enabled/
   ```

4. **Verifica la configuración de Nginx**:

   Asegúrate de que no haya errores de sintaxis en la configuración:

   ```bash
   sudo nginx -t
   ```

5. **Reinicia Nginx** para aplicar los cambios:

   ```bash
   sudo systemctl restart nginx
   ```

#### Paso 3: Configurar el Firewall

Asegúrate de que el firewall permita el tráfico en el puerto 80 (HTTP) y 443 (HTTPS):

```bash
sudo ufw allow 'Nginx Full'
```

#### Paso 4: Probar la Configuración

1. Abre tu navegador y visita tu dominio. Deberías ser redirigido a tu aplicación Angular que está corriendo en Docker.

2. Si tienes configurado HTTPS, puedes usar Certbot para obtener un certificado SSL gratuito de Let's Encrypt. Para esto, sigue los pasos a continuación:

   **Instala Certbot**:

   Si aún no tienes Certbot instalado, puedes hacerlo con los siguientes comandos:

   ```bash
   sudo apt-get update
   sudo apt-get install certbot python3-certbot-nginx
   ```

   **Configurar HTTPS con Certbot**:

   Una vez que Certbot esté instalado, puedes obtener un certificado SSL para tu dominio con el siguiente comando:

   ```bash
   sudo certbot --nginx -d tu_dominio.com -d www.tu_dominio.com
   ```

   Asegúrate de reemplazar `tu_dominio.com` con tu dominio real.

   **Sigue las instrucciones en pantalla** para completar la configuración de SSL. Certbot te preguntará si deseas redirigir todo el tráfico HTTP a HTTPS. Se recomienda que elijas esta opción para que todo el tráfico se dirija a la versión segura de tu sitio.

   **Verificar la configuración de Nginx**:

   Certbot debería haber editado tu archivo de configuración de Nginx automáticamente para incluir la configuración de HTTPS. Puedes verificar esto abriendo el archivo de configuración:

   ```bash
   sudo nano /etc/nginx/sites-available/mi_dominio.conf
   ```

   Deberías ver algo similar a esto:

   ```nginx
   server {
       listen 80;
       server_name tu_dominio.com www.tu_dominio.com;

       return 301 https://$host$request_uri; # Redirige HTTP a HTTPS
   }

   server {
       listen 443 ssl;
       server_name tu_dominio.com www.tu_dominio.com;

       ssl_certificate /etc/letsencrypt/live/tu_dominio.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/tu_dominio.com/privkey.pem;

       location / {
           proxy_pass http://localhost:4201; # Cambia este puerto al puerto de tu aplicación Angular
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   **Reinicia Nginx** para aplicar los cambios:

   ```bash
   sudo systemctl restart nginx
   ```

   **Renovación Automática de Certificados**:

   Certbot configura automáticamente la renovación de certificados, pero puedes asegurarte de que el cron job esté configurado ejecutando:

   ```bash
   sudo certbot renew --dry-run
   ```

   Esto simula la renovación de tu certificado para asegurarte de que no haya problemas.

### 10. Monitoreo y Mantenimiento

Después de implementar tu aplicación, considera usar herramientas para monitorear el rendimiento y los logs de tus contenedores, como `docker logs` y `docker stats`, o herramientas de terceros como Prometheus y Grafana para un monitoreo avanzado.
