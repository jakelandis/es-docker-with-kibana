### Usage

```bash
# rm -rf ./temp/es0 #only to start fresh
docker-compose up
curl -u elastic:changeme http://localhost:9200
open http://localhost:5601 #elastic/change 
```

### Data

The data directory is set to `./temp/es0`. 
Delete this directory for a clean start. Do not commit this to repository. 

### Keystore

The keystore is included here for simplicity. The only added entry
is `bootstrap.password` with the value `changeme`

`bin/elasticsearch-keystore add bootstrap.password` #changeme


### Troubleshooting

#### ES will not start up

The ES keystore will on ocassion change formats and embededing the keystore
like this does not play well when upgrading the ES docker image to 
a version where the keystore format changed. If this happens, the following
error will occur:

```
es0-1      | java.nio.file.FileSystemException: /usr/share/elasticsearch/config/elasticsearch.keystore.tmp -> /usr/share/elasticsearch/config/elasticsearch.keystore: Device or resource busy
es0-1      | 	at java.base/sun.nio.fs.UnixException.translateToIOException(UnixException.java:100)
es0-1      | 	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:106)
es0-1      | 	at java.base/sun.nio.fs.UnixFileSystem.move(UnixFileSystem.java:823)
es0-1      | 	at java.base/sun.nio.fs.UnixFileSystemProvider.move(UnixFileSystemProvider.java:289)
es0-1      | 	at java.base/java.nio.file.Files.move(Files.java:1316)
```

To fix, download the non-docker ES version locally. Unzip and run the following 
commands:

```
bin/elasticsearch-keystore create
bin/elasticsearch-keystore add bootstrap.password
bin/elasticsearch-keystore show bootstrap.password 
```

Copy and paste the keystore from the `config` directory to override and commit the one here. 
This is only necessary when upgrading the ES docker image to a version and
that version has changed the keystore format, which does not happen very often. 

