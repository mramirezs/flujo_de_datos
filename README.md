# Flujo de Datos en Sistemas Operativos

## 1. Introducción

Explicaremos conceptos fundamentales sobre la manipulación de flujos de datos en sistemas operativos, incluyendo la redirección de entrada/salida estándar, el uso de tuberías, y la gestión de permisos de archivos.

## 2. Saberes Previos

1. **Salida y Entrada Estándar**: En los sistemas operativos, la salida estándar (`stdout`) es el flujo de datos donde los programas escriben sus resultados, mientras que la entrada estándar (`stdin`) es el flujo desde donde los programas reciben datos. El error estándar (`stderr`) es un flujo separado para mensajes de error.

2. **Permisos en Archivos**: Los sistemas de archivos gestionan permisos para usuarios y grupos, determinando qué acciones pueden realizarse sobre archivos y directorios.

## 3. Agenda

- Redirección de salida estándar.
- Redirección de entrada estándar.
- Uso de tuberías para la comunicación entre comandos.
- Gestión de usuarios y permisos de acceso.

## 4. Metacaracteres en Shell

Un metacaracter es un carácter escrito que tiene un significado especial para el shell que se usa para conectar comandos, realizar búsquedas específicas, etc.

### Tabla de Metacaracteres

| Metacaracter | Descripción                                      |
|--------------|--------------------------------------------------|
| `*`          | Coincide con cero o más caracteres. |
| `?`          | Coincide con un solo carcter. |
| `[]`         | Coincide con uno de los caracteres dentro de los corchetes. |
| `{}`         | Expande una lista de elementos. |
| `\|`         | Conecta la salida de un comando con la entrada de otro (tubería). |
| `>`          | Redirecciona la salida estándar a un archivo.    |
| `>>`         | Añade la salida estándar a un archivo sin sobrescribirlo. |
| `<`          | Redirecciona la entrada estándar desde un archivo. |
| `2>`         | Redirecciona el error estándar a un archivo.     |
| `&`          | Ejecuta un comando en segundo plano.            |
| `;`          | Separa comandos para ejecución secuencial.      |
| `&&`         | Ejecuta el siguiente comando solo si el anterior fue exitoso. |
| `\|\|`       | Ejecuta el siguiente comando solo si el anterior falló. |
| `!`          | Ejecuta comandos anteriores o niega una condición. |
| `~`          | Representa el directorio de inicio del usuario. |

### Ejemplos

```bash
*.txt # encuentra todos los archivos con extensión `.txt`.

?.txt # encuentra archivos como `a.txt` o `b.txt`.

[abc].txt # encuentra `a.txt`, `b.txt` y `c.txt`.

file{1,2,3}.txt # genera `file1.txt`, `file2.txt` y `file3.txt`.

ls -l | grep "archivo" # muestra detalles de los archivos cuyo nombre contiene "archivo".

echo "hola" > archivo.txt # escribe "hola" en `archivo.txt`.

echo "mundo" >> archivo.txt # añade "mundo" al final de `archivo.txt`.

sort < archivo.txt # ordena el contenido de `archivo.txt`.

comando & # ejecuta `comando` en segundo plano.

comando1;comando2 # ejecuta `comando1` y luego `comando2`.

comando1 && comando2

comando1 || comando2

!comando # ejecuta el comando anterior que no es `comando`.

cd ~ # te lleva al directorio de inicio.
```

## 5. Redirección de Flujos de Datos

### 5.1 Redirección de Salida Estándar

Para redirigir la salida estándar a un archivo:

```bash
$ ls > file_list.txt
```
