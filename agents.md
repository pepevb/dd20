# Digital D20 - Visión General del Funcionamiento

La aplicación **Digital D20** es una plataforma web completa diseñada para facilitar el juego de rol de mesa (RPG), inspirada en sistemas como Dungeons & Dragons. Su arquitectura modular y el uso de tecnologías modernas permiten una experiencia interactiva y en tiempo real tanto para Maestros de Juego como para jugadores. A continuación, se detalla cómo funciona la aplicación:

## 1. Estructura General del Proyecto

El proyecto se organiza en varios directorios clave que agrupan diferentes funcionalidades y recursos:

*   **Módulos de Aventura (Ej: `beastmasters-daughter/`, `burning-goblins/`, `salorium/`):** Cada uno de estos directorios representa una aventura o un módulo de juego independiente. Contienen todos los recursos específicos necesarios para esa aventura, incluyendo archivos HTML para la estructura de la historia, archivos CSS para estilos únicos, JavaScript para lógicas de juego específicas, imágenes (mapas, personajes, escenarios) y archivos de audio (música ambiental, efectos de sonido).

*   **Componentes Centrales (`core/`):** Este directorio actúa como el repositorio principal para recursos compartidos y globales. Incluye:
    *   **CSS:** Estilos base, estilos de dados, de encuentros, de flujo de la historia, de índices y estilos de impresión (`basestyles.css`, `dice.css`, `encounter-styles.css`, etc.).
    *   **DNDWiki y SRD (inglés/español):** Contiene archivos HTML con la documentación de reglas de Dungeons & Dragons en inglés (`SRD5en/`) y español (`SRD5es/`), así como una wiki de D&D genérica (`dndwiki/`).
    *   **Fuentes (`fonts/`):** Tipografías personalizadas para la interfaz.
    *   **Imágenes (`images/`):** Recursos gráficos comunes utilizados en toda la aplicación.
    *   **JavaScript (`js/`):** Scripts globales que proporcionan funcionalidades transversales.
    *   **Música (`music/`):** Bandas sonoras y música ambiental general.
    *   **Portadas (`portadas/`):** Imágenes de portada para diferentes secciones o módulos.

*   **Servidor del Tablero Virtual (VTT) (`backend/`):** Este es el componente backend principal que orquesta la experiencia del juego en línea. Es una aplicación Node.js construida sobre el framework Feathers.js.

*   **Herramientas de Desarrollo (`develop/`):** Contiene herramientas como `map_creator.html` y sus scripts asociados, que facilitan la creación y extracción de contenido para nuevas aventuras o mapas.

## 2. Funcionamiento del Backend (`backend/`)

El servidor `backend` es crucial para la funcionalidad en tiempo real y la persistencia de datos:

*   **Framework:** Utiliza **Feathers.js** (con Express.js para rutas HTTP y Socket.io para WebSockets) para construir una API REST y una arquitectura de eventos en tiempo real.

*   **Punto de Entrada:** El servidor se inicia con `node index.js` (como se define en `package.json`).

*   **Servicio de Mensajes:** Un `MessageService` básico (`backend/index.js`) gestiona la comunicación dentro de cada sala, aunque se mantiene en memoria para cada instancia de sala.

*   **Comunicación en Tiempo Real (WebSockets):**
    *   Cada conexión de cliente se une a un canal llamado `everybody`.
    *   Todos los eventos (creación/actualización de datos) se publican en este canal, permitiendo la sincronización instantánea entre todos los participantes de una sala.

*   **Persistencia de Datos:**
    *   Utiliza **LevelDB** para almacenar datos de las salas de juego. La ruta `/save` (método `POST`) permite guardar información específica de la sala (ej. `room_id`, `background`) en la base de datos de clave-valor.

