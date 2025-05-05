# Mosquitto

https://github.com/BenJ1337/docker_ha_home-assistant-bootstrap

## Authentication methods
https://mosquitto.org/documentation/authentication-methods/

Creating a password file
```
docker exec ha-msqtt mosquitto_passwd -c -b /mosquitto/config/passwd.txt frigate frigate123456
```

Add more users
```
docker exec ha-msqtt mosquitto_passwd -b /mosquitto/config/passwd.txt frigate frigate123456
```

> Warning: File /mosquitto/config/passwd.txt has world readable permissions. Future versions will refuse to load this file.
> 
> To fix this, use `chmod 0700 /mosquitto/config/passwd.txt`.Warning: File /mosquitto/config/passwd.txt owner is not root. 
> Future versions will refuse to load this file.To fix this, use `chown root /mosquitto/config/passwd.txt`.
> 
> Warning: File /mosquitto/config/passwd.txt group is not root. Future versions will refuse to load this file.
