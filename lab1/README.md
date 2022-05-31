# Documentación Lab 1 HDFS

La creación del Key Pair y del Bucket S3 ya se realizó en el laboratorio 0.
## Creación Cluster EMR
Para la creación del cluster fui al menú de EMR en AWS y seleccionar Create Cluster.  
Se le dio a opciones vanzadas para hacer las configuraciones necesarias.   
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
![alt text](/lab1/img/opcionescluster.jpg "emr1")  
Se dio en next y en el siguiente paso de Hardware se deja todo por defecto revisando que se tenga 1 Nodo Master y 2 Nodos Core.  
Luego en el tercer paso de configuración general del cluster se le agregó un nombre al cluster y se seleccionó la dirección en la que se guardaron los datasets en S3:  
![alt text](/lab1/img/seguridadcluster.jpg "emr2")  
En el último paso de Seguridad se seleccionó el Key Pair que se creó al inicio del reto y se dio en crear cluster.  
Se muestra el funcionamiento asi, con el paso de los minutos.
![alt text](/lab1/img/conexion.jpg"clusterok")    


## Activación servicios del cluster
Se configuraron varios servicios del cluster para el trabajo con los datos.  
Se abrieron los puertos 8888, 9443 y 8890 para configurar estos servicios. Esto se realizó desde EMR en la pestaña de Block Public Access.  
![alt text](/lab1/img/reglas.jpg "baccesemr")  
Era necesario agregar las reglas de entrada al grupo de seguridad del nodo master, para tener el funcionamiento correcto del trafico. Por lo que se accedio a estas y se configuraron estos peurtos necesarios.
![alt text](/lab1/img/seguridadcluster.jpg "clusterinfo")  
Y luego dentro del grupo de seguridad se modificaron las reglas.   


### Hue
Para ingresar a Hue se copió el enlace de user interfaces que decía Hue y se ingresó.  
En la página de inicio se deben ingresar unas credenciales para crear cuenta. En el usuario se ingresó hadoop y la contraseña siguiendo los parámetros que se mostraban.  
![alt text](/lab1/img/hue.jpg "loginhue")  
Luego de ingresar ya apareció toda la aplicación y se puede ingresar a Hive y a varios otros lenguajes y también se puede acceder a la información que se tenía almacenada en los buckets.  


## Acceso al nodo master con SSH
En el grupo de seguridad del nodo master también se agregó una regla de entrada que permitiera el tráfico de SSH.  
Ingresando a los detalles del cluster se podía observar una opción de conectarse al nodo master mediante SSH. Se copió el comando que aparecía para la conexión y se obtuvo la conexión mediante SSH exitosa.  
![alt text](/lab1/img/conexion.jpg "sshmaster")  

## Gestión de archivos hacia HDFS via Hue
Se ingresó a Hue como se explicó en la configuración de Hue. Ya en Hue se buscó la opción de Files en el menú de la izquierda ya que ese es el HDFS.  
Allí se encontró la carpeta de /user. Para agregar más archivos se creó un nuevo directorio en el que se almacenaron nuevos datasets.  
![alt text](/lab1/img/hueuser.jpg "hdfshue1")  
Dentro del directorio se creó una carpeta datasets.  
![alt text](/lab1/img/hueuser.jpg "hdfshue2")  
Dentro del directorio datasets se crearon carpetas para cada uno de los datasets que existían.  
![alt text](/img/datasetshue.jpg "hdfshue6")  
Ya en cada una de las carpetas se seleccionó Upload files y se subieron los archivos locales a cada uno de los directorios para los datasets.  
![alt text](/lab1/img/datasetscreados.jpg "hdfshue4")  
Y toda la estructura de directorios quedó así.  
![alt text](/lab1/img/carpetas.jpg "hdfshue3")  
Si se entraba a alguno de los archivos de los datasets se veía así.  
![alt text](/lab1/img/pordentrodatasets.jpg "hdfshue7")  

