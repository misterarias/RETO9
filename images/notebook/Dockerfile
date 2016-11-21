FROM jupyter/all-spark-notebook

# For both Pythons
USER root
COPY requirements.txt .
RUN pip install -r requirements.txt
RUN pip2 install -r requirements.txt

# Enable a world of extensions, very few are useful
#RUN conda install -c conda-forge jupyter_contrib_nbextensions
#RUN jupyter-nbextension install jupyter_contrib_nbextensions --py --sys-prefix && jupyter-nbextension enable  jupyter_contrib_nbextensions --py --sys-prefix

RUN  conda install --quiet --yes 'r-curl' 'r-xml' 'r-igraph' 'r-bitops' && conda clean -tipsy

# Add spark CSV support
RUN echo "spark.jars.packages     com.databricks:spark-csv_2.10:1.3.0" >> /usr/local/spark/conf/spark-defaults.conf
#ENV PYSPARK_SUBMIT_ARGS "--packages com.databricks:spark-csv_2.10:1.4.0 pyspark-shell"
#ENV SPARKR_SUBMIT_ARGS "--packages com.databricks:spark-csv_2.10:1.4.0 sparkr-shell"
#ENV SPARK_SUBMIT_ARGS "--packages com.databricks:spark-csv_2.10:1.4.0 spark-shell"

USER $NB_USER
