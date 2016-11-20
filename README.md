# RETO9 - Uso de GraphX

En esta propuesta de actividad vamos a manejar datos usando procesamiento de grafos. En primer lugar crearemos un set de datos usando R y despues lo procesaremos usando Spark GraphX.

## Preparacion del entorno de trabajo

Aunque se puede realizar este ejercicio con otros entornos, para preparar las instrucciones se ha usado una maquina virtual con Ubuntu 14.04, asi que las instrucciones deben funcionar sin cambios en ese entorno.

### Instalacion de R

Ejecutar:

<code>
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list'

gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9

gpg -a --export E084DAB9 | sudo apt-key add -

sudo apt-get update

sudo apt-get -y install r-base

</code>

### Instalacion de Spark

## Creacion de datos

## Procesamiento

"spark-shell --packages com.databricks:spark-csv_2.11:1.2.0"

http://stat-computing.org/dataexpo/2009/the-data.html

http://ampcamp.berkeley.edu/big-data-mini-course/graph-analytics-with-graphx.html

http://semanticweb.cs.vu.nl/R/wikipedia_graph/wikipedia_graph.html

http://www.sparktutorials.net/analyzing-flight-data:-a-gentle-introduction-to-graphx-in-spark
