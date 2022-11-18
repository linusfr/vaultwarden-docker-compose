# vaultwarden docker-compose

Simple docker-compose setup for vaultwarden.

## Backup
```
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
```

## Restore
```
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
