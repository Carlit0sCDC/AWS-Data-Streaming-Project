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

Lo primero que haremos será 