*   **Manejo de Rutas HTTP:**
    *   **Archivos Estáticos:** Sirve el contenido del directorio `frontend/` (HTML, CSS, JS, imágenes) como archivos estáticos.
    *   **Rutas Dinámicas de Salas:** Rutas como `/room/:roomId` o `/newroom/:roomId` permiten crear y acceder a salas de juego específicas. Al acceder, se sirve el archivo `frontend/game.html` o `frontend/new-game.html` y se inicializa un servicio de mensajes para esa sala.
    *   **Autenticación de ImageKit:** La ruta `/signature` (método `GET`) proporciona parámetros de autenticación necesarios para que el cliente interactúe directamente con ImageKit para la gestión de imágenes.

*   **Gestión de Imágenes:** Se integra con **ImageKit** para la carga, manipulación y entrega optimizada de imágenes, lo cual es fundamental para los mapas y recursos visuales del VTT.

## 3. Funcionamiento del Frontend (`frontend/`)

La interfaz de usuario es una aplicación web dinámica, interactiva y rica en funcionalidades, construida con las siguientes tecnologías:

*   **HTML:** Proporciona la estructura de las páginas. Los archivos clave incluyen `index.html` (página de inicio), `game.html` (interfaz principal de juego), `create-room.html`, `load.html`, `dice.html`, etc.

*   **CSS:** Se utiliza **Bulma** como framework CSS para un diseño base responsivo y modular, complementado con estilos personalizados (ej. `main.css`, `newmain.css`) y estilos adaptados de otras herramientas de D&D (ej. `5etools.css`, `dndbeyond.css`).

*   **JavaScript:** Es el motor de la interactividad, utilizando varias librerías de vanguardia:
    *   **Fabric.js:** Esencial para el **lienzo interactivo**. Permite a los Maestros de Juego y jugadores dibujar, mover y manipular objetos (tokens de personajes, elementos de mapa, efectos) directamente en el navegador, creando una experiencia de tablero dinámica.
    *   **Three.js y Cannon.js:** Posiblemente se utilizan para la **simulación de dados en 3D** con físicas realistas, así como para otros posibles efectos visuales o entornos 3D dentro del VTT.
    *   **Hammer.js:** Permite el **reconocimiento de gestos táctiles**, mejorando significativamente la usabilidad y la interactividad en dispositivos móviles.
    *   **wheel-zoom.js:** Proporciona funcionalidad de zoom suave para los mapas y el lienzo.
    *   **Lógica Principal:** Archivos como `main.js`, `intro.js`, `new-intro.js`, `dice.js`, `load.js` implementan la lógica del juego, la interacción con el servidor Feathers.js, la gestión de la UI y la carga de recursos.
    *   **language.js:** Gestiona la internacionalización de la aplicación.

## 4. Flujo General de una Aventura

1.  **Acceso:** Un usuario accede a la aplicación a través de la página de inicio (ej. `index.html`).
2.  **Creación/Unión a Sala:** Desde la interfaz, puede crear una nueva sala de juego (`create-room.html`) o unirse a una existente (`load.html`), lo que lo redirige a una URL específica de sala (ej. `/room/mi_sala`).
3.  **Interfaz de Juego:** Dentro de la sala, se carga la interfaz principal del VTT (`game.html` o `new-game.html`). El lienzo interactivo de Fabric.js se convierte en el centro de la acción.
4.  **Interacción en Tiempo Real:** Maestros de Juego y jugadores interactúan con el mapa, mueven tokens, lanzan dados (posiblemente en 3D), envían mensajes, etc. Todas estas acciones se sincronizan instantáneamente entre los participantes a través de WebSockets, gracias al servidor Feathers.js.
5.  **Persistencia:** Periódicamente, o cuando se realizan acciones clave, el estado de la sala (ej. la posición de los tokens, el mapa de fondo) se guarda en LevelDB a través de la ruta `/save` del backend, asegurando que el progreso no se pierda.
6.  **Recursos de Aventura:** Los recursos específicos de la aventura que se está jugando (imágenes, audio, lógica JS) se cargan dinámicamente desde los directorios de aventura correspondientes, integrándose en la experiencia general del VTT.

En resumen, Digital D20 es una plataforma robusta y bien pensada que combina un backend en tiempo real con un frontend interactivo para ofrecer una experiencia de juego de rol de mesa digital rica y fluida.
