# 301023-Taller-de-Docker-de-Docker-Club-DevOps-JEspeche

**Este Stack fue creado por Joaquin Espeche para el curso de Docker del Club de Devops de la Facultad de Ingeniería UNLP.**

Esta es la lista de pasos que segui para realizar este stack.

-1 Primero creo la red en la que se va a comunicar todo el stack (MySQL, WordPress, phpMyAdmin), creo la red con el nombre de (ds-wordpress-6.1.1-net) y la defino con (driver: bridge) para que todos los contenedores de la misma red se puedan comunicar entre si.

-2 Luego dentro de services defino todos los contenedores que va a levantar la red y los configuro.

  -MySQL:Configurar y definino los parametros del mismo. 
  - (image: mysql:5.7) Defino la version de MySQL.
  - (container_name: ds-wordpress-6.1.1-mysql) Indico el nombre del contenedor (el cual se usara para la conexion con la base de datos).
  - (tty: true) Se habilita para poder conectar la base de datos desde un terminal y poder lanzar comando SQL.
  - (ports: - "4208:3306") Asignamos los puertos por el cual se conectara la aplicacion.
  - (volumes: - "./var/libclea/mysql/:/var/lib/mysql") Definimos un directorio donde se guardaran los datos de la base de datos, (esto es muy importante para que en caso de para la aplicacion no se restablezca los datos).
  - (environment) Creo variables de entorno para definir la contraseñas ROOT, crear la base de datos y un usuario y contraseña para la misma.
      - MYSQL_ROOT_PASSWORD: 1234 (Root Password).
      - MYSQL_DATABASE: wordpress (Database Name).
      - MYSQL_USER: miusuario (Database Username).
      - MYSQL_PASSWORD: mipassword (Databese User Password).
  - (networks: - ds-wordpress-6.1.1-net) Le asigno la red previamente ya creada.
    
  -WordPress: configuro y defino los parametros del mismo. 
  - (image: wordpress:latest) Defino la version de WordPress.
  - (container_name: ds-wordpress-6.1.1) Le asigno este nombre al contenedor.
  - (ports: - "4282:80") Asignamos los puertos por el cual se conectara la aplicacion.
  - (volumes: - "./var/www/html/:/var/www/html") Definimos un directorio donde se guardaran los datos persistentes de WordPress, (esto es muy importante para que en caso de para la aplicacion no se restablezca los datos).
  - (environment) Creo variables de entorno para nombrar la base de datos, asignar un usuario y una contraseña para la misma y designarle el host (En este caso ser MySQL).
      - WORDPRESS_DB_USER: miusuario (WordPress Database Username).
      - WORDPRESS_DB_PASSWORD: mipassword (WordPress Database Password).
      - WORDPRESS_DB_NAME: wordpress (WordPress Database Name).
      - WORDPRESS_DB_HOST: ds-wordpress-6.1.1-mysql (Database Host, llama al container previamente creado en la configuracion de MySQL)
  - (depends_on: - mysql) Creo una dependencia del contenedor WordPress hacia el contenedor MySQL
  - (networks: - ds-wordpress-6.1.1-net) Le asigno la red previamente ya creada.
    
  -phpMyAdmin: configuro y defino los parametros del mismo.
  - (image: phpmyadmin/phpmyadmin) Defino la version de phpMyAdmin.
  - (container_name: ds-phpmyadmin) Le asigno este nombre al contenedor.
  - (ports:  - "4283:80") Asignamos los puertos por el cual se conectara la aplicacion.
  - (environment) Creo variables de entorno, la primera para definir la ubicacion de la base de datos MySQL y la segunda es darle la contraseña root de la misma.
      -PMA_HOST: ds-wordpress-6.1.1-mysql (Database Host, llama al container previamente creado en la configuracion de MySQL).
      -MYSQL_ROOT_PASSWORD: 1234 (MySQL Root Password, colocamos la contraseña root de MySQL previamente colocada en la configuracion del servicio MySQL).
  - (depends_on: - mysql) Creo una dependencia del contenedor phpMyAdmin hacia el contenedor MySQL.
  - (networks: - ds-wordpress-6.1.1-net) Le asigno la red previamente ya creada.
    
-3 Una vez ya confugurado todos los servicios de nuestro Stack nos dirigimos al directorio de docker, donde tenemos el archivo docker-compose.yml y ejecutamos desde un terminal el siguiente comando ("sudo docker-compose up").

-4 Esperamos a que se levanten todos los servicios y nos dirigimos al navegador web a la dirección localhost:4282 (o al puerto que hayas especificado para WordPress).
            
           
            
            
            

          
