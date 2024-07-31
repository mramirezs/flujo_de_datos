# flujo_de_datos

## Saberes previos

1. ¿Qué entiendes como salida y entrada estándar en datos?

2. ¿Recuerdan los permisos en archivos?

## Agenda 

* Redirección de salida estándar.
* Redirección de entrada estándar.
* Comunicación entre comandos y uso de tuberías.
* Gestión de usuarios y permisos de accesos.

## Metacaracteres

Un metacaracter es un carácter escrito que tiene un significado especial para el shell que se usa para conectar comandos, realizar búsquedas específicas, etc.

| Figura 1

| Tabla 1

## Flujo de datos

* Cuando un programa o comando escribe el resultado de su ejecución en el monitor, está usando la salida estándar, en inglés standard output o stdout. A esto se le llama stream o flujo de datos o canal de datos.

* De igual forma, los datos de entrada a un programa o comando se suelen pasar por la entrada estándar, en inglés standard input o stdin.

* Cuando un programa o comando escribe un mensaje de error en el monitor, lo hace a través de otro canal de datos conocido como error estándar, en inglés standard error o stderr.

| Figura 2

* Cada uno de los flujos de datos vistos tiene asociado un número que se conoce como descriptor de archivo.

* Este descriptor es un índice en la tabla de descriptores que el sistema operativo utiliza para acceder a dichos flujos y archivos que se encuentran abiertos en un instante de tiempo.

* La entrada estándar tiene asociado el número 0, la salida estándar el valor 1 y el error estándar el índice 2.

* Una característica verdaderamente poderosa del shell es la capacidad de redirigir la entrada y salida de comandos hacia y desde otros comandos y archivos.

* Para permitir que los comandos puedan unirse mediante sus entradas y salidas, el shell utiliza metacaracteres.

* Un metacarácter es un carácter escrito que tiene un significado especial para el shell para conectar comandos o solicitar expansión.

