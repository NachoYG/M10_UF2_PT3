# ACTIVITAT Nº2 – InnoDB part I (2 punts)

**Font:** [MySQL :: MySQL 8.0 Reference Manual :: 15.8.1 InnoDB Startup Configuration](https://dev.mysql.com/doc/refman/8.0/en/innodb-init-startup-configuration.html)

- Desactiva l'opció que ve per defecte de innodb\_file\_per\_table.

        Primer canvio el valor de la variable global i comprovo que sigui correcte.
        'SET GLOBAL innodb_file_per_table=0;'
        'SHOW GLOBAL VARIABLES LIKE "innodb_file_per_table";'


 - Importa la BD Sakila com a taules InnoDB.

        Com en el cas anterior, primer canviem el tipus de taules del schema.
        'sed -i "s/MyISAM/InnoDB/" sakila-schema.sql'
        'cp sakila-schema.sql /home/asix/sakila.sql'
        'chmod 777 /home/asix/sakila.sql'
        
        Tot seguit, executem el fitxer per tal de carregar el schema de la BD.
        'SOURCE /home/asix/sakila.sql;'
        
        Comprovem el motor que utilitzen les taules.
        'SELECT table_name, engine
            FROM information_schema.tables
         WHERE table_schema="sakila";'

- Quin/quins són els fitxers de dades? A on es troben i quin és la seva mida?

        Es troben **/var/lib/mysql** i tots els fitxers o carpetes que inicien amb 'ib' pertanyen a InnoDB. L'arxiu principal és 'ibdata1'.



- Canviar la localització dels fitxers del tablespace de sistema per defecte a /discs-mysql/

         Primer de tot creo la carpeta /discs-mysql/ i li dono com a propietari l'usuari mysql.
         'mkdir /discs-mysql'
         'chown mysql:mysql /discs-mysql'
         
         Després, hem de editar el fitxer /etc/my.cnf i afegir la següent línia.
         '[mysqld]
          innodb_data_file_path=/discs-mysql/ibdata1'


- Tinguem dos fitxers corresponents al tablespace de sistema.

         Modifiquem la línia afegida de la següent manera.
         'innodb_data_file_path=/discs-mysql/ibdata1;/discs-mysql/ibdata2'


- Tots dos han de tenir la mateixa mida inicial (10MB)

         Editem la línia indicant la mida de 10MB en mode autoextend.
         'innodb_data_file_path=/discs-mysql/ibdata1:10M:autoextend;/discs-mysql/ibdata2:10:autoextend'


- El tablespace ha de créixer de 5MB en 5MB.

         Hem de afegir també la següent línia just a sota de l'anterior.
         'innodb_autoextend_increment=5M'



- Situa aquests fitxers (de manera relativa a la localització per defecte) en una nova localització simulant el següent:

_/discs-mysql/disk1/primer fitxer de dades → simularà un disc dur_

_/discs-mysql/disk2/segon fitxer de dades → simularà un segon disc dur._

         No entenc per que s'ha de fer amb ruta relativa, es a dir, si la ruta per defecte es /var/lib/mysql, quin sentit té que fiqui ../../../discs-mysql/disc1/2 ? Hauria de ser una ruta completa com ja s'indica, l'únic que fas és ficar els dos fitxer en carpetes diferents.
