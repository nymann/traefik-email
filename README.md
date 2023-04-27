# My working email setup

1. Substitute my domain nymann.dev with yours. `grep -r nymann.dev .`


```sh
docker network create web
cd traefik
docker-compose up -d
cd ../roundcube
docker-compose up -d
cd ../mailserver
docker-compose up -d

# Generate DKIM
docker exec -ti mailserver setup config dkim keysize 2048
```

2. Set DNS values

```
TXT nymann.dev "v=spf1 mx ~all" 300
TXT _dmarc.nymann.dev "v=DMARC1; p=none; rua=mailto:dmarc.report@nymann.xyz; ruf=mailto:dmarc.report@nymann.xyz; sp=none; ri=86400"
TXT mail._domainkey.nymann.xyz (set to the value of mailserver/data/dms/config/opendkim/keys/nymann.dev/mail.txt (one line))
MX nymann.dev mail.nymann.dev 10
A mail.nymann.dev YOUR_IP
```
