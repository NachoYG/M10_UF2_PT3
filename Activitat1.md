# ACTIVITAT Nº1

- Indica quins són els motors d&#39;emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d&#39;aquesta.

        Amb la sentència 'SHOW ENGINES;' podem observar que el motors admesos són els que indiquen un 'YES' a la finestra 'Support'.

- Com puc saber quin és el motor d&#39;emmagatzematge per defecte? Mostra com canviar aquest paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor MyISAM?

        Amb la mateixa comanda anterior 'SHOW ENGINES;' podem veure que el motor actiu surt en 'DEFAULT'.
        Per canviar el motor per defecte, des de el servidor, ens dirigim al fitxer '/etc/my.cnf' afegim la següent línia 'default-storage-engine=MyISAM' i reiniciem el servei MySQL.
        O bé, també el podem canviar des de el SGBD amb la sentència 'SET default_storage_engine=MyISAM;'.


        Per últim comprovem que hagi canviat el motor per defecte amb ``SHOW ENGINES;``.


- Explica els passos per instal·lar i activar l&#39;ENGINE MyRocks. MyRocks és un motor d&#39;emmagatzematge per MySQL basat en RocksDB (SGBD incrustat de tipus clau-valor). Aquest tipus d&#39;emmagatzematge està optimitzat per ser molt eficient en les escriptures amb lectures acceptables.

          Per instal·lar el RocksDB executem desde el servidor 'yum install percona-server-rocksdb'.
         
          Un cop instal·lat, hem de executar 'ps-admin --enable-rocksdb -u asix -ppatata' per tal d'activar el rocksdb. 
          
          Com es pot observar, hem de fer servir un usuari administrador de mysql, en el meu cas, faig servir el usuari 'asix'; creat a la pràctica anterior.

          Comprovem amb la comanda 'SHOW ENGINES;' que s'hagi instal·lat correctament.


- Importa la BD Sakila com a taules MyISAM. Fes els canvis necessaris per importar la BD Sakila perquè totes les taules siguin de tipus MyISAM.

  _Mira quins són els fitxers físics que ha creat, quan ocupen i quines són les seves extensions. Mostra&#39;n una captura de pantalla i indica què conté cada fitxer._
  _Un cop fet això torna a deixar el motor InnoDB per defecte._

           Primer de tot, hem de descarregar el fitxer de la BD de Sakila.
           
           Un cop el fitxer descarregat, executem la següent comanda per transformar el tipus de taules
           'sed -i "s/InnoDB/MyISAM"'.
           
           Tot seguit, canviaré de path el fitxer i li donaré permisos per que pugui executar l'usuari asix.
           'cp sakila-schema.sql /home/asix/sakila.sql'
           'chmod 777 /home/asix/sakila.sql'
           
           Per últim, importem la base de dades transformada amb la seguent sentència SQL.
           'SOURCE /home/asix/sakila.sql;' 
           
           Des de el nostre SGBD podem observar amb la següent sentència quin motor utilitzen les taules del nou esquema afegit.


- A partir de MySQL apareixen els schemas de metadades i informació guardats amb InnoDB. Busca informació d&#39;aquests schemas. Indica quin és l&#39;objectiu de cadascun d&#39;ells i posa&#39;n un exemple d&#39;ús.

           Font: https://dev.mysql.com/doc/refman/8.0/en/information-schema.html


           Aquests schemas de metadades es denominen 'INFORMATION\_SCHEMA' i donen accés a les metadades de la BD; informació sobre el servidor, nom de la BD, privilegis...

           Podem trobar el següents INFORMATION\_SCHEMA:

                - Table reference: Dona informació variada de les o la BD.
                - General Tables: Dona informació general de les taules no associades a motors d'emmagatzematge, complements o components particulars d'una BD.
                - InnoDB Tables: Es pot fer servir per tal de supervisar activitat en curs, per tal de detectar ineficiències o per solucionar problemes de rendiment i capacitat.
                - Thread Pool Tables: Proporciona informació sobre el funcionament del grup de sub-processos.
                - Connection-Control Tables: Dona informació sobre les connexions de Login.
                - Firewall Tables: Proporciona informació sobre dades de memòria del Firewall.

