# telematicaFinal
laboratorio 3 telematica


## Creación de Bucket S3
Para la creación del bucket se entró al menú de S3 en AWS y se seleccionó en Create Bucket. Se creó el bucket con esta información:  
![alt text](/lab0/img/bucket.jpg "bucket")  
Luego se ingresó al bucket y se crearon directorios para manejar la información que se iba a subir. Dentro del bucket se seleccionó Create folder y se ingresó esta información.  
![alt text](/lab0/img/carpeta.jpg "folderraw")  
Dentro de la carpeta creada se seleccionó la opción de Upload para subir la información de los datasets  
![alt text](/lab0/img/archivoscargados.jpg "upload")  
Cuando se subieron los archivos había unos que no se necesitaban entonces se seleccionaron para eliminarlos y luego se subió la información.  Acá se dejaron solo los archivos necesarios, de estos hubo que realizar algunas movidas con los datasets entre ellos, descomprimir zips y entrrar a descargar archivos.

## Creación de Key Pair
Para la creación del Key Pair se entró al menú de EC2 en AWS y se seleccionó la opción de Key Pairs.
Allí se creó el Key Pair con esta información.  
![alt text](/lab0/img/clave.jpg "keypair")

## Creación Cluster EMR
Para la creación del cluster se debió ir al menú de EMR en AWS y seleccionar Create Cluster.  
Inmediatamente se ingresa se tuvo que seleccionar en Advanced Options para seleccionar con más detalle las configuraciones que se necesitan.   
En la primera opción se seleccionó emr-6.3.1 según el tutorial y las opciones de: 
- Hue 4.9.0
- Spark 3.1.1
- Hadoop 3.2.1
- JupyterHub 1.2.0
- Hive 3.1.2
- Zepellin 0.9.0
- Tez 0.9.2
- JupyterEnterpriseGateway 2.1.0

Quedó de esta forma:  
![alt text](/lab0/img/opcionescluster.jpg "emr1")  
Se dio en next y en el siguiente paso de Hardware se deja todo por defecto revisando que se tenga 1 Nodo Master y 2 Nodos Core.  
Luego en el tercer paso de General Cluster Settings se le agregó un nombre al cluster y se seleccionó la dirección en la que se guardaron los datasets en S3:  
![alt text](/lab0/img/seguridadcluster.jpg "emr2")  
En el último paso de Security se seleccionó el Key Pair que se creó al inicio del reto y se dio en crear cluster.  
Después de esperar unos minutos el cluster se crea y se muestra funcionando asi:  
![alt text](/lab0/img/conexion.jpg"clusterok")  

## Activación servicios del cluster
Se configuraron varios servicios del cluster para el trabajo con los datos.  
Se necesitaban abrir los puertos 8888, 9443 y 8890 para configurar estos servicios. Esto se realizó desde EMR en la pestaña de Block Public Access.  
![alt text](/lab0/img/reglas.jpg "baccesemr")  
También se debían agregar las reglas de entrada al grupo de seguridad del nodo master para que permitiera tráfico por esos puertos. Para ingresar al grupo de seguridad del nodo master se ngresó a los detalles del cluster y en la parte de abajo señalaba el link para entrar al grupo de seguridad del nodo master.  
![alt text](/lab0/img/seguridadcluster.jpg"clusterinfo")  
Y luego dentro del grupo de seguridad se modificaron las reglas.  


### Hue
Para ingresar a Hue se copió el enlace de user interfaces que decía Hue y se ingresó.  
En la página de inicio se deben ingresar unas credenciales para crear cuenta. En el usuario se ingresó hadoop y la contraseña siguiendo los parámetros que se mostraban.  
![alt text](/lab0/img/hue.jpg "loginhue")  
Luego de ingresar ya apareció toda la aplicación y se puede ingresar a Hive y a varios otros lenguajes y también se puede acceder a la información que se tenía almacenada en los buckets.  


### Zepellin
Para acceder a Zepellin se ingresó al link de Zepellin en user interfaces.  
Se ingresó y ya se pudo observar que se podían crear notas que se agregan a S3 automáticamente.  
![alt text](/lab0/img/zeppeling.jpg "zepellinok")  

### JupyterHub
Para acceder a JupyterHub también se utilizó el enlace de user interfaces.  
En la página de inicio solicitó una autenticación y se ingresó con el usuario jovyan y la clave jupyter que eran por default.  
![alt text](/lab0/img/jupyter.jpg "jupyterhublogin")  
Luego se pudo acceder a la aplicación y se tuvo acceso a los notebooks existentes y a los que se desearan crear nuevos.  

## Acceso al nodo master con SSH
En el grupo de seguridad del nodo master también se agregó una regla de entrada que permitiera el tráfico de SSH.  
Ingresando a los detalles del cluster se podía observar una opción de conectarse al nodo master mediante SSH. Se copió el comando que aparecía para la conexión y se obtuvo la conexión mediante SSH exitosa.  
![alt text](/lab0/img/conexion.jpg "sshmaster")  