## Gestión de archivos hacia HDFS via SSH
Para la gestión de los archivos hacia HDFS mediante SSH primero se agregaron los datasets a HDFS. Para esto se ingresó a los detalles del cluster que se había creado y se va hacia la pestaña de Steps y se dio en Add Step.  
![alt text](/lab1/img/steps.jpg "addstep")  
Luego en la configuración se agregaron estos parámetros. En los argumentos se escogió para el origen se selecciona el bucket que se había creado en el Lab anterior y el destino una carpeta /datasets.  
![alt text](/lab1/img/pasotesagregados.jpg "stepcnf")  
Luego ya se ingresó al nodo master mediante SSH y se revisó lo que se copió con el comando:  
```
hdfs dfs -ls /datasets
```
![alt text](/lab1/img/hdfs.jpg "hdfsdataset")  
También se corrieron los siguientes comandos para revisar bien la estructura de directorios que había en HDFS:
```
hdfs dfs -ls /
hdfs dfs -ls /user
```
![alt text](/lab1/img/hdfsuser.jpg "hdfslist")  
Luego en la carpeta del usuario se crearon los directorios donde se guardó la información de los datasets.
```
hdfs dfs -mkdir /user/hadoop/datasets/all-news
hdfs dfs -mkdir /user/hadoop/datasets/gutenberg
hdfs dfs -mkdir /user/hadoop/datasets/gutenberg-small
hdfs dfs -mkdir /user/hadoop/datasets/airlines
hdfs dfs -mkdir /user/hadoop/datasets/onu
hdfs dfs -mkdir /user/hadoop/datasets/otros
hdfs dfs -mkdir /user/hadoop/datasets/retail_logs
hdfs dfs -mkdir /user/hadoop/datasets/spark
```
Después de tener las carpetas creadas se copia la información de los datasets que se pasaron a la carpeta raíz.
```
hadoop fs -cp /datasets/airlines/* /user/hadoop/datasets/airlines/
hadoop fs -cp /datasets/all-news/* /user/hadoop/datasets/all-news/
hadoop fs -cp /datasets/gutenberg/* /user/hadoop/datasets/gutenberg/
hadoop fs -cp /datasets/gutenberg-small/* /user/hadoop/datasets/gutenberg-small/
hadoop fs -cp /datasets/onu/* /user/hadoop/datasets/onu/
hadoop fs -cp /datasets/otros/* /user/hadoop/datasets/otros/
hadoop fs -cp /datasets/retail_logs/* /user/hadoop/datasets/retail_logs/
hadoop fs -cp /datasets/spark/* /user/hadoop/datasets/spark/
```
Para el ejemplo se muestra la información que se copió de la carpeta de Gutenberg-Small
![alt text](/lab1/img/copia.jpg "gutenbergsmall")  

## Gestión de archivos hacia AWS S3 via SSH
Para la gestión de archivos en S3 por SSH primero se conectó al nodo master mediante SSH y ya allí se empezaron a realizar los comandos de manejo de S3. El primero fue para crear un bucket.  
```
aws s3 mb s3://salbissercbucketazo-ssh
```  
![alt text](/lab1/img/bucketazo.jpg "s3ssh1")  
Luego se corrió otro comando que creaba los directorios dentro del bucket, aca lo utilice para todos
```
aws s3api put-object --bucket salbissercbucketazo-ssh --key airlines/
```  
![alt text](/lab1/img/archivobucket.jpg "s3ssh2")  
Los directorios en AWS quedaron así.  
![alt text](/lab1/img/funciona.jpg "s3ssh3")  
Luego para la subida de archivos a cada uno de los directorios se realizaba el siguiente comando.  
```
aws s3 cp D:\Universidad\9 semestre\Labtelematica\funciona\telematicafunciona\datasets\airlines.csv s3://salbissercbucketazo-ssh/airlines/
```  
Y los archivos se subieron también a AWS S3 satisfactoriamente.  
![alt text](/lab1/img/funcionando.jpg "s3ssh4")  

## Gestión de archivos hacia AWS S3 via Hue
Desde Hue también se pudo crear un bucket. Se ingresó a la opción de S3 en el menú y aparte de observar los buckets existentes también se tenía la opción de crear más.  
Luego de crear el bucket exitosamente dentro de él se crearon los directorios donde iban a estar los datasets y quedó de esta forma.  
Luego de tener todos los directorios ya dentro de cada uno se pudieron subir los archivos que le correspondían a cada carpeta.
![alt text](/lab1/img/airlines.jpg "s3hue1")   
En AWS S3 también se comprobó que se hubiese creado todo bien y así se mostraba.  
![alt text](/lab1/img/carpetashue.jpg "s3hue1")  





## Acceso público a Bucket S3

Para garantizar acceso público a la información del bucket se garantizó acceso mediante el botón de make public.  
En los permisos del bucket se agregan la opción de leer la información desde cualquier acceso público. 
![alt text](/lab1/img/permisos.jpg "makepublic")  

Aca podemos ver el exito
![alt text](/lab1/img/exito.jpg "makepublic")  

 
Ya se puede acceder a la información del bucket en este link:  
https://salbissercbucketazo-ssh.s3.us-east-1.amazonaws.com/