# ACTIVITAT Nº5 – REDOLOG (2 punts)

**Font:** [The relationship between Innodb Log checkpointing and dirty Buffer pool pages - Percona Database Performance Blog](https://www.percona.com/blog/2012/02/17/the-relationship-between-innodb-log-checkpointing-and-dirty-buffer-pool-pages/)

- Com podem comprovar (Innodb Log Checkpointing)

        Amb la següent sentència.
          'SHOW ENGINE INNODB STATUS;'
          
        Hem de buscar el apartat que fica LOG.

- LSN (Log Sequence Number)
        
        Número de seqüència de logs.
            'Log sequence number      43106086'


- L'últim LSN actualitzat a disc
        L'últim Log escrit a disc.
            'Log written up to          43106086'
        

- Quin és l'últim LSN que se li ha fet Checkpoint
        Últim checkpoint
            'Last checkpoint at             43106086'

- Proposa un exemple a on es vegi l'ús del redolog

        Per exemple, si tenim un problema i se&#39;ns apaga repentinament el servidor amb 
        sentències executant-se, el sistema de redolog s'encarregarà de, a través de logs, guardar les sentències 
        i els seus resultats per tal de que al moment d'aixecar la màquina de nou, no es perdi consistència a les dades.

- Com podem mirar el número de pàgines modificades (dirty pages)? I el número total de pàgines?
               
        Número de dirty pages afegides
            'Added dirty pages uo to         43106086'


