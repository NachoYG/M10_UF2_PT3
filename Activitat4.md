# ACTIVITAT Nº4 – InnoDB part III

**Font:** [base de datos - Crear tablespace en Mysql - Stack Overflow en español](https://es.stackoverflow.com/questions/120734/crear-tablespace-en-mysql)

- Crea un tablespace anomenat &#39;ts1&#39; situat a /discs-mysql/disc1/ i col·loca les taules actor, address i category de la BD Sakila.

            Primer creem el tablespace indicant la ruta que es demana. Tot seguit afegim les taules indicades al enunciat.
            'CREATE TABLESPACE ts1 AND DATAFILE "/discs-mysql/disc1/ts1.ibd";'
            'ALTER TABLE sakila.actor TABLESPACE = ts1;'
            'ALTER TABLE sakila.address TABLESPACE = ts1;'
            'ALTER TABLE sakila.category TABLESPACE = ts1;'

- Crea un altre tablespace anomenat &#39;ts2&#39; situat a /discs-mysql/disc2/ i col·loca la resta de taules.

            Ara ens disposem a crear el segon tablespace.
            'CREATE TABLESPACE ts2 AND DATAFILE "/discs-mysql/disc1/ts2.ibd";'
            'ALTER TABLE sakila.city TABLESPACE = ts2;'
            'ALTER TABLE sakila.country TABLESPACE = ts2;'
            'ALTER TABLE sakila.customer TABLESPACE = ts2;'
            'ALTER TABLE sakila.film_actor TABLESPACE = ts2;'
            'ALTER TABLE sakila.film_category TABLESPACE = ts2;'
            'ALTER TABLE sakila.film_text TABLESPACE = ts2;'
            'ALTER TABLE sakila.inventory TABLESPACE = ts2;'
            'ALTER TABLE sakila.language TABLESPACE = ts2;'
            'ALTER TABLE sakila.payment TABLESPACE = ts2;'
            'ALTER TABLE sakila.rental TABLESPACE = ts2;'
            'ALTER TABLE sakila.staff TABLESPACE = ts2;'
            'ALTER TABLE sakila.store TABLESPACE = ts2;'


- Comprova que pots realitzar operacions DML a les taules dels dos tablespaces.

            He executat dos select per tal de comprovar si la sentència trobava la taula.
             'SELECT * FROM t1.actor;'
             'SELECT * FROM t2.store;'



- Quines comandes i configuracions has realitzat per fer els dos apartats anteriors?

            CREATE TABLESPACE
            ALTER TABLE
            SELECT

