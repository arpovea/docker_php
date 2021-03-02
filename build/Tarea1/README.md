Creamos el contenedor de mariadb y ejecutamos el script para cargar los datos:

```
docker run --name db_bookmedik \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=bookmedik \
-e MYSQL_USER=bookmedik_user \
-e MYSQL_PASSWORD=bookmedik_pass \
-v ~/Github/docker_php/build/Tarea1/database:/var/lib/mysql \
-d mariadb:latest
```

Editamos el fichero schema.sql para que no cree la base de datos bookmedik:

`/*create database bookmedik;*/`

Copiamos el el esquema en el contenedor:

`docker cp bookmedik/schema.sql db_bookmedik:/tmp`

Ejecutamos la carga de datos:

`docker exec db_bookmedik bash -c 'mysql -uroot -p"$MYSQL_ROOT_PASSWORD" < /tmp/schema.sql'`

Cambiamos la configuracion de "core/controller/Database.php":

```
<?php
class Database {
    public static $db;
    public static $con;
    function Database(){
        $this->user=getenv(ENV_MYSQL_USER);$this->pass=getenv(ENV_MYSQL_PASSWORD);$this->host=getenv(DB_PORT_3306_TCP_ADDR);$this->ddbb=getenv(ENV_MYSQL_DATABASE);
    }

    function connect(){
        $con = new mysqli($this->host,$this->user,$this->pass,$this->ddbb);
        $con->query("set sql_mode=");
        return $con;
    }

    public static function getCon(){
        if(self::$con==null && self::$db==null){
            self::$db = new Database();
            self::$con = self::$db->connect();
        }
        return self::$con;
    }

}
```
Creamos la configuracion de apache2:

```
<VirtualHost *:80>
  ServerAdmin me@mydomain.com
  DocumentRoot /var/www/bookmedik

  <Directory /var/www/bookmedik/>
      Options Indexes FollowSymLinks MultiViews
      AllowOverride All
      Order deny,allow
      Allow from all
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

```