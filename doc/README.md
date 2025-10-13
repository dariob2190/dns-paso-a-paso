# Práctica DNS: Configuración de un servidor

## Sumario:

1. Configuraciones previas
2. Instalación del servidor
3. Configuración del servidor
4. Comprobaciones de las configuraciones, resoluciones y consultas
5. Comprobación usando _dig_
6. Comprobación usando _nslookup_
7. Cuestiones finales

## 1. Configuraciones previas

Vamos a empezar creando un directorio en Github.
Nos vamos a nuestro perfil de Github y pinchamos en "Repositories" o "Repositorios".

![Captura de mi perfil de Github](./capturas/captura1.png)

Una vez ahí hacemos clic en "New" o "Nuevo" para crear un nuevo repositorio.

![Captura de mis repositorios](./capturas/captura2.png)

Una vez aquí establecemos las preferencias de nuestro repositorio, en mi caso serán poner el nombre apropiado, hacerlo privado y crearé el README.md.

![Captura de la creación de un repositorio](./capturas/captura3.png)

Vamos a pasar ya a Visual Studio Code. Vamos a hacer un `git clone` (necesitamos tener Git previamente instalado y configurado) de nuestro repositorio (en mi caso será `git clone git@github.com:dariob2190/dns-paso-a-paso.git`) para poder tener una copia del repositorio en la carpeta donde ejecutemos el comando (en mi caso, Documentos).

Una vez lo hayamos copiado, abrimos la carpeta. Vamos a editar el README.md y lo subimos al repositorio.

![Captura de la edición del README.md](./capturas/captura4.png)

A continuación vamos a crear y editar el .gitignore y lo subimos también.

![Captura de la edición del .gitignore](./capturas/captura5.png)
