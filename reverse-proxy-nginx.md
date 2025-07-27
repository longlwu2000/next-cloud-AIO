

###Edit config.php
sudo docker run -it --rm --volume nextcloud_aio_nextcloud:/var/www/html:rw alpine sh -c "apk add --no-cache nano && nano /var/www/html/config/config.php"

###Config network
sudo nano /etc/dnsmasq.conf
sudo nano /etc/hosts

###Nếu lỗi k thể truy cập trong mạng nội bộ (Nat Loopback) thì edit file daemon.json trong docker
{
  "dns": ["<ip router DNS nội bộ>","8.8.8.8", "8.8.4.4"]
}

### Chạy lệnh này để run mastercontainer với dns 8.8.8.8
sudo docker run \
--init \
--sig-proxy=false \
--name nextcloud-aio-mastercontainer \
--restart always \
--publish 8080:8080 \
--dns 8.8.8.8 \
--env APACHE_PORT=11000 \
--env APACHE_IP_BINDING=0.0.0.0 \
--env APACHE_ADDITIONAL_NETWORK="" \
--env SKIP_DOMAIN_VALIDATION=true \
--env NEXTCLOUD_DATADIR="/mnt/nextcloudHDD/nextcloudData" \
--volume nextcloud_aio_mastercontainer:/mnt/docker-aio-config \
--volume /var/run/docker.sock:/var/run/docker.sock:ro \
ghcr.io/nextcloud-releases/all-in-one:latest
