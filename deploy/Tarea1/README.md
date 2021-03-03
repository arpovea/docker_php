Aqui se encuentra el docker-compose con las variables necesarias para levantar el escenario y la imagen que se ha creado en el directorio build de la "Tarea1".

Un punto a comentar de este compose es en servicio "db" en el contenedor de mariadb, al cual se le agrega un vólumen en el cual se le especifica el "schema.sql" para que lo cargue automáticamente.

```
    volumes:
      - /home/adri/GitHub/docker_php/build/Tarea1/bookmedik/schema.sql:/docker-entrypoint-initdb.d/datadump.sql
```