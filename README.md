# Proyecto de transmision de datos streaming a AWS

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/Roadmap.png" alt="Logo de Insight Analysts Collective" >
</p>

## Indice

1. [Descripcion general](#descripcion-general)
2. [Primeros pasos](#primeros-pasos)
3. [Etapa final](#Etapa-final)
4. [Conclusión](#Conclusión)

# Descripcion general

Este proyecto se encarga de recolectar datos streaming simulados desde python como si vinieran desde una API o base de datos en tiempo real y mediante python y la utilizacion de librerias adecuadas se llevan dichos datos a un Producer de Kafka, luego a un Consumer y ese Consumer va a estar conectado a Amazon S3 en una base de datos, luego mediante AWS Glue Crawler se los pasa por un Catalog para finalmente terminar en Amazon Athena y poder realizar las consultas que se requieran.

El proyecto parte en base a los siguientes componentes:

* Fuente de datos: Para la simulacion de datos streaming en el notebook de KafkaProducer creamos un iterador que se encarga de recoger los datos de nuestro csv 'indexProcessed.csv' e iterar cada fila parte a parte convirtiendo cada fila en un diccionario de forma desordenada.
* Python: Para nuestro proceso de recoleccion y envio de datos vamos a recurrir a python enviando esos datos a AWS y conectandonos mediante la libreria s3fs y nuestras terminales emergentes CMD de nuestro sistema operativo Windows (en este caso).
* Terminales CMD: Para la transmision de nuestros datos vamos a estar utilizando 5 ventanas emergentes (terminales) CMD para lograr esta transmision de datos streaming hacia AWS.
* Amazon EC2: Esta instancia de Amazon nos ayudará a conectar de manera remota las terminales desde Amazon.
* Kafka: Va a cumplir un rol clave para la transmision de datos ya que mediante la creacion de Kafka server en una terminal procederemos a crear el producer y el consumer de data streaming en otras dos terminales apartes.
* Amazon S3: Va a ser nuestro lugar de almacenamiento de nuestros datos streaming mediante la utilizacion de una base de datos.
* AWS Glue Crawler: Se va a encargar de descubrir, clasificar nuestros datos proveniente de nuestra base de datos para automatizar el proceso de gestion de metadatos para que estos puedan ser utilizados en procesos de ETL y analisis posteriores.
* AWS Glue Data Catalog: Nos permite gestionar y almacenar los metadatos y la informacion sobre los datos de manera eficiente, en este caso clasificados por Glue Crawler.
* Amazon Athena: Finalmente despues de este largo proceso a partir de los datos provenientes de Amazon S3, Athena nos va a servir para ejecutar las consultas interactivas pertinentes mediante SQL estandar sobre grandes conjuntos de datos.

## Primeros pasos

Lo primero que haremos será lanzar una instancia de EC2 en nuestra cuenta de AWS.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/1.png" alt="Logo de Insight Analysts Collective" >
</p>

Luego de eso crearemos un par de claves.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/2.png" alt="Logo de Insight Analysts Collective" >
</p>

A partir de lo anterior, conectaremos nuestra instancia de EC2 y en la seccion de Cliente SSH copiaremos el comando de conexion de nuestra instancia en el portapapeles o en un bloc de notas.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/3.png" alt="Logo de Insight Analysts Collective" >
</p>

A continuacion, abriremos nuestra primer terminal, en la cual mediante el comando 'cd' nos navegaremos hasta la carpeta raiz de nuestro proyecto donde guardaremos todos nuestros archivos necesarios para el mismo, una vez dentro pegaremos el comando copiado desde EC2 y esperaremos a que nos conecte al mismo, luego de eso instalaremos todos los archivos/paquetes necesarios para nuestra conexion de zookeeper, los cuales estan dentros del archivo 'commands.txt' en el repositorio.

Despues de seguir todas las intrucciones de comandos para nuestra conexion de zookeeper se nos quedará tomada la terminal, lo cual significa que ya esta lista dicha configuración.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/4.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/5.png" alt="Logo de Insight Analysts Collective" >
</p>

Abriremos una nueva terminal y realizaremos el mismo proceso de conexion a EC2 con nuestro broker de Kafka y seguiremos los comandos tal cual nos figura en nuestro archivo de comandos.

Luego veremos como sucede lo mismo que con zookeeper.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/6.png" alt="Logo de Insight Analysts Collective" >
</p>

Luego copiaremos desde nuestra instancia de EC2 nuestra direccion IPv4 Publica y la pegaremos en nuestros tres comandos de kafka de topics, produces y consumer respectivamente.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/7.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/8.png" alt="Logo de Insight Analysts Collective" >
</p>

Abriremos una nueva terminal, nos conectaremos a EC2 siguiendo los procesos anteriores, nos dirigiremos con 'cd' a nuestro directorio de kafka y pegaremos la primer linea de topics que editamos en nuestro bloc de notas, lo cual hará que se conecte nuestro producer, donde se enviarán los datos.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/9.png" alt="Logo de Insight Analysts Collective" >
</p>

En la siguiente imagen podemos evidenciar la estructura necesaria para el envio de data streaming desde python.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/10.png" alt="Logo de Insight Analysts Collective" >
</p>

Una vez creada y configurada nuestra cuarta terminal con el ultimo comando del bloc de notas obtendremos nuestro consumer, el cual se encargará de recibir la data streaming, tambien podemos visualizar la estructura de consumer de nuestro notebook KafkaConsumer y como segun lo que vayamos enviando desde KafkaProducer nos lo emitirá en nuestro Consumer.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/11.png" alt="Logo de Insight Analysts Collective" >
</p>

A continuación vemos nuestras 4 terminales necesarias para el proceso de data streaming, la configuracion de zookeeper, nuestro broker de kafka, nuestro producer y nuestro consumer.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/12.png" alt="Logo de Insight Analysts Collective" >
</p>

Lo siguiente que haremos en python es crear un iterador en el cual por cada fila de nuestro archivo csv nos creará un diccionario en el cual se almacenarán los datos para cada filay y luego esos diccionarios se enviarán desde el producer de manera streaming hacia nuestro consumer hasta que lo detengamos manualmente.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/13.png" alt="Logo de Insight Analysts Collective" >
</p>

En nuestra pestaña de KafkaConsumer podemos evidenciar como van flutuando los datos streaming. Lo mismo sucede con nuestro almacenamiento en S3 una vez lo hayamos creado, configurado y conectado con AWS CLI.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/14.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/15.png" alt="Logo de Insight Analysts Collective" >
</p>

A continuación creamos nuestro crawler necesario para el proyecto.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/16.png" alt="Logo de Insight Analysts Collective" >
</p>

En las siguientes dos imagenes podemos ver como ejecutando nuestro receptor de KafkaConsumer y nuestro generador de data streaming de KafkaProducer se empezarán a enviar los datos pasando por todo el proceso anteriormente detallado hasta llegar a S3 y Amazon Athena.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/17.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/18.png" alt="Logo de Insight Analysts Collective" >
</p>

Luego en Athena podemos realizar las consultas convenientes o requeridas que necesitemos en un lenguaje de SQL estándar, en este caso al hacer un COUNT(*) y actualizando la consulta constantemente vemos como progresivamente van aumentando la cantidad de filas debido a la constante carga de data streaming del proceso realizada con exito.

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/19.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/20.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/21.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/22.png" alt="Logo de Insight Analysts Collective" >
</p>

<p align="center">
  <img src="https://github.com/Carlit0sCDC/AWS-Data-Streaming-Project/blob/main/source/23.png" alt="Logo de Insight Analysts Collective" >
</p>

## Conclusion

A partir de este proyecto podemos evidenciar un pipeline de datos streaming desde nuestro equipo local hacia AWS con un desarrollo complejo pero interactivo a la vez, espero les haya parecido entretenida y llevadera la explicacion del mismo.
