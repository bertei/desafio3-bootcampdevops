# Desafio Devops

El proyecto contiene una aplicación básica con Node, Ngnix y MySQL.

Con cada actualización de la página, se registrará un nuevo registro en la base de datos y se mostrará en la lista, en la misma página.

El proyecto contiene algunas falencias y errores, analizar e implementar las correcciones correspondientes.

Si no entiendes algún concepto o parte del problema, ¡no hay de qué preocuparse! Queremos que aceptes el desafío hasta donde sepas.

### ¿Lo que debe hacerse? ### 

 - ajustes que hacen que todas las aplicaciones se levanten y se comuniquen
 - un README que contiene información sobre el problema a lo largo del proyecto para identificar y corregir errores

Comprométase en el camino para que podamos entender su forma de pensar!! :)

### Solución
1: El primer error que me encontré fue que la red "compose-network" estaba definida en los containers pero no creada. Por lo tanto, la cree haciendo:
    networks:
        node-network:
            driver: bridge
2: El segundo error tuvo que ver con el puerto que precisa el contenedor MYSQL, que no estaba definido.
    ports:
        - "3306:3306"
3: El tercer error tiene que ver con el container de la app NODE. En donde no estaba declarado el comando que instala las dependencias de la app desde el package manager NPM (definidas en package.json).
Ni tampoco el comando que ejecuta la aplicación (CMD ["node", "index.js"])
    RUN npm install
    CMD [ "node", "index.js" ]
4: El cuarto error es que la base de datos MYSQL creada tiene el nombre de "node_db" y en el archivo de la app NODE (connectionDB.js) esta establecido como "nodedb" sin el "_". De esta manera se puede conectar la app con la base de datos.
    database: process.env.DATABASE || 'node_db'
5: El último error esta relacionado con el webserver NGINX, en donde se establece la configuración en el archivo nginx.conf pero en el Dockerfile no se copia este archivo dentro de la carpeta de nginx. Por lo tanto, se agrega esta acción en el dockerfile.
    COPY nginx.conf /etc/nginx/conf.d
6: Agregar el archivo .gitignore y dentro:
    node/node_modules
    node/package-lock.json


 
  