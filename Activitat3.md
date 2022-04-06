# ACTIVITAT Nº3 – InnoDB part II (1 punt)

**Font:** [MySQL :: MySQL 8.0 Reference Manual :: 15.8.1 InnoDB Startup Configuration](https://dev.mysql.com/doc/refman/8.0/en/innodb-init-startup-configuration.html)

- Partint de l&#39;esquema anterior configura el Percona Server perquè cada taula generi el seu propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vulgueu). Indica quins són els canvis de configuració que has realitzat.

        Per realitzar aquest punt, l'únic que cal fer és afegir la següent línia al fitxer my.cnf indicant la ruta de la carpeta on es volen guardar els tablespaces.
        'innodb_data_home_dir=/tspaces'

        Hem d'assegurar-nos que la carpeta existeix i que l'usuari mysql es el propietari.
        'mkdir /tspaces'
        'chown mysql:mysql /tspaces'
        
        Reiniciem el servei mysql.
        'service mysqld restart'

