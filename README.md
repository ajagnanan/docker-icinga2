# docker-icinga2
--------------------

This image configures an [Icinga2](https://www.icinga.org/icinga2) installation that runs within apache2. The container includes [supervisord](http://supervisord.org) to control the process.

### To run this image:

A docker-compose file is included as an example and can be used to run the container locally.

  ```
    docker-compose up
  ```

### Linking to a DB container

The `docker-compose` file shows how to link a mysql database to the container.

### Database migrations

The databases can be populated by running the commands below. These `DO NOT` have to be run manually as the container is configured to
run these scripts.

  ```
    mysql -h $IDO_DB_HOSTNAME -u $IDO_DB_USERNAME $IDO_DB_DATABASE --password=$IDO_DB_PASSWORD < /usr/share/icinga2-ido-mysql/schema/mysql.sql
    mysql -h $ICINGAWEB2_DB_HOSTNAME -u $ICINGAWEB2_DB_USERNAME --password=$ICINGAWEB2_DB_PASSWORD $ICINGAWEB2_DB_DATABASE < /usr/share/icingaweb2/etc/schema/mysql.schema.sql
  ```

Icinga-Web2 requires a user to be present in the database:

  ```
    ICINGAADMIN_PASSWORD=`openssl passwd -1 "PUT_ICINGAADMIN_PASSWORD_HERE"`
    mysql -h $ICINGAWEB2_DB_HOSTNAME -u $ICINGAWEB2_DB_USERNAME --password=$ICINGAWEB2_DB_PASSWORD $ICINGAWEB2_DB_DATABASE -e "INSERT IGNORE INTO icingaweb_user (name, active, password_hash) VALUES ('icingaadmin', 1, '${ICINGAADMIN_PASSWORD}');"
  ```

### SSH / Login

  ```
    Username: root
    Password: $ROOT_PASS
    Port: 22
  ```

  The web interface is available at `http://localhost`

### volumes

  ```
    /etc/icinga2
    /etc/icingaweb2
    /var/lib/icinga2
  ```

*** Image based on [korekontrol/icinga2](https://github.com/korekontrol/docker-icinga2) with some modifications.
