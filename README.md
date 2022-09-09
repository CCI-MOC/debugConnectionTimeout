connection debug

This is a project to highlight the connection issue that we have been coping with 
in the install and deploymoent of xdmod

* deploy the project

        oc apply -k k8s/kube-base

* get the xdmod-pod name

        oc get pods 

* replace xdmod-pod with the xdmod-pod name

        oc rsh xdmod-pod 

* then run

        % xdmod-init

Although 'xdmod-init' will fail with the connection_timeout, it doesn't do it as often as I would like

Here is an example output showing that this occasionally replicates the issue:

      Select an option \(1, 2, 3, 4, 5, 6, 7, 8, q\):

      Organization 
      running comand xdmod-setup
      Select an option \(1, 2, 3, 4, 5, 6, 7, 8, q\): 

      Organization Name: 

      Organization Abbreviation: 

      Overwrite config file '/etc/xdmod/organization.json' \(yes, no\)\? \[.*\]

      Press ENTER to continue.*

      Select an option \(1, 2, 3, 4, 5, 6, 7, 8, q\): 

      Create the file share database
      set server_root in /etc/httpd/conf/httpd.conf
        Ingesting resources
      tar: Removing leading `/' from member names
      Writing /etc/xdmod/etc_xdmod.b64 to db
      sh-4.2$ ETL\EtlOverseer: Error verifying data endpoints:
      ('Shredder/Staging Database', class=ETL\DataEndpoint\Mysql, config=database, schema=mod_shredder, host=mariadb:3306, user=xdmod): Error querying for schema 'mod_shredder' Exception: 'SQLSTATE[HY000] [2003] Can't connect to MySQL server on 'mariadb' (110)' Using DataEndpoint: '('Shredder/Staging Database', class=ETL\DataEndpoint\Mysql, config=database, schema=mod_shredder, host=mariadb:3306, user=xdmod)'
      #0 /usr/share/xdmod/classes/ETL/EtlOverseer.php(379): ETL\EtlOverseer->verifyDataEndpoints(Object(ETL\Configuration\EtlConfiguration), true)
      #1 /usr/share/xdmod/tools/etl/etl_overseer.php(608): ETL\EtlOverseer->execute(Object(ETL\Configuration\EtlConfiguration))
      #2 {main}

I'll include logs in the project.

* Database logging has been turned on.

  * log in to the maradb container:

        oc rsh mariadb-pod

  * General log (probably a query log):

        oc cat /var/log/mysql/mysql.log

  * Slow query log:

        oc cat /var/log/mysql/mariadb-slow.log


Something that worked around that may be related, line ~293 I had to put a sleep as 
it seem that a write to the attached volumn was failing.

After doing this, I relize there is more code that could be eliminated


