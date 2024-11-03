# Proyecto OFSIASS

Este repositorio contiene la estructura del sistema OFSIASS, diseñado para optimizar el flujo de solicitudes e insumos en Agrícola Sara Palma SAS. Se trata de un **monorepo** que incluye submódulos para dos componentes principales:

- **api-solicitudes**: Un servicio backend desarrollado en Node.js, que gestiona la lógica de negocio y la interacción con la base de datos.
- **web-solicitudes**: Una aplicación frontend que proporciona una interfaz de usuario intuitiva y accesible para la gestión de solicitudes e insumos.

## OPTIMIZACIÓN DEL FLUJO DE SOLICITUDES DE INSUMOS EN AGRÍCOLA SARA PALMA S.A.S - OFSIASS

El proyecto consiste en el desarrollo de un sistema de gestión para optimizar el flujo de solicitudes de insumos y fertilizantes en Sara Palma S.A.S. La aplicación permite a los usuarios crear, gestionar y realizar un seguimiento de las solicitudes de insumos de manera eficiente. Entre sus funcionalidades principales se incluyen:

1. **Creación de Solicitudes**: Permite a los Servicios Técnicos generar solicitudes detalladas para los insumos requeridos, incluyendo la posibilidad de adjuntar documentos relevantes y especificar cantidades y requisitos especiales.

2. **Cotización de Solicitudes**: El departamento de Compras puede recibir y revisar las solicitudes generadas, generando cotizaciones en un formato estandarizado y facilitando la comunicación con los proveedores.

3. **Aprobación de Solicitudes**: El Jefe Financiero tiene acceso a un panel donde puede revisar todas las solicitudes y cotizaciones, tomando decisiones informadas sobre la aprobación o rechazo de las solicitudes.

4. **Recepción de Insumos**: Los Receptores pueden registrar la recepción de insumos de manera estructurada, asegurando que se mantenga un registro claro y preciso del estado de los insumos recibidos.

5. **Visualización y Reportes**: Los Visualizadores pueden acceder a un dashboard interactivo que proporciona gráficos e indicadores clave sobre el estado de las solicitudes, tiempos de procesamiento y cumplimiento de requisitos, facilitando un análisis efectivo de la información.

El sistema busca mejorar la eficiencia operativa y la toma de decisiones al proporcionar una plataforma centralizada para la gestión de solicitudes, minimizando la confusión y los errores asociados con el manejo manual de información.

## Tabla de Contenidos

