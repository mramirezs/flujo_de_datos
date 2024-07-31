# Flujo de Datos en Sistemas Operativos

## 1. Introducción

Este documento explica conceptos fundamentales sobre la manipulación de flujos de datos en sistemas operativos, incluyendo la redirección de entrada/salida estándar, el uso de tuberías, y la gestión de permisos de archivos.

## 2. Saberes Previos

1. **Salida y Entrada Estándar**: En los sistemas operativos, la salida estándar (`stdout`) es el flujo de datos donde los programas escriben sus resultados, mientras que la entrada estándar (`stdin`) es el flujo desde donde los programas reciben datos. El error estándar (`stderr`) es un flujo separado para mensajes de error.

2. **Permisos en Archivos**: Los sistemas de archivos gestionan permisos para usuarios y grupos, determinando qué acciones pueden realizarse sobre archivos y directorios.

## 3. Agenda

- Redirección de salida estándar.
- Redirección de entrada estándar.
- Uso de tuberías para la comunicación entre comandos.
- Gestión de usuarios y permisos de acceso.

## 4. Metacaracteres en Shell

Los metacaracteres son caracteres especiales que el shell interpreta de manera particular para realizar tareas como redireccionar flujos de datos o conectar comandos.

### Tabla de Metacaracteres

| Metacaracter | Descripción                                      |
|--------------|--------------------------------------------------|
| `|`          | Conecta la salida de un comando con la entrada de otro (tubería). |
| `>`          | Redirecciona la salida estándar a un archivo.    |
| `>>`         | Añade la salida estándar a un archivo sin sobrescribirlo. |
| `<`          | Redirecciona la entrada estándar desde un archivo. |
| `2>`         | Redirecciona el error estándar a un archivo.     |
| `&`          | Ejecuta un comando en segundo plano.            |
| `;`          | Separa comandos para ejecución secuencial.      |
| `&&`         | Ejecuta el siguiente comando solo si el anterior fue exitoso. |
| `||`         | Ejecuta el siguiente comando solo si el anterior falló. |

## 5. Redirección de Flujos de Datos

### 5.1 Redirección de Salida Estándar

Para redirigir la salida estándar a un archivo:

```bash
$ ls > file_list.txt
```
