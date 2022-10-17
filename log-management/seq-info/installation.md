# *installing seq using docker*
```
PH=$(echo '<password>' | docker run --rm -i datalust/seq config hash)

mkdir -p <local path to store data>

docker run \
  --name seq \
  -d \
  --restart unless-stopped \
  -e ACCEPT_EULA=Y \
  -e SEQ_FIRSTRUN_ADMINPASSWORDHASH="$PH" \
  -v <local path to store data>:/data \
  -p 80:80 \
  -p 5341:5341 \
  datalust/seq
```
* You need to set your own password instead of `<password>` and also the path for volume binding seq configuration files.  
* Only one running Seq container can access a storage volume concurrently,  
it's not possible to run two or more container instances pointing to the same storage.  