- [Características](#características)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Instalación](#instalación)
- [Uso](#uso)
- [Licencia](#licencia)

## Características

- Autenticación de usuarios con token, asegurando que solo los usuarios registrados puedan acceder a las funcionalidades de la aplicación.
- Gestión de roles (Administrador y Usuario), permitiendo diferentes niveles de acceso y permisos según el rol asignado.
- Protección de rutas mediante guards, garantizando que los usuarios sin los permisos adecuados no puedan acceder a ciertas partes de la aplicación.
- Interfaz amigable y responsive, diseñada para ser accesible desde dispositivos móviles y de escritorio, mejorando la experiencia del usuario.
- Manejo de errores y validaciones en formularios, asegurando que la entrada de datos sea correcta y brindando retroalimentación clara al usuario.

## Tecnologías Utilizadas

- **[Angular](https://angular.io/docs)**: Framework de desarrollo para construir aplicaciones web de una sola página (SPA).
- **[TypeScript](https://www.typescriptlang.org/docs/)**: Lenguaje de programación que añade tipado estático a JavaScript, mejorando la calidad del código.
- **[HTML/CSS](https://developer.mozilla.org/es/docs/Web)**: Tecnologías básicas para la creación de contenido y diseño web.
- **[SweetAlert2](https://sweetalert2.github.io/)**: Librería para mostrar alertas personalizadas, mejorando la interacción del usuario con la aplicación.
- **[RxJS](https://rxjs.dev/guide/overview)**: Biblioteca para la programación reactiva, utilizada para gestionar estados asíncronos y eventos en la aplicación.
- **[Node.js](https://nodejs.org/en/docs/)**: Entorno de ejecución de JavaScript para el backend, permitiendo la creación de servidores y aplicaciones de red.
- **[Express](https://expressjs.com/en/starter/installing.html)**: Framework de Node.js para la creación de servidores web y APIs RESTful.
- **[MySQL](https://dev.mysql.com/doc/)**: Sistema de gestión de bases de datos relacional, utilizado para almacenar y gestionar la información de la aplicación.
- **[Sequelize](https://sequelize.org/master/)**: ORM (Object-Relational Mapping) para Node.js, facilitando la interacción con la base de datos y la creación de modelos de datos.
- **[JWT](https://jwt.io/introduction/)**: JSON Web Tokens, utilizado para la autenticación y autorización de usuarios en la aplicación.
- **[Swagger](https://swagger.io/docs/)**: Herramienta para la documentación de APIs, facilitando la comprensión y el uso de los endpoints disponibles. (Próximamente)
- **[Docker](https://docs.docker.com/)**: Plataforma de contenedores que facilita la creación, implementación y ejecución de aplicaciones en entornos aislados.

## Instalación

Para instalar el proyecto, sigue estos pasos:

### 1. Clona el Repositorio

Clona el repositorio principal junto con los submódulos utilizando el siguiente comando:

```bash
git clone --recurse-submodules https://github.com/Teelakreiste/ofsiass.git
```

### 2. Navega a la Carpeta del Proyecto

Una vez clonado, dirígete a la carpeta del proyecto:

```bash
cd ofsiass
```

### 3. Configura las Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto para almacenar las variables de entorno necesarias. Puedes utilizar un archivo de ejemplo `.env.example` como referencia. Aquí tienes un ejemplo de cómo puede lucir tu archivo `.env`:

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

### 4. Instala Dependencias

Cada submódulo tiene su propia carpeta de dependencias. Accede a cada uno de los submódulos (`api-solicitudes` y `web-solicitudes`) y ejecuta el comando de instalación de dependencias:

#### Para la API

```bash
cd api-solicitudes
npm install
cd ..
```

#### Para la Aplicación Web

```bash
cd web-solicitudes
npm install
cd ..
```

*Nota*: Actualiza la url de la API en el archivo `src/environments/environment.ts` en la propiedad `apiUrl` con la dirección de la API en tu entorno de producción y `src/environments/environment.development.ts` en el caso de desarrollo.

```typescript
// environment.ts
export const environment = {
  production: true,
  apiUrl: 'http://localhost:3000', // Actualiza esta URL con la dirección de la API en producción
};

// environment.development.ts
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000', // Actualiza esta URL con la dirección de la API en desarrollo
};
```

### 5. Construye y Ejecuta los Contenedores

Utiliza Docker Compose para construir y ejecutar los contenedores necesarios para la aplicación:

```bash
docker-compose up --build
```

Esto levantará la API, la base de datos MySQL y la aplicación web.

### 6. Accede a la Aplicación

Una vez que los contenedores estén en funcionamiento, podrás acceder a la aplicación web en tu navegador a través de la URL: `http://localhost:4201`

Y la API estará disponible en: `http://localhost:9383`

### 7. Pruebas

Asegúrate de que todas las funcionalidades estén funcionando correctamente realizando pruebas en la aplicación.

### 8. Detener los Contenedores

Para detener los contenedores en ejecución, puedes utilizar:

```bash
docker-compose down
```

*Nota*: Para ejecutar la aplicación ya sea en producción o desarrollo no es necesario utilizar Docker, puedes seguir los pasos de instalación y ejecución de cada submódulo por separado, teniendo en cuenta las variables de entorno y configuraciones necesarias y que requieres tener instalado Node.js y MySQL en tu máquina o servidor para el buen funcionamiento de la aplicación.

#### Despliegue en Producción (Ver [Deployment.md](./config/Deployment.md))

## Uso

Después de instalar y ejecutar la aplicación, los usuarios pueden realizar las siguientes acciones:

- **Registro de Usuarios**: Los nuevos usuarios pueden registrarse en el sistema proporcionando su información.
- **Inicio de Sesión**: Los usuarios existentes pueden acceder a la aplicación utilizando sus credenciales. Se generará un token que se usará para la autenticación en futuras solicitudes.
- **Gestión de Solicitudes**: Los usuarios pueden crear, editar y eliminar solicitudes de insumos. Cada solicitud puede incluir detalles como el tipo de insumo, cantidad y especificaciones adicionales.
- **Visualización de Reportes**: Los administradores pueden generar y visualizar reportes sobre el estado de las solicitudes, el historial de cambios y la eficiencia del proceso de cotización.

## Licencia

Este proyecto está bajo una licencia que está por definir. Se proporcionará más información sobre la licencia en una fecha posterior.
