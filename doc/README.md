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

Vamos a crear el fichero Vagrantfile, para ello ejecutamos `vagrant init debian/bullseye64` (debemos instalar previamente vagrant) y esperamos a que nos cree el fichero.

![Captura de la creación del fichero Vagrantfile](./capturas/captura6.png)

Vamos a crear el fichero boostrap.sh. No lo completaremos ahora pero ya lo tendremos listo.

![Captura de la creación del fichero boostrap.sh](./capturas/captura7.png)

Ahora nos vamos a los hosts de nuestro equipo cliente y nos aseguramos que no tengamos nada adicional.

![Captura de los hosts de confianza del equipo cliente](./capturas/captura8.png)

## 2. Instalación del servidor

Comencemos a instalar todo lo necesario.
Para empezar vamos a conectarnos a la máquina de Vagrant con el comando `Vagrant ssh`.

![Captura de la conexión por SSH al servidor](./capturas/captura9.png)

Vamos a instalar Bind. Para ello vamos a ejecutar un `sudo apt install bind9 bind9utils bind9-doc` para instalarlo (puede ser necesario ejecutar `sudo apt update`).

![Captura de la instalación de Bind en el servidor](./capturas/captura10.png)

## 3. Configuración del servidor

Ahora vamos a editar `/etc/default/named`. Yo usaré `nano` que es el editor que más me gusta pero se puede utilizar cualquier otro.
Para editar el fichero primero necesitamos usar `sudo`para poder modificarlo. Ejecutariamos el comando tal que así `sudo nano /etc/default/named`.
Una vez dentro en la línea `OPTIONS=...` tenemos que añadir "-4" al final, quedando tal que así `OPTIONS="-u bind -4"`

![Captura de la edición del fichero "/etc/default/named"](./capturas/captura11.png)

Vamos ahora a editar el fichero `/etc/bind/named.conf.options`, pero al ser un fichero tan sensible es mejor que hagamos una copia de seguridad. Podemos hacerlo con el comando `sudo cp /etc/bind/named.conf.options /etc/bind/named.conf.backup`.

![Captura de la copia de seguridad de "/etc/bind/named.conf.options"](./capturas/captura12.png)

Vamos a entrar ahora en `/etc/bind/named.conf.options` y tenemos que dejarlo tal que así:

```
acl confiables {
        192.168.1.0/24;
};

options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        // forwarders {
        //      0.0.0.0;
        // };

        allow-transfer { none; };

        listen-on port 53 { 192.168.2.45; };

        recursion yes;
        allow-recursion { confiables; };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation yes;

        // listen-on-v6 { any; };
};
```

![Captura del nuevo contenido de "/etc/bind/named.conf.options"](./capturas/captura13.png)
