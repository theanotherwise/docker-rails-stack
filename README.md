# Rails Application Stack

##  Prepare Environment
```bash
sysctl vm.overcommit_memory=1

echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

## Make Offline
```bash
docker-compose config | awk '{if ($1 == "image:") print $2;}'

for img in `docker-compose config | awk '{if ($1 == "image:") print $2;}'` ; do
    images="$images $img";
done

docker save -o services.img $images

docker load -i services.img

docker-compose up
```