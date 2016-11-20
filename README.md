
![Graph](https://raw.githubusercontent.com/germanblanco/RETO9/master/graph.png "Graph")

# RETO9 - Uso de GraphX

En esta propuesta de actividad vamos a manejar datos usando procesamiento de grafos. En primer lugar crearemos un set de datos usando R y despues lo procesaremos usando Spark GraphX.

## Preparacion del entorno de trabajo

Aunque se puede realizar este ejercicio con otros entornos, para preparar las instrucciones se ha usado una maquina virtual con Ubuntu 14.04, asi que las instrucciones deben funcionar sin cambios en ese entorno.

### Instalacion de R

<pre>
sudo apt-get install libxml2-dev libx11-dev libglu1-mesa-dev freeglut3-dev mesa-common-dev libcurl4-gnutls-dev
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list'
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install r-base
</pre>

### Instalacion de Java

<pre>
sudo apt-add-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
</pre>

### Instalacion de Spark

Bajarse spark-1.6.3-bin-hadoop2.6.tgz de http://spark.apache.org/downloads.html

<pre>
sudo mkdir /usr/local/src/scala
sudo tar xvf spark-1.6.3-bin-hadoop2.6.tgz -C /usr/local/src/scala/
echo "export SCALA_HOME=/usr/local/src/scala/spark-1.6.3-bin-hadoop2.6" >> .bashrc
echo 'export PATH=$SCALA_HOME/bin:$PATH' >> .bashrc
. .bashrc
</pre>

## Creacion de datos

<pre>
wget http://semanticweb.cs.vu.nl/R/wikipedia_graph/crawler.R
R
</pre>

Los siguientes comandos son dentro del shell de R:

<pre>
install.packages(c('RCurl','XML','igraph','bitops'),dependencies=TRUE)
source("crawler.R")
blue_box_2 <- crawl("https://simple.wikipedia.org/wiki/Blue_box", 2)
save.graph(blue_box_2, "blue_box_2")
</pre>

## Procesamiento

<pre>
spark-shell --packages com.databricks:spark-csv_2.11:1.2.0
</pre>

Los siguientes comandos son dentro del shell de Spark:

<pre>
import org.apache.spark.graphx._
import org.apache.spark.rdd.RDD
import scala.util.MurmurHash

val blue_box_2 = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").load("blue_box_2.csv")
val linksToFrom = blue_box_2.select($"Source", $"Target")
val pageNames = blue_box_2.select($"Source", $"Target").flatMap(x => Iterable(x(0).toString, x(1).toString))
val pageVertices: RDD[(VertexId, String)] = pageNames.distinct().map(x => (MurmurHash.stringHash(x), x))
val linkEdges = linksToFrom.map(x => ((MurmurHash.stringHash(x(0).toString),MurmurHash.stringHash(x(1).toString)), 1)).reduceByKey(_+_).map(x => Edge(x._1._1, x._1._2,x._2))
val graph = Graph(pageVertices, linkEdges)
graph.numVertices // 475
graph.numEdges // 499
</pre>

## Pregunta

Con estas herramientas, se pide resolver la pregunta de "Cual es la pagina de Wikipedia mas importante a menos de dos grados de distancia de Blue Box?". Para entender la pregunta y saber como resolverla, se pueden seguir los siguientes enlaces:

http://ampcamp.berkeley.edu/big-data-mini-course/graph-analytics-with-graphx.html

http://semanticweb.cs.vu.nl/R/wikipedia_graph/wikipedia_graph.html

http://www.sparktutorials.net/analyzing-flight-data:-a-gentle-introduction-to-graphx-in-spark
