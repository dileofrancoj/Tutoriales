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
<small>
** Las imágenes de Docker son esencialmente una instantánea de un contenedor. Las imágenes se crean con el comando build
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

<small>
Una aclaración copada es que docker run levanta el contenedor mientras que exec se aplica a contenedores que <strong>YA ESTÁN CORRIENDO</strong>
</small>

<hr>

<h4>Aclaración 🎤 </h4>
<p>Este documento de notas son notas personales que saco de diferentes documentaciones, videos y prácticas que voy haciendo para incorporar conceptos basicos, intermedios y avanzados de <b>Docker</b>. </p>

<p> Esto no pretende ser una guia de Docker escrita por mi si no que es material que uso de consulta</p>

<h4>Autor: Franco Di Leo 🤓</h4>
