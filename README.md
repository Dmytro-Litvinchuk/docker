# docker
Example of docker-compose include mailhog, mysql 5.7, php 7.2-apache-buster
PHP image include PDO MySql, mysqli, configured xDebug and configured MailHog.
For first using just execute:
<pre>docker-compose up -d</pre>
In case if you something changed in docker file and would like to rebuild:
It'll remove DB and images<pre>docker-compose down</pre>
Rebuild image <pre>docker-compose up --build -d</pre> (also can be with flag no cache)

You can install DB by command into docker
<pre>cat {dump.sql} | docker exec -i {id container of MySQL} /usr/bin/mysql -u {root user name} --password={root user password} {DB name}</pre>
Where {value} - your variable.