- Posa un exemple que produeix un DEADLOCK i mostra-ho al professor.

           En primer lloc, creem una taula amb una fila i comencem una transacció.
           'CREATE TABLE t (i INT) ENGINE = InnoDB;'
           'INSERT INTO t(i) VALUES(1);'
           'START TRASACTION;'

           Després hem de indicar que volem que aquesta transacció sigui bloquejada en mode compartit.
           'SELECT * FROM t WHERE i = 1 LOCK IN SHARE MODE;'

           Des de un altra sessió, comencem una transacció i esborrem la fila de la taula.
           'START TRANSACTION;'
           'DELETE FROM t WHERE i = 1;'

           Intentem esborrar la fila des de la primera sessió.
           'DELETE FROM t WHERE i = 1;'

           Hem de rebre un missatge com aquets.
           'ERROR 1213 (40001): Deadlock found when trying to get lock;
            try restarting trasaction'




# ACTIVITAT Nº5 – REDOLOG (2 punts)

**Font:** [The relationship between Innodb Log checkpointing and dirty Buffer pool pages - Percona Database Performance Blog](https://www.percona.com/blog/2012/02/17/the-relationship-between-innodb-log-checkpointing-and-dirty-buffer-pool-pages/)

- Com podem comprovar (Innodb Log Checkpointing)

Amb la següent sentència.

![](RackMultipart20220403-4-ig9m1g_html_e85ed9087c5191bf.png)

Hem de buscar el apartat que fica LOG.

![](RackMultipart20220403-4-ig9m1g_html_b0913b5ce7aa8cda.png)

- LSN (Log Sequence Number)

Número de seqüència de logs.

![](RackMultipart20220403-4-ig9m1g_html_f44f3167293acac.png)

- L&#39;últim LSN actualitzat a disc

L&#39;últim Log escrit a disc.

![](RackMultipart20220403-4-ig9m1g_html_6b96656e67f161f2.png)

- Quin és l&#39;últim LSN que se li ha fet Checkpoint

![](RackMultipart20220403-4-ig9m1g_html_92abd1fb1e6fd8c1.png)

- Proposa un exemple a on es vegi l&#39;ús del redolog

Per exemple, si tenim un problema i se&#39;ns apaga repentinament el servidor amb sentències executant-se, el sistema de redolog s&#39;encarregarà de, a través de logs, guardar les sentències i els seus resultats per tal de que al moment d&#39;aixecar la màquina de nou, no es perdi consistència a les dades.

- Com podem mirar el número de pàgines modificades (dirty pages)? I el número total de pàgines?

![](RackMultipart20220403-4-ig9m1g_html_11833662130272d3.png)

# ACTIVITAT Nº6 – Implantar BD Distribuïdes (1,5 punts)

- Prepara un Servidor Percona Server amb la BD de Sakila.
- Prepara un segon servidor Percona Server a on hi hauran un conjunt de taules FEDERADES al primer servidor.
- Per realitzar aquest link entre les dues BD podem fer-ho de dues maneres:

_ **Opció 1:** _ _especificar TOTA la cadena de connexió a CONNECTION._

_ **Opció 2:** _ _especificar una connexió a un server a CONNECTION que prèviament s&#39;ha creat mitjançant CREATE SERVER._

- Posa un exemple de 2 taules de cada opció.
- Detalla quines són els passos i comandes que has hagut de realitzar en cada màquina.

# ACTIVITAT Nº7 – Storage Engine CSV (0,5 punts)

- Documenta i posa exemple de com utilitzar ENGINE CSV.

_Cal documentar els passos que has hagut de realitzar per preparar l&#39;exemple: configuracions, instruccions DML, DDL, etc...._

# ACTIVITAT Nº8 – Storage Engine MyRocks (1 punt)

- Documenta i posa exemple de com utilitzar ENGINE MyRocks. Crea una Base de dades amb 2 o 3 taules i inserta contingut.

_Cal documentar els passos que has hagut de realitzar per preparar l&#39;exemple: configuracions, instruccions DML, DDL, etc...._

- A quin directori es guarden els fitxers de dades? Fes un llistat de a on són els fitxers i què ocupen.
- Quina és la compressió per defecte que utilitza per les taules? Com ho faries per canviar-lo. Per exemple utilitza Zlib o ZSTD o sense compressió.
