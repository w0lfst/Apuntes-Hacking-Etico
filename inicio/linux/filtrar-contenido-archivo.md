# 🗃 Filtrar contenido arhivo

Filtramos contenido de un archivo (passwd) con grep:

* Filtrar por todo aquello que acabe en ... (sh) (añadimos el dolar a la palabra que estamos buscando):

```bash
grep "sh$"
```

* Extraer usuarios del passwd \`

```bash
cat /etc/passwd | grep "sh$ | awk '{print $1}' FS = ":"
```

* `'{print $1}'` Imprime el primer valor.
* `FS` Teniendo en cuenta el limitador (:)

(Buscar en un fichero que empiece y termine por pepito, `grep "^pepito$" -n (ver linea)`)

***

Tambien se puede realizar con **cut**

```bash
cut -d':' -f 1
```

* `-d` Delimitador
* `-f` Filtrar por campo
