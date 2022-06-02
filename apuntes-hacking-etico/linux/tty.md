# Tratamiento de la tty 

```bash
script /dev/null -c bash
CTRL + Z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

Debemos modificar las columnas y filas para que se muestre de manera correcta la propocion de nuestra terminal.
Para esto debemos ejecutar:
```bash
stty size
X X
```
Ajustamos la terminal con nuestro numero de filas y columnas 
```bash
stty rows X colums X 
```
> En X pondriamos el numero que nos muestre el comando ejecutado anteriormente
