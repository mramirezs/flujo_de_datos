# Flujo de Datos en Sistemas Operativos Linux

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

| Comando            | file1.txt | file2.txt | file33.txt | fileA.txt | fileC.zip | fileB.txt | Output.pdf |
|--------------------|-----------|-----------|------------|-----------|-----------|-----------|------------|
| $ ls file*         |     X     |     X     |      X     |    X      |    X      |     X     |            |
| $ ls file?.txt     |     X     |     X     |            |    X      |           |     X     |            |
| $ ls file[AC].???  |           |           |            |    X      |    X      |           |            |
| $ ls file[A-C].??? |           |           |            |    X      |    X      |     X     |            |
| $ ls *.txt         |     X     |     X     |      X     |    X      |           |     X     |            |

## 5. Redirección de Flujos de Datos

Cuando un programa o comando escribe el resultado de su ejecución en el monitor, está usando la `salida estándar`, en inglés `standard output` o `stdout`. A esto se le llama `stream` o `flujo de datos` o `canal de datos`. De igual forma, los datos de entrada a un programa o comando se suelen pasar por la `entrada estándar`, en inglés `standard input` o `stdin`. Cuando un programa o comando escribe un `mensaje de error` en el monitor, lo hace a través de otro canal de datos conocido como `error estándar`, en inglés `standard error` o `stderr`.

