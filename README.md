```
➜ cat docker-compose.yml    
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server
    restart: unless-stopped
    volumes:
      - data:/data
      - /mnt/backups/dir/bitwarden:/backups
    ports:
      - 127.0.0.1:8090:80
    environment:
      SIGNUPS_ALLOWED: 'false'   # set to false to disable signups(register) from any email address
      ADMIN_TOKEN: 'frGNWa2MnfQm+8apNAD2lS61u5WKBw3VbmomlLtk03hUB1zrHUABL/OoAWWX8VyN' # password for /admin url
      LOG_FILE: '/var/log/bitwarden.log'
      USE_SYSLOG: 'true'
      SIGNUPS_VERIFY: 'true' # email invitation activation link must be clicked before user can login 
      SIGNUPS_DOMAINS_WHITELIST: 'domain.com' # enable signup (register) for email users with @domain.com
      SHOW_PASSWORD_HINT: 'false' # from hardening guide https://github.com/dani-garcia/bitwarden_rs/wiki/Hardening-Guide-%28WIP%29
      DOMAIN: 'https://domain.com'
      SMTP_HOST: 'smtp.some-domain.org'
      SMTP_FROM: 'some-email'
      SMTP_PORT: '587'
      SMTP_SSL: 'true' 
      SMTP_USERNAME: ${SMTP_USERNAME}
      SMTP_PASSWORD: ${SMTP_PASSWORD}
    logging:
      options:
        tag: VAULTWARDEN
        max-file: "3"
        max-size: "10m"

volumes:
  data:

--

➜ cat docker-compose.backup.yml 
version: "3.9"
services:
  backup:
    image: ubuntu
    volumes:
      - data:/data
      - /mnt/backups:/backupmount
    command: tar --verbose --create --file /backupmount/vaultwarden/vaultwarden.tar /data

volumes:
  data:

--

➜ cat docker-compose.restore.yml 
version: "3.9"
services:
  restore:
    image: ubuntu
    volumes:
      - data:/data
      - /mnt/backups:/backupmount
    command: tar --verbose --extract --file /backupmount/vaultwarden/vaultwarden.tar

volumes:
  data:
```
