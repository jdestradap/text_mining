## Pasos para configuración ambiente AWS                              

1. Acceder a la cuenta de aws educate por medio del siguiente link:
https://www.awseducate.com/signin/SiteLogin

2. Luego en ingresamos a la opción AWS Starter Account.

3. Ingresamos a la consola de AWS.

4. Para almacenar en la nube  archivos con AWS utilizamos S3. Creamos el Bucket lo hacemos público y se sube el archivo mediante un drag and drop.

5. Para configurar el cluster y poder procesar en paralelo las noticas, se utiliza EMR (Elastic Map Reduce) el cual es un servicio ofrecido por AWS para creacion de clusters en la nube.

Se ingresa a EMR, se crea el cluster con la siguiente confguración:

- Nombre del cluster: Almacenamiento
- Loggin: Verdadero
- S3 folder: s3://aws-logs-895339990044-us-east-1/elasticmapreduce/
- Release: emr-5.24.0
- Application: Spark: Spark 2.4.0 on Hadoop 2.8.5 YARN with Ganglia 3.7.2 and Zeppelin 0.8.1
- Instance type: m3.xlarge
- Número de instancias: 3
- EC2 Key Pair: almacenamiento-dos

6. Para crear los notebooks y trabajar con Spark, ingresamos a la opción Notebooks, luego "Create NoteBook", en ingresamos la siguiente información:

- Notebook name
- Cluster: Seleccionamos el cluster previamente creado.
- Notebook location: Seleccionamos el lugar donde quedara almacenado el notebook.

7. Instalacion de librerias adicionales

Para instalar librerias adicionales, como nltk para el preprocesamiento, ingresamos a cada maquina por ssh utilizando el EC2 Key Pair configurado en la creación del Cluster.


Para ingresar al master:

ssh -i almacenamiento-dos.pem hadoop@ec2-18-205-245-164.compute-1.amazonaws.com

sudo su -
pip install pandas
pip install matplotlib
pip install nltk

python -m nltk.downloader stopwords


Para ingresar al slave 1:

ssh -i almacenamiento-dos.pem hadoop@ec2-18-207-150-157.compute-1.amazonaws.com

Para ingresar al slave 2:

ssh -i almacenamiento-dos.pem hadoop@ec2-54-235-5-64.compute-1.amazonaws.com


Nota: Si no es posible ingresar por ssh a las maquinas es necesario configurar que puerto y desde que ip se puede acceder a estas maquinas. Esto es posible hacerlo, ingresando a cada maquina y configurando
por la opción: Servicios -> EC2 -> Network & Security -> Security Groups -> Inbound: Aca nos paramos en cada maquina en editamos la tabla para agregar la siguiente configuración

Type: All TCP
Protocol: TCP
Port Range: 0 - 65535 
Source: Custom
Ip: 0.0.0.0/0

8. Instalacion de Jupyter HUB