![Flujo de datos](https://linuxhandbook.com/content/images/2020/06/Linux-redirection-normal-flow.png)

Cada uno de los flujos de datos vistos tiene asociado un número que se conoce como descriptor de archivo. Este descriptor es un índice en la tabla de descriptores que el sistema operativo utiliza para acceder a dichos flujos y archivos que se encuentran abiertos en un instante de tiempo. La entrada estándar tiene asociado el número `0`, la salida estándar el valor `1` y el error estándar el índice `2`.

Una característica verdaderamente poderosa del shell es la capacidad de redirigir la entrada y salida de comandos hacia y desde otros comandos y archivos. Para permitir que los comandos puedan unirse mediante sus entradas y salidas, el shell utiliza metacaracteres. Un metacarácter es un carácter escrito que tiene un significado especial para el shell para conectar comandos o solicitar expansión. Los metacaracteres incluyen el carácter de barra vertical (|), ampersand (&), punto y coma (;), paréntesis derecho ()), paréntesis izquierdo ((), signo menor que (<) y signo mayor que (>).

### 5.1 Redirección de Salida Estándar

La mayoría de los programas de línea de comando que muestran sus resultados lo hacen enviándolos a la salida estándar. De forma predeterminada, la salida estándar dirige su contenido a la pantalla. Para redirigir la salida estándar a un archivo, se utiliza el metacarácter mayor «>».

```bash
$ ls > file_list.txt
```
En este ejemplo, se ejecuta el comando ls y los resultados se escriben en un archivo llamado file_list.txt. Dado que la salida de ls se redirigió al archivo, no aparecen resultados en la pantalla. Cada vez que se repite el comando anterior, file_list.txt se sobrescribe con la salida del comando ls. Para que los nuevos resultados se agreguen al archivo, usamos los símbolos mayor mayor «>>».

```bash
$ ls >> file_list.txt
```

Cuando se adicionan los resultados, los nuevos resultados se agregan al final del archivo, lo que hace que el archivo sea más largo cada vez que se repite el comando. Si al ejecutar un comando se produce un error, dicho error por defecto también se mostrará en la pantalla, pero podemos redirigir estos mensajes a un archivo usando:

```bash
$ comando 2> archivoError
```
Donde el valor 2 indica el descriptor del error estándar.

### 5.1 Redirección de Entrada Estándar

Muchos comandos pueden aceptar entrada a partir de la entrada estándar. De forma predeterminada, la entrada estándar obtiene su contenido del teclado, pero al igual que la salida estándar, se puede redirigir. Para redirigir la entrada estándar de un archivo en lugar del teclado se utiliza el símbolo menor «<».

```bash
$ sort < file_list.txt
```

En este ejemplo, usamos el comando sort para procesar el contenido del archivo file_list.txt. El resultado se muestra en la pantalla, ya que no se redirigió la salida estándar. Además, es posible redirigir la salida estándar a otro archivo.

## 6. Comunicación entre comandos y uso de tuberías

### 6.1 Tubería entre comandos

El metacarácter de tubería, barra vertical (|), conecta la salida de un comando a la entrada del siguiente comando. Esto permite que un comando trabaje con algunos datos y luego el siguiente comando trabaje sobre los resultados del comando anterior sin necesidad de almacenar resultados intermedios en el sistema de archivos.

```bash
$cat /etc/passwd | sort | head -n 5
```

![Ejecución de comandos: Estructuras de control](https://procomsys.wordpress.com/wp-content/uploads/2018/05/escon.png)


### 6.2 Ejecución secuencial de comandos

En este tipo de ejecución, en lugar de escribir los comandos uno tras otro y esperar el fin de su ejecución antes de ejecutar el siguiente, es posible encadenar varios comandos en la misma orden, separándolos por un punto y coma (;).

```bash
$ comando1;comando2;comando3
```

En el encadenamiento comando1; comando2; comando3, el comando comando2 se ejecuta al terminar el comando comando1; igualmente, el comando3 se ejecuta cuando comando2 ha terminado. No hay ningún vínculo entre estos tres comandos; es decir, la ejecución de un comando no está condicionada por el resultado o código de retorno del anterior.

### 6.3 Ejecución condicional de comandos

Al igual que en la ejecución secuencial, podemos ejecutar varios comandos en la misma orden, pero con la diferencia de que el comando siguiente se ejecutará en func ión de si el comando anterior final izó o no satisfactoriamente. Por lo tanto, en este tipo de ejecución nos encontramos con dos condiciones que dan lugar a dos subtipos de ejecuciones condicionales.

**Operador lógico Y (&&):** En este tipo de ejecución, si queremos que el siguiente comando se ejecute, el comando anterior ha de finalizar exitosamente. Los comandos deberán ir separados por el operador AND lógico que se representa con dos signos ampersand (&&).

```bash
$ comando1 && comando2 && comando3 &&...&& comandoN
```

**Operador lógico O (||):** En este tipo de ejecución, si queremos que el siguiente comando se ejecute, el comando anterior no ha de finalizar exitosamente. Los comandos deberán ir separados por el operador OR lógico, que se representa por dos barras verticales (||). La sintaxis genérica para este tipo de ejecución es la siguiente:

```bash
$ comando1 || comando2 || comando3 ||...|| comandoN 
```

En el encadenamiento comando1 || comando2 || comando3, el comando2 se ejecutará solo si el comando 1 no fue exitoso e igualmente, comando3 se ejecutará si el comando2 no fue exitoso. Por ejemplo, supongamos que verificamos si el archivo miarchivo.txt existe, y si no existe, lo creamos. Para hacerlo, podemos ejecutar el siguiente comando:

```
$ [ -f ~/miarchivo.txt ] || touch ~/miarchivo.txt 
```

También podemos combinar varios operadores en el mismo comando. Por ejemplo, si el archivo miarchivo.txt existe, mostraremos un mensaje indicando que el archivo existe y en caso de que no exista, crearemos el archivo.

```
$[ -f ~/miarchivo.txt ] && echo “El archivo miarchivo.txt existe” || touch ~/miarchivo.txt
```

### 6.4 Ejemplos

```bash
# Crea el archivo "lista" con la salida del comando ls -l.

$ ls -l > lista

# Añade al archivo lista el contenido del directorio /etc.

$ ls -l /etc >> lista  

# Muestra el contenido de lista página a página.

$ cat < lista | more 

# Envía los mensajes de error al dispositivo nulo (a la basura).

$ls /NoEsta 2> /dev/null 

# Crear un archivo kk vacío

$ > kk

# Lee información del teclado, hasta que se teclea Ctrl-D; copia todo al archivo entrada.

$ cat > entrada

# Lee información del teclado, hasta que se introduce una línea con END; copia todo al archivo entrada.

$cat << END > entrada

# Redirige la salida estándar del comando al archivo salida y la salida de error al archivo error.

$ ls -l /bin/Bash /NoEsta > salida 2> error
```

## 7. Gestión de usuarios y permiso de acceso en el Sistema Operativo Linux

### 7.1 Permisos de archivos

El sistema operativo Linux es multiusuario, por tanto, las personas que lo usan deben identificarse para asegurar la confidencialidad de los datos contenidos en los archivos. Cada usuario del sistema dispone de una cuenta de usuario y está permitido compartir archivos entre colaboradores, por tanto, existe una noción de grupo de usuarios.

Cada usuario es miembro obligatoriamente de un grupo que llamaremos grupo principal y es el utilizado al crear archivos. El usuario puede pertenecer a otros varios grupos: sus grupos secundarios determinan sus derechos de acceso a los archivos creados por otros miembros de los grupos. Los permisos de acceso a los archivos determinan las acciones que pueden emprender los usuarios.

Los derechos de acceso en Linux, también conocidos como modos, se definen por tres niveles:
  
- Propietario o usuario
- Grupo
- Los otros

### 7.2 Permisos del propietario

El propietario es aquel usuario que genera o crea un archivo/directorio dentro de su directorio de trabajo, o en algún otro directorio sobre el que tenga permiso. Cada usuario tiene la potestad de crear, por defecto, los archivos que quiera dentro de su directorio de trabajo. En principio, él y solamente él será el que tenga acceso a la información contenida en los archivos y directorios que hay en su directorio HOME.

### 7.3 Permisos del grupo 

Lo más normal es que cada usuario pertenezca a uno o más grupos de trabajo. De esta forma, cuando se gestiona un grupo, se gestionan todos los usuarios que pertenecen a éste. Es decir, es más fácil integrar varios usuarios en un grupo al que se le conceden determinados privilegios en el sistema, que asignar los privilegios de forma independiente a cada usuario.

### 7.4 Permisos del resto de usuarios

Por último, también los privilegios de los archivos contenidos en cualquier directorio, pueden tenerlos otros usuarios que no pertenezcan al grupo de trabajo en el que está integrado el archivo en cuestión. Es decir, a los usuarios que no pertenecen al grupo de trabajo en el que está el archivo, pero que pertenecen a otros grupos de trabajo, se les denomina resto de usuarios del sistema.

### 7.5 Manejo de permisos

Cada archivo en Linux queda identificado por 10 caracteres a los que se les denomina máscara. De estos 10 caracteres, el primero (de izquierda a derecha) hace referencia al tipo de archivo. Los 9 siguientes, de izquierda a derecha y en bloques de 3, hacen referencia a los permisos que se le conceden, respectivamente, al propietario, al grupo y al resto u otros.

El primer carácter de un archivo puede ser:

- `-`: Archivo
- `d`: Directorio
- `b`: Archivo especial de bloques
- `c`: Archivo especial de caracteres
- `l`: Archivo de Enlace Simbólico
- `p`: Archivo especial de cauce o tubería (pipe)

Los siguientes nueve caracteres son los permisos que se les concede a los usuarios en general del sistema. Los primeros tres caracteres se refieren a los permisos del propietario, los siguientes tres corresponden al grupo y los últimos tres caracteres son los permisos para el resto de los usuarios. Los caracteres que definen estos permisos son los siguientes:

- `-`: Sin permiso
- `r`: Permiso de lectura (**r**ead)
- `w`: Permiso de escritura (**w**rite)
- `x`: Permiso de ejecución (e**x**ecution)

Existen diferencias en las acciones que se permiten dependiendo de si se trata de un archivo o de un directorio. 

#### 7.5.1 Permisos para archivos:

- **Lectura:** permite visualizar el contenido del archivo.
- **Escritura:** permite modificar el contenido del archivo.
- **Ejecución:** permite ejecutar el archivo como si de un programa ejecutable se tratase.

#### 7.5.2 Permisos para directorios: 

- **Lectura:** Permite saber qué archivos y directorios contiene el directorio que tiene este permiso.
- **Escritura:** permite crear archivos en el directorio, bien sea archivos ordinarios o directorios. Se pueden borrar directorios, copiar archivos en el directorio, mover, cambiar el nombre, etc.
- **Ejecución:** permite situarse sobre el directorio para poder examinar su contenido, copiar archivos de o hacia él. Si además se dispone de los permisos de escritura y lectura, se podrán realizar todas las operaciones posibles sobre archivos y directorios. Si no se dispone del permiso de ejecución, no podremos acceder a dicho directorio (aunque utilicemos el comando “cd”), ya que esta acción será denegada. También permite delimitar el uso de un directorio como parte de una ruta.

### Representación en octal de los permisos

La representación octal (base ocho en lugar de base decimal) de los permisos es muy sencilla:

- Lectura (`r`) tiene el valor de `4`.
- Escritura (`w`) tiene el valor de `2`.
- Ejecución (`x`) tiene el valor de `1`.

El valor del permiso para cada grupo expresado en octal es igual a la suma de los permisos que se otorgan sobre el archivo. En la Tabla podemos observar la equivalencia entre los permisos y su valor en octal.

![Manejo de permisos](https://xunilinux.wordpress.com/wp-content/uploads/2016/02/3.jpg)

![Estructura de permisos](https://naps.com.mx/blog/wp-content/uploads/2016/02/Permisos-en-sistema-de-archivos.jpg)

### 7.6 Ejemplos

El usuario adiserio quien es el propietario del archivo prueba tiene permiso para lectura, escritura y ejecución sobre el archivo. Los usuarios que pertenecen al grupo bioinf disponen de permisos para lectura y escritura. El resto de los usuarios del sistema solo pueden tienen permiso de lectura.

```bash
-rwxrw-r-- 1 adiserio bioinf 4096 jun 30 19:30 prueba
```

El usuario adiserio, propietario del directorio Bio, tiene permiso para ver el contenido del directorio (lectura), crear o borrar archivos en el directorio Bio (escritura) y situarse dentro del directorio Bio (ejecución). En cambio, los usuarios pertenecientes al grupo bioinf pueden ver el contenido del directorio (lectura) e ingresar al mismo (ejecución), pero no pueden modificar su contenido. El resto de los usuarios solo pueden ver parcialmente la información del contenido del directorio (lectura), pero con mensajes que indican que no se disponen de los permisos necesarios.

```bash
drwxr-xr-- 1 adiserio bioinf 4096 jun 30 19:30 Bio
```
## 8. A practicar

| Figura 6

```bash
# Entrada estándar
$cat /etc/passwd 
$cat < /etc/passwd 

# Salida estándar
$head –n 5 /etc/passwd > head_passwd 
$head –n 1 /etc/passwd > head_passwd 
$head –n 2 /etc/passwd >> head_passwd 

# Error estándar
$cat f20 $cat f20 2> error.log 
$cat f20 > salida.txt 2> error.log 
$cat salida.txt 
$cat error.log 
$cat head_passwd > salida.txt 2> error.log 
$cat salida.txt 
$cat error.log

# Tubería 
$head -n 5 /etc/passwd | tee head_passwd 
$ping www.google.com | tee ping.txt 
$cat ping.txt

# Ejecución secuencial 
$pwd 
$date 
$echo "Aprendiendo comandos" 
$pwd;date;echo "Aprendiendo comandos" 
$date;pwd;echo "Aprendiendo comandos" 
$date - ;pwd;echo "Aprendiendo comandos"

# Ejecución condicional
$head /etc/passwd && echo "Arriba Peru" 
$head /etc/shadow && echo "Arriba Peru" 
$head /etc/shadow || echo "Arriba Peru" 
$head /etc/shadow 2> error.log|| echo "Universidad Nacional Mayor de San Marcos"

# Ejecución secuencial
$date;pwd;ls > estado.txt 
$cat estado.txt 
$(date;pwd;ls) > estado.txt 
$cat estado.txt

# Permisos
$> foo.txt 
$ls -l foo.txt 
$chmod 600 foo.txt 
$ls -l foo.txt
```