* Los metacaracteres incluyen el carácter de barra vertical (|), ampersand (&), punto y coma (;), paréntesis derecho ()), paréntesis izquierdo ((), signo menor que (<) y signo mayor que (>).

## Redirección de salida estándar

* La mayoría de los programas de línea de comando que muestran sus resultados lo hacen enviándolos a la salida estándar. De forma predeterminada, la salida estándar dirige su contenido a la pantalla. Para redirigir la salida estándar a un archivo, se utiliza el metacarácter mayor «>».

´´´
$ ls > file_list.txt
´´´

* En este ejemplo, se ejecuta el comando ls y los resultados se escriben en un archivo llamado file_list.txt. Dado que la salida de ls se redirigió al archivo, no aparecen resultados en la pantalla. Cada vez que se repite el comando anterior, file_list.txt se sobrescribe con la salida del comando ls.

* Para que los nuevos resultados se agreguen al archivo, usamos los símbolos mayor mayor «>>».

´´´
$ ls >> file_list.txt
´´´

* Cuando se adicionan los resultados, los nuevos resultados se agregan al final del archivo, lo que hace que el archivo sea más largo cada vez que se repite el comando. 

* Si al ejecutar un comando se produce un error, dicho error por defecto también se mostrará en la pantalla, pero podemos redirigir estos mensajes a un archivo usando:

´´´
$ comando 2> archivoError
´´´

* Donde el valor 2 indica el descriptor del error estándar

## Redirección de entrada estándar

* Muchos comandos pueden aceptar entrada a partir de la entrada estándar. De forma predeterminada, la entrada estándar obtiene su contenido del teclado, pero al igual que la salida estándar, se puede redirigir.

* Para redirigir la entrada estándar de un archivo en lugar del teclado se utiliza el símbolo menor «<».

´´´
$ sort > file_list.txt
´´´

* En este ejemplo, usamos el comando sort para procesar el contenido del archivo file_list.txt. El resultado se muestra en la pantalla, ya que no se redirigió la salida estándar. Además, es posible redirigir la salida estándar a otro archivo.

## Comunicación entre comandos y uso de tuberías

### Tubería entre comandos

* El metacarácter de tubería, barra vertical (|), conecta la salida de un comando a la entrada del siguiente comando.

* Esto permite que un comando trabaje con algunos datos y luego el siguiente comando trabaje sobre los resultados del comando anterior sin necesidad de almacenar resultados intermedios en el sistema de archivos.

´´´
$cat /etc/passwd | sort | head -n 5
´´´

| Figura 3

### Ejecución secuencial de comandos

* En este tipo de ejecución, en lugar de escribir los comandos uno tras otro y esperar el fin de su ejecución antes de ejecutar el siguiente, es posible encadenar varios comandos en la misma orden, separándolos por un punto y coma (;).

´´´
$comando1;comando2;comando3
´´´

En el encadenamiento comando1; comando2; comando3, el comando comando2 se ejecuta al terminar el comando comando1; igualmente, el comando3 se ejecuta cuando comando2 ha terminado. No hay ningún vínculo entre estos tres comandos; es decir, la ejecución de un comando no está condicionada por el resultado o código de retorno del anterior.

### Ejecución condicional de comandos

* Al igual que en la ejecución secuencial, podemos ejecutar varios comandos en la misma orden, pero con la diferencia de que el comando siguiente se ejecutará en func ión de si el comando anterior final izó o no satisfactoriamente. Por lo tanto, en este tipo de ejecución nos encontramos con dos condiciones que dan lugar a dos subtipos de ejecuciones condicionales.

* Operador lógico Y (&&): En este tipo de ejecución, si queremos que el siguiente comando se ejecute, el comando anterior ha de finalizar exitosamente. Los comandos deberán ir separados por el operador AND lógico que se representa con dos signos ampersand (&&).

´´´
$comando1 && comando2 && comando3 &&...&& comandoN
´´´

* Operador lógico O (||): En este tipo de ejecución, si queremos que el siguiente comando se ejecute, el comando anterior no ha de finalizar exitosamente. Los comandos deberán ir separados por el operador OR lógico, que se representa por dos barras verticales (||). La sintaxis genérica para este tipo de ejecución es la siguiente:

´´´
$comando1 || comando2 || comando3 ||...|| comandoN 
´´´

* En el encadenamiento comando1 || comando2 || comando3, el comando2 se ejecutará solo si el comando 1 no fue exitoso e igualmente, comando3 se ejecutará si el comando2 no fue exitoso.

* Por ejemplo, supongamos que verificamos si el archivo miarchivo.txt existe, y si no existe, lo creamos. Para hacerlo, podemos ejecutar el siguiente comando:

´´´
$ [-f ~/miarchivo.txt] || touch ~/miarchivo.txt 
´´´

* También podemos combinar varios operadores en el mismo comando. Por ejemplo, si el archivo miarchivo.txt existe, mostraremos un mensaje indicando que el archivo existe y en caso de que no exista, crearemos el archivo.

´´´
$[ -f ~/miarchivo.txt ] && echo “El archivo miarchivo.txt existe” || touch ~/miarchivo.txt
´´´

### Ejemplos

* Crea el archivo "lista" con la salida del comando ls -l.

´´´
$ ls -l > lista
´´´

* Añade al archivo lista el contenido del directorio /etc.

´´´
$ls -l /etc >> lista  
´´´

* Muestra el contenido de lista página a página.

´´´
$cat < lista | more 
´´´

* Envía los mensajes de error al dispositivo nulo (a la basura).

´´´
$ls /NoEsta 2> /dev/null 
´´´

* Crear un archivo kk vacío

´´´
$ > kk
´´´

* Lee información del teclado, hasta que se teclea Ctrl-D; copia todo al archivo entrada.

´´´
$ cat > entrada
´´´

* Lee información del teclado, hasta que se introduce una línea con END; copia todo al archivo entrada.

´´´
$cat << END > entrada
´´´

* Redirige la salida estándar del comando al archivo salida y la salida de error al archivo error.

´´´
$ ls -l /bin/Bash /NoEsta > salida 2> error
´´´

## Gestión de usuarios y permiso de acceso

### Permisos de archivos

* El sistema operativo Linux es multiusuario, por tanto, las personas que lo usan deben identificarse para asegurar la confidencialidad de los datos contenidos en los archivos. Cada usuario del sistema dispone de una cuenta de usuario y está permitido compartir archivos entre colaboradores, por tanto, existe una noción de grupo de usuarios.

* Cada usuario es miembro obligatoriamente de un grupo que llamaremos grupo principal y es el utilizado al crear archivos. El usuario puede pertenecer a otros varios grupos: sus grupos secundarios determinan sus derechos de acceso a los archivos creados por otros miembros de los grupos. Los permisos de acceso a los archivos determinan las acciones que pueden emprender los usuarios.

* Los derechos de acceso en Linux, también conocidos como modos, se definen por tres niveles:
  
- Propietario o usuario
- Grupo
- Los otros

### Permisos del propietario

* El propietario es aquel usuario que genera o crea un archivo/directorio dentro de su directorio de trabajo, o en algún otro directorio sobre el que tenga permiso.

* Cada usuario tiene la potestad de crear, por defecto, los archivos que quiera dentro de su directorio de trabajo. En principio, él y solamente él será el que tenga acceso a la información contenida en los archivos y directorios que hay en su directorio HOME.

### Permisos del grupo 

* Lo más normal es que cada usuario pertenezca a uno o más grupos de trabajo. De esta forma, cuando se gestiona un grupo, se gestionan todos los usuarios que pertenecen a éste.

* Es decir, es más fácil integrar varios usuarios en un grupo al que se le conceden determinados privilegios en el sistema, que asignar los privilegios de forma independiente a cada usuario.

### Permisos del resto de usuarios

* Por último, también los privilegios de los archivos contenidos en cualquier directorio, pueden tenerlos otros usuarios que no pertenezcan al grupo de trabajo en el que está integrado el archivo en cuestión.

* Es decir, a los usuarios que no pertenecen al grupo de trabajo en el que está el archivo, pero que pertenecen a otros grupos de trabajo, se les denomina resto de usuarios del sistema.

### Manejo de permisos

* Cada archivo en Linux queda identificado por 10 caracteres a los que se les denomina máscara. De estos 10 caracteres, el primero (de izquierda a derecha) hace referencia al tipo de archivo. Los 9 siguientes, de izquierda a derecha y en bloques de 3, hacen referencia a los permisos que se le conceden, respectivamente, al propietario, al grupo y al resto u otros.

* El primer carácter de un archivo puede ser:

- '-' Archivo
- 'd' Directorio
- 'b' Archivo especial de bloques
- 'c' Archivo especial de caracteres
- 'l' Archivo de Enlace Simbólico
- 'p' Archivo especial de cauce o tubería (pipe)

* Los siguientes nueve caracteres son los permisos que se les concede a los usuarios en general del sistema. Los primeros tres caracteres se refieren a los permisos del propietario, los siguientes tres corresponden al grupo y los últimos tres caracteres son los permisos para el resto de los usuarios. Los caracteres que definen estos permisos son los siguientes:

- '-' Sin permiso
- 'r' Permiso de lectura (Read)
- 'w' Permiso de escritura (Write)
- 'x' Permiso de ejecución (eXecution)

* Existen diferencias en las acciones que se permiten dependiendo de si se trata de un archivo o de un directorio. 

#### Permisos para archivos:

- Lectura: permite visualizar el contenido del archivo.
- Escritura: permite modificar el contenido del archivo.
- Ejecución: permite ejecutar el archivo como si de un programa ejecutable se tratase.

#### Permisos para directorios: 

* Lectura: Permite saber qué archivos y directorios contiene el directorio que tiene este permiso.

* Escritura: permite crear archivos en el directorio, bien sea archivos ordinarios o directorios. Se pueden borrar directorios, copiar archivos en el directorio, mover, cambiar el nombre, etc.

* Ejecución: permite situarse sobre el directorio para poder examinar su contenido, copiar archivos de o hacia él. Si además se dispone de los permisos de escritura y lectura, se podrán realizar todas las operaciones posibles sobre archivos y directorios. Si no se dispone del permiso de ejecución, no podremos acceder a dicho directorio (aunque utilicemos el comando “cd”), ya que esta acción será denegada. También permite delimitar el uso de un directorio como parte de una ruta.

### Representación en octal de los permisos

* La representación octal (base ocho en lugar de base decimal) de los permisos es muy sencilla:

- Lectura tiene el valor de 4.
- Escritura tiene el valor de 2.
- Ejecución tiene el valor de 1.

* El valor del permiso para cada grupo expresado en octal es igual a la suma de los permisos que se otorgan sobre el archivo. En la Tabla podemos observar la equivalencia entre los permisos y su valor en octal.

| Figura 4

| Figura 5

## Ejemplos

* El usuario adiserio quien es el propietario del archivo prueba tiene permiso para lectura, escritura y ejecución sobre el archivo. Los usuarios que pertenecen al grupo bioinf disponen de permisos para lectura y escritura. El resto de los usuarios del sistema solo pueden tienen permiso de lectura.

´´´
-rwxrw-r-- 1 adiserio bioinf 4096 jun 30 19:30 prueba
´´´

* El usuario adiserio, propietario del directorio Bio, tiene permiso para ver el contenido del directorio (lectura), crear o borrar archivos en el directorio Bio (escritura) y situarse dentro del directorio Bio (ejecución). En cambio, los usuarios pertenecientes al grupo bioinf pueden ver el contenido del directorio (lectura) e ingresar al mismo (ejecución), pero no pueden modificar su contenido. El resto de los usuarios solo pueden ver parcialmente la información del contenido del directorio (lectura), pero con mensajes que indican que no se disponen de los permisos necesarios.

´´´
drwxr-xr-- 1 adiserio bioinf 4096 jun 30 19:30 Bio
´´´

## A practicar

| Figura 6

´´´
# Entrada estándar
$cat/etc/passwd 
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
´´´



