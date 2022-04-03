# ACTIVITAT Nº1 (1punt)

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
           
           Un cop el fitxer descarregat, executem la següent comanda per transformar el tipus de taules.
           
           Tot seguit, canviaré de path el fitxer i li donaré permisos per que pugui executar l&#39;usuari asix.
           
           Per últim, importem la base de dades transformada.
            
           Des de el nostre SGBD podem observar amb la següent sentència quin motor utilitzen les taules del nou esquema afegit.


- A partir de MySQL apareixen els schemas de metadades i informació guardats amb InnoDB. Busca informació d&#39;aquests schemas. Indica quin és l&#39;objectiu de cadascun d&#39;ells i posa&#39;n un exemple d&#39;ús.

**Font:** [https://dev.mysql.com/doc/refman/8.0/en/information-schema.html](https://dev.mysql.com/doc/refman/8.0/en/information-schema.html)

Aquests schemas de metadades es denominen &#39;INFORMATION\_SCHEMA&#39; i donen accés a les metadades de la BD; informació sobre el servidor, nom de la BD, privilegis...

Podem trobar el següents INFORMATION\_SCHEMA:

- **Table reference:** Dona informació variada de les o la BD.
- **General Tables:** Dona informació general de les taules no associades a motors d&#39;emmagatzematge, complements o components particulars d&#39;una BD.
- **InnoDB Tables:** Es pot fer servir per tal de supervisar activitat en curs, per tal de detectar ineficiències o per solucionar problemes de rendiment i capacitat.
- **Thread Pool Tables:** Proporciona informació sobre el funcionament del grup de sub-processos.
- **Connection-Control Tables:** Dona informació sobre les connexions de Login.
- **Firewall Tables:** Proporciona informació sobre dades de memòria del Firewall.

- Posa un exemple que produeix un DEADLOCK i mostra-ho al professor.

En primer lloc, creem una taula amb una fila i comencem una transacció.



Després hem de indicar que volem que aquesta transacció sigui bloquejada en mode compartit.



Des de un altra sessió, comencem una transacció i esborrem la fila de la taula.



Intentem esborrar la fila des de la primera sessió.


Rebem.


# ACTIVITAT Nº2 – InnoDB part I (2 punts)

**Font:** [MySQL :: MySQL 8.0 Reference Manual :: 15.8.1 InnoDB Startup Configuration](https://dev.mysql.com/doc/refman/8.0/en/innodb-init-startup-configuration.html)

- Desactiva l&#39;opció que ve per defecte de innodb\_file\_per\_table.

Primer canvio el valor de la variable global i comprovo que sigui correcte.

![](RackMultipart20220403-4-ig9m1g_html_254dfb542c9e662c.png)

- Importa la BD Sakila com a taules InnoDB.

Com en el cas anterior, primer canviem el tipus de taules del schema.

![](RackMultipart20220403-4-ig9m1g_html_d3bb4a0d5776fdf5.png)

Tot seguit, executem el fitxer per tal de carregar el schema de la BD.

![](RackMultipart20220403-4-ig9m1g_html_e5c0865b13aab44c.png)

Comprovem el motor que utilitzen les taules.

![](RackMultipart20220403-4-ig9m1g_html_b715f6d7e0cf3495.png) ![](RackMultipart20220403-4-ig9m1g_html_d41fd6c1c22438c6.png)

- Quin/quins són els fitxers de dades? A on es troben i quin és la seva mida?

Es troben **/var/lib/mysql** i tots els fitxers o carpetes que inicien amb &#39; **ib**&#39; pertanyen a InnoDB. L&#39;arxiu principal és ibdata1.

![](RackMultipart20220403-4-ig9m1g_html_e9811ebf5263e231.png)

- Canviar la localització dels fitxers del tablespace de sistema per defecte a /discs-mysql/

Primer de tot creo la carpeta /discs-mysql/ i li dono com a propietari l&#39;usuari mysql.

![](RackMultipart20220403-4-ig9m1g_html_513ad129dfb0c11c.png)

Després, hem de editar el fitxer /etc/my.cnf i afegir la següent línia.

![](RackMultipart20220403-4-ig9m1g_html_23f335e0daa9878c.png)

- Tinguem dos fitxers corresponents al tablespace de sistema.

Modifiquem la línia afegida de la següent manera.

![](RackMultipart20220403-4-ig9m1g_html_88ba84fe58d6dc87.png)

- Tots dos han de tenir la mateixa mida inicial (10MB)

Editem la línia indicant la mida de 10MB en mode autoextend.

![](RackMultipart20220403-4-ig9m1g_html_1b561c57ae6e0c25.png)

- El tablespace ha de créixer de 5MB en 5MB.

Hem de afegir també la següent línia.

![](RackMultipart20220403-4-ig9m1g_html_ef927eac3bfeaba9.png)

- Situa aquests fitxers (de manera relativa a la localització per defecte) en una nova localització simulant el següent:

_/discs-mysql/disk1/primer fitxer de dades → simularà un disc dur_

_/discs-mysql/disk2/segon fitxer de dades → simularà un segon disc dur._

No entenc per que s&#39;ha de fer amb ruta relativa, es a dir, si la ruta per defecte es /var/lib/mysql, quin sentit té que fiqui ../../../discs-mysql/disc1/2 ? Hauria de ser una ruta completa com ja s&#39;indica, l&#39;únic que fas és ficar els dos fitxer en carpetes diferents.

![](RackMultipart20220403-4-ig9m1g_html_e8097f8b7afffc14.png)

#

# ACTIVITAT Nº3 – InnoDB part II (1 punt)

**Font:** [MySQL :: MySQL 8.0 Reference Manual :: 15.8.1 InnoDB Startup Configuration](https://dev.mysql.com/doc/refman/8.0/en/innodb-init-startup-configuration.html)

- Partint de l&#39;esquema anterior configura el Percona Server perquè cada taula generi el seu propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vulgueu). Indica quins són els canvis de configuració que has realitzat.

Per realitzar aquest punt, l&#39;únic que cal fer és afegir la següent línia al fitxer my.cnf indicant la ruta de la carpeta on es volen guardar els tablespaces.

![](RackMultipart20220403-4-ig9m1g_html_32dad32ed73b0811.png)

Hem d&#39;assegurar-nos que la carpeta existeix i que l&#39;usuari mysql es el propietari.

![](RackMultipart20220403-4-ig9m1g_html_7e1c0b527da85cd6.png)

Reiniciem el servei mysql.

![](RackMultipart20220403-4-ig9m1g_html_ac2782d64f574a03.png)

# ACTIVITAT Nº4 – InnoDB part III (1 punt)

**Font:** [base de datos - Crear tablespace en Mysql - Stack Overflow en español](https://es.stackoverflow.com/questions/120734/crear-tablespace-en-mysql)

- Crea un tablespace anomenat &#39;ts1&#39; situat a /discs-mysql/disc1/ i col·loca les taules actor, address i category de la BD Sakila.

Primer creem el tablespace indicant la ruta que es demana. Tot seguit afegim les taules indicades al enunciat.

![](RackMultipart20220403-4-ig9m1g_html_bce71f4e20f3e1cb.png)

- Crea un altre tablespace anomenat &#39;ts2&#39; situat a /discs-mysql/disc2/ i col·loca la resta de taules.

Ara ens disposem a crear el segon tablespace.

![](RackMultipart20220403-4-ig9m1g_html_c2b752dcbc19bfa.png)

- Comprova que pots realitzar operacions DML a les taules dels dos tablespaces.

He executat dos select per tal de comprovar si la sentència trobava la taula.

![](RackMultipart20220403-4-ig9m1g_html_4a079c8e81b21b7.png)

Efectivament, les detecta. Son buides ja que aparentment, a l&#39;hora d&#39;importar la BD no s&#39;afegeixen dades a les taules.

![](RackMultipart20220403-4-ig9m1g_html_365808006e15bc09.png)

![](RackMultipart20220403-4-ig9m1g_html_e6d4a1241c11f693.png)

- Quines comandes i configuracions has realitzat per fer els dos apartats anteriors?

Les indicades a les captures.

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
