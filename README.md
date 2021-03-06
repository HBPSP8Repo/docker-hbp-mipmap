# Docker container for MIPMap

Use the following command to build the MIPMap image:

```sh
  $ ./build.sh
```

To use this image, you need a PostgreSQL database running.

A file named `mipmap_db.properties` should be located at /opt/mipmap_db.properties and contain the following:
```ini
  driver = org.postgresql.Driver
  uri = jdbc:postgresql://db:5432/
  username = mipmap
  password = mipmap
  mappingTask-DatabaseName = mipmap
```

Parameters 'username', 'password' and 'mappingTask-DatabaseName' need to be updated to match the PostgreSQL parameters.
The database is to be linked by default as 'db'. This can be changed in the 'uri' parameter above.

Alternatively to providing /opt/postgresdb.properties, you can supply the following environment properties to the container:

* MIPMAP_DB_DRIVER: Optional, JDBC driver to use to connect to the database. Defaults to 'org.postgresql.Driver'
* MIPMAP_DB_URL: Optional, JDBC url to use to connect to the database. Defaults to 'jdbc:postgresql://db:5432/'
* MIPMAP_DB_USER: Optional, user name used when connecting to the database. Defaults to 'mipmap'
* MIPMAP_DB_PASSWORD: Password used when connecting to the database.
* MIPMAP_DB_NAME: Optional, name of the database to connect to.  Defaults to 'mipmap'

MIPMap also expects a `map.xml` file at `/opt/source/map.xml`. The `map.xml` file refers to source files and target files with their relative path. These files should be located in the `source` and `target` folders, which are mounted in the container with relevant access rights. The output is generated in the `target/Target-translatedInstances0` subfolder.

Eventually, this container expects two folders and two files to be mapped:

```yml
  - "${mipmap_source}:/opt/source:ro"
  - "${mipmap_map}:/opt/map.xml:ro"
  - "${mipmap_pgproperties}:/opt/postgresdb.properties:ro"
  - "${mipmap_target}:/opt/target:rw"
```

The container contains demo files and can be tested with the following command:

```sh
$ mipmap_source=$(pwd)/source mipmap_map=$(pwd)/map.xml \
  mipmap_pgproperties=$(pwd)/postgresdb.properties \
  mipmap_target=$(pwd)/target mipmap_db=$(pwd)/data docker-compose up
```

It might be necessary to launch the container twice for the test to complete: a database has to be initialised the first time around, and this step is usually not finished fast enough for MIPMap to be able to connect.

## Push on Docker Hub

Run: `./docker_push.sh`
