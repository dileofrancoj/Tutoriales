# Notas de Docker

<h2>¿Qué es Docker? ⚡︎🐳</h2>
<p>
Docker es un proyecto de código abierto que automatiza el despliegue de aplicaciones dentro de contenedores de software, proporcionando una capa adicional de abstracción y automatización de virtualización de aplicaciones en múltiples sistemas operativos
</p>

## Introducción a los comandos básicos 💻

<small> Se supone que si llegaste acá tenes instalado Docker... de no ser así podes entrar a: https://www.docker.com/ y descargarlo </small>

<p>Una vez instalado, podes ver todos los contenedores <strong>activos</strong> con el comando</p>

```bash
docker ps
```

<p>Para visualizar todas las imagenes creadas (activas / muertas): </p>
<small> Las imágenes de Docker son esencialmente una instantánea de un contenedor. Las imágenes se crean con el comando build
</small>

```bash
docker image ls
```

<p>Esto nos devuelve una tabla con: REPOSITORY, TAG, IMAGE ID, CREATED, SIZE </p>
<p> Esto es bastante descriptivo pero para aclararlo, el campo TAG nos habla de una referencia propia que le dimos a la imagen</p>

<p>Imaginemos que queremos descargar / correr una imagen (ej. Ubuntu) </p>

```bash
docker run ubuntu
```

<small> Si no existe la imagen en tu PC esto descarga la imagen del repositorio oficial hub.docker.com y por default pulleara la version latest que exista dentro del repo.</small>

<p> Por otra parte, podes correr un comando al correr una imagen poniendo docker run [imagen] [comando] </p>

```bash
docker run ubuntu ls -l
```

<h3>Para parar un contenedor que está corriendo basta con hacer un stop al ID </h3>

```bash
docker stop [id]
```

<small>
Una aclaración copada es que docker run levanta el contenedor mientras que exec se aplica a contenedores que <strong>YA ESTÁN CORRIENDO</strong>
</small>
<hr>
<h3>Instalando nginx como imagen en Docker </h3>

<p>Entrá a: <a href="https://hub.docker.com/">DockerHub</a>, busca Nginx </p>

```bash
docker run -d nginx:1.15.7
```

<p>Es una buena práctica especificar la versión de la imagen que estás descargando. El -d viene de deamon y es para que corra como proceso en background</p>

<p>Una vez que la imagen este corriendo, ejecutamos docker exect para ejecutar un comando dentro del contenedor que está corriendo </p>

```bash
docker exec -it [ID-Contenedor] bash
apt-get update #actualizamos el SO
apt-get install procps
ps fax
# tener en cuenta que solo se ven los procesos que corre el contenedor (en este caso nginx)
apt-get install curl
curl localhost
```

<h3>Volúmenes</h3>
<p>En Informática, se considera volumen el área de almacenamiento de un disco duro o de una de sus particiones, accesible mediante un formato consistente en un sistema de archivos.</p>

<p>En pocas palabras lo que vamos a hacer ahora es crear un volumen dentro de la imagen de Nginx que esté vinculado con una carpeta de nuestra PC</p>
<p>La idea es que yo pueda desarrollar en mi pc (entorno local / host), guardar los cambios y que estos se reflejen dentro del contenedor</p>

<ol>
    <li>1. Crea una carpeta que se llame "best-web" (o el nombre que quieras...)</li>
    <li>Dentro de esa carpeta crea un index.html</li>
    <li>Accede a la terminal</li>
</ol>

```bash
docker run -v ~/Desktop/best-web/index.html:/usr/share/nginx/html/index.html:ro -d nginx:1.15.7

#Estamos corriendo el contenedor apuntando nuestro index.html al directorio output de la imagen de nginx

#Si accedemos al contenedor y ejecutamos la terminal interactiva:
docker exec -it nginx:1.15.7 bash
cd /usr/share/nginx/html
cat index.html #vemos el contenido de nuestro index
exit
docker stop [ID]
```

<p>El próximo paso lógico es poder acceder al contendor desde afuera para que podamos desarrollar y visualizar los cambios en tiempo real. Vamos a exponer el puerto 80 que levanta nginx.</p>

```bash
docker run -v ~/Desktop/myWeb/index.html:/usr/share/nginx/html/index.html:ro -p 8080:80 -d nginx:1.15.7

# con esto lo que logramos es desde nuestro puerto 8080 (host) apuntar al puerto 80 del contenedor. Por lo que si abrimos nuestro localhost:8080 veremos nuestra web desde el web server nginx del contenedor.
```

<p>En caso que sea un build propio de un proyecto (React xej)</p>

```bash
docker run -v ~/Desktop/PUNCHIT/REACT/clase001/build/:/usr/share/nginx/html:ro -p 8080:80 -d nginx:1.19.6
```

<hr>
<h3>Ejemplo de como Dockerizar un proyecto de NodeJS</h4>
<p>El primer paso es asegurarse de tener express y express-generator instalados en la PC </p>

```bash
cd Desktop
express api-rest
touch Dockerfile
touch .dockerignore
```

```bash
vim Dockerfile
FROM node:12-alpine #punto de partida de la imagen
WORKDIR /app # work directory
COPY package*.json ./ #copiamos package.json y package-lock.json
RUN npm install #instalamos las dependencias
RUN npm install forever -g -y #instalamos forever para correr la aplicacion en segundo plano
COPY . .  #copiamos todo el proyecto al workdirectory
EXPOSE 30000 #habilitamos el puerto 3000 del contenedor
CMD forever -c 'node --harmony' ./bin/www #corremos forever
```

<p>Además, tenemos que tener en cuenta de no copiar los node_modules. Para eso, lo agregamos al .dockerignore

```bash
vim .dockerignore
node_modules
```

```bash
docker build -t [nombre-del-tag] . #folder . (local)
docker images # tendría que aparecer la imagen creada
docker run -p 4000:3000 -d [ID-imagen || nombre-del-tag] #-d para que corra en background. 4000 puerto de entrada del host y apunta al 3000 del contenedor
```

<p>Be happy 🤓</p>
<hr>

<h4>Aclaración 🎤 </h4>
<p>Este documento de notas son notas personales que saco de diferentes documentaciones, videos y prácticas que voy haciendo para incorporar conceptos basicos, intermedios y avanzados de <b>Docker</b>. </p>

<p> Esto no pretende ser una guia de Docker escrita por mi si no que es material que uso de consulta</p>

<p>PD: Si ves algun error conceptual o encontrás un error de ortografía, podes avisarme o hacer una pull request :D </p>

<h4>Autor: Franco Di Leo 🤓</h4>
