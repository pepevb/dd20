# Digital D20

This project has two parts:

 * A framework to create hyperlynked RPG modules with all the rules maps and scenes connected. 
 * A super usable full featured Virtual Table Top
 
## Digital D20 framework

First point to start is to read the manual, you can access it con http://play.digitald20.com and going
inside homebrew adventures. 
There you will read how to create with http://twinery.org an RPG adventure. 

You can take a look on the directory structure of the free adventures, and have an idea of how to create them. 

## Mobile version

There is also an Android and iOS App, but these are not yet open software. 


## Virtual Table Top

Look inside the folder `backend` for the server-side code and `frontend` for the client-side code.

As a Node.js project, the first thing you need to do is to download all the required libraries. 
You can create your own server in a few minutes using a free Node.js server like Heroku. 

99% of the code runs locally on `main.js` (en el frontend), el servidor solo envía los mensajes a todas las personas conectadas a la misma URL. 

### Cómo Lanzar la Aplicación

Para poner en marcha la aplicación, sigue estos pasos:

1.  **Navega al directorio `backend`:**
    Abre tu terminal y cambia el directorio a `backend` dentro de tu espacio de trabajo.

    ```bash
    cd /Users/pepe/cursor/dd20/backend
    ```

2.  **Instala las dependencias:**
    La aplicación utiliza dependencias de Node.js que se listan en `package.json`. Necesitas instalarlas usando npm (Node Package Manager).

    ```bash
    npm install
    ```
    Esto descargará e instalará todas las librerías necesarias (Feathers.js, Express, Socket.io, LevelDB, ImageKit, etc.) en el directorio `node_modules/`.

3.  **Lanza la aplicación:**
    El archivo `package.json` define un script de inicio (`"start": "node index.js"`). Puedes ejecutar este script para iniciar el servidor.

    ```bash
    npm start
    ```
    Si todo funciona correctamente, deberías ver un mensaje en la terminal similar a:
    ```
    Feathers server listening on localhost:3000
    ```
    Esto indica que el servidor Feathers.js está escuchando las conexiones entrantes en el puerto 3000 (o el puerto configurado en `process.env.PORT` si está definido).

4.  **Accede a la aplicación en tu navegador:**
    Una vez que el servidor esté corriendo, abre tu navegador web y navega a la siguiente dirección:

    ```
    http://localhost:3000
    ```
    Esto debería cargar la interfaz de usuario de la aplicación.

**Notas Importantes:**

*   **Versión de Node.js:** El archivo `backend/package.json` especifica `"node": "22.11.0"` bajo la sección `engines`. Asegúrate de tener una versión compatible de Node.js instalada en tu sistema. Se recomienda usar `nvm` (Node Version Manager) para gestionar las versiones de Node.js.
*   **Base de Datos `db/`:** La aplicación utiliza LevelDB, que almacenará sus datos en un directorio `./db` dentro de `backend/`. Este directorio se creará automáticamente si no existe.
*   **Archivos Estáticos:** Todos los archivos dentro del directorio `frontend/` son servidos como archivos estáticos directamente por el servidor.

### Visión general de la aplicación

La aplicación **Digital D20** es una plataforma web completa para jugar aventuras de rol de mesa, inspirada en Dungeons & Dragons, con un fuerte enfoque en la interactividad y la experiencia en tiempo real. Se estructura en las siguientes partes clave:

*   **Módulos de Aventura:** La plataforma soporta múltiples aventuras o módulos de juego independientes, ubicados en directorios como `beastmasters-daughter`, `burning-goblins` y `salorium`. Cada módulo encapsula sus propios recursos (HTML, CSS, JavaScript, imágenes y audio) para una experiencia narrativa y ambiental única.

*   **Componentes Centrales (`core/`):** El directorio `core/` actúa como un repositorio centralizado para estilos globales, scripts, recursos multimedia y datos fundamentales, como las reglas de D&D en inglés y español. Estos componentes son compartidos y reutilizados por todas las aventuras, asegurando coherencia y eficiencia.

*   **Servidor de Tablero Virtual (VTT) (`backend/`):** Este directorio contiene el corazón del Virtual Tabletop. Se trata de una aplicación Node.js robusta, construida con el framework Feathers.js. El servidor `backend` se encarga de:
    *   **Servir Archivos Estáticos:** Distribuye todos los archivos estáticos (HTML, CSS, JavaScript, imágenes) que conforman la interfaz de usuario de las aventuras (ubicados en el directorio `frontend/`).
    *   **Gestión de Salas de Juego:** Permite a los usuarios crear, unirse y sincronizar sesiones de juego en "salas" virtuales.
    *   **Comunicación en Tiempo Real:** Utiliza WebSockets a través de Socket.io para una comunicación fluida y en tiempo real entre los jugadores y el maestro de juego.
    *   **Persistencia de Datos:** Almacena datos cruciales de las salas, como fondos de mapa o estados de juego, utilizando LevelDB, una base de datos de clave-valor.
    *   **Integración de Imágenes:** Se integra con ImageKit para la gestión, optimización y entrega eficiente de imágenes.

*   **Frontend Interactivo (`frontend/`):** La interfaz de usuario es una aplicación web dinámica desarrollada con HTML, CSS (utilizando Bulma para el diseño base y estilos personalizados) y JavaScript. Destaca por su alta interactividad, gracias a librerías clave como:
    *   **Fabric.js:** Imprescindible para la creación de un lienzo interactivo, permitiendo a los usuarios dibujar mapas, manipular tokens y otros elementos gráficos directamente en el navegador.
    *   **Three.js y Cannon.js:** Empleadas para la simulación de dados en 3D y otros efectos visuales y de física, enriqueciendo la experiencia inmersiva del VTT.
    *   **Hammer.js:** Facilita el reconocimiento de gestos táctiles, mejorando la usabilidad en dispositivos móviles.
    *   Además, cuenta con funcionalidades avanzadas como zoom, gestión de idiomas y scripts dedicados para cargar y guardar el estado de las salas.

*   **Herramientas de Desarrollo (`develop/`):** El proyecto incluye herramientas internas, como un creador de mapas, para simplificar y agilizar la creación de nuevo contenido y la personalización de las aventuras.

En resumen, Digital D20 ofrece una experiencia de rol de mesa digital completa y flexible, diseñada para maestros de juego y jugadores que buscan una plataforma interactiva y en tiempo real para sus aventuras.

### About me

My work has nothing to do with software neither writting, but I love dungeons and dragones since I was a little kid. 
I do not have much time, but feel free to contact me, specially if you want to push any of these two projects!!
I have opened a Pateron with 0 folowers yet, I just want to pay the bills of the servers, that is all. In the past I had the idea to make business with RPGs but that era has ended for me. I just want to create funny things in my spare time. 
