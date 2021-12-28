# Tareas cron

Una tarea cron es una tarea que se ejecuta a intervalos regulares de tiempo. Se ejecuta a nivel de sistema.

## Creacion de una tarea cron.
El directorio para crear una tarea cron es `/etc/cron.d`

1. Creamos un nuevo archivo en el directorio de cron. 
```bash
nano /etc/cron.d/tarea
```
<p align="center">
  <img width="866" height="269" src="https://raw.githubusercontent.com/w0lfst/Apuntes-Hacking-Etico/main/assets/images/cron.png">
</p>

En la imagen vemos la sintaxis correcta para ejecutar la tarea. En nuestro caso utilizaremos el siguiente ejemplo:
```bash
* * * * * root /home/w0lfst/file.sh
```
Esta tarea se ejecutara cada minuto. 
2. El arhivo `file.sh` no existe, vamos a crearlo:
```bash
nano /home/w0lfst/file.sh
```
3. Añadimos el siguiente codigo de ejemplo: 
```bash
#!/bin/bash

sleep 10
rm -r /tmp/
```
4. Iniciamos el servicio cron:
```bash
service cron start
``` 
