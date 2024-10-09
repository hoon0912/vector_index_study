
# Local Environment
| Type                | Version     |
|---------------------|-------------|
| **OS**              | Sonoma 14.3 |
| **Docker**          | 27.2.1-rd   |
| **Java (OpenJDK)**  | 17.0.12     |
| **Python**          | 3.12.7      |

# Set up

## 1. Install Rancher Desktop
https://docs.rancherdesktop.io/getting-started/installation/

## 2. Install Java
```shell
brew install openjdk@17
java --version # openjdk 17.0.12 2024-07-16

echo 'export JAVA_HOME=$(/usr/libexec/java_home -v17)' >> ~/.zshrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.zshrc
```

## 3. Install Python
```shell
brew search python
brew install python@3.12
brew install jupyter

python -m venv fastcampus_vector
source fastcampus_vector/bin/activate
pip install ipykernel
pip install nltk
pip install pyspark
pip install kafka-python
deactivate
~/fastcampus_vector/bin/python -m ipykernel install --user --name "venv-Python-3.12"
```
## 4. Install libraries
```shell
curl -O https://dlcdn.apache.org/spark/spark-3.4.3/spark-3.4.3-bin-hadoop3.tgz
tar -xzf spark-3.4.3-bin-hadoop3.tgz
sudo mv spark-3.4.3-bin-hadoop3 /opt/spark-3.4.3

curl -O https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.12/3.4.3/spark-sql-kafka-0-10_2.12-3.4.3.jar
sudo mv spark-sql-kafka-0-10_2.12-3.4.3.jar /opt/spark-3.4.3/jars

curl -O https://repo1.maven.org/maven2/org/apache/kafka/kafka-clients/3.3.2/kafka-clients-3.3.2.jar
sudo mv kafka-clients-3.3.2.jar /opt/spark-3.4.3/jars

curl -O https://repo1.maven.org/maven2/org/apache/spark/spark-streaming-kafka-0-10_2.12/3.4.3/spark-streaming-kafka-0-10_2.12-3.4.3.jar
sudo mv spark-streaming-kafka-0-10_2.12-3.4.3.jar /opt/spark-3.4.3/jars

curl -O https://repo1.maven.org/maven2/org/apache/spark/spark-token-provider-kafka-0-10_2.12/3.4.3/spark-token-provider-kafka-0-10_2.12-3.4.3.jar
sudo mv spark-token-provider-kafka-0-10_2.12-3.4.3.jar /opt/spark-3.4.3/jars

curl -O https://repo1.maven.org/maven2/org/apache/commons/commons-pool2/2.12.0/commons-pool2-2.12.0.jar
sudo mv commons-pool2-2.12.0.jar /opt/spark-3.4.3/jars

cp /opt/spark-3.4.3/conf/spark-defaults.conf.template /opt/spark-3.4.3/conf/spark-defaults.conf
sudo ln -s /opt/spark-3.4.3 /opt/spark

echo 'spark.driver.extraJavaOptions   -Djava.security.manager=allow' >> /opt/spark-3.4.3/conf/spark-defaults.conf
echo 'spark.executor.extraJavaOptions   -Djava.security.manager=allow' >> /opt/spark-3.4.3/conf/spark-defaults.conf

echo 'export SPARK_HOME=/opt/spark' >> ~/.zshrc
echo 'export PATH=$SPARK_HOME/bin:$PATH' >> ~/.zshrc
echo 'export PYSPARK_PYTHON=$(brew --prefix python)/libexec/bin/python' >> ~/.zshrc
echo "export PYSPARK_DRIVER_PYTHON='jupyter'" >> ~/.zshrc
echo "export PYSPARK_DRIVER_PYTHON_OPTS='notebook --no-browser --port=8889'" >> ~/.zshrc
echo "export PYSPARK_SUBMIT_ARGS='--jars /opt/spark-3.4.3/jars/elasticsearch-spark-30_2.12-8.15.0.jar,/opt/spark-3.4.3/jars/spark-sql-kafka-0-10_2.12-3.4.3.jar,/opt/spark-3.4.3/jars/kafka-clients-3.3.2.jar,/opt/spark-3.4.3/jars/spark-streaming-kafka-0-10_2.12-3.4.3.jar,/opt/spark-3.4.3/jars/spark-token-provider-kafka-0-10_2.12-3.4.3.jar,/opt/spark-3.4.3/jars/commons-pool2-2.12.0.jar pyspark-shell'" >> ~/.zshrc
```

```shell
curl -O https://repo1.maven.org/maven2/org/elasticsearch/elasticsearch-spark-30_2.12/8.15.0/elasticsearch-spark-30_2.12-8.15.0.jar
mv elasticsearch-spark-30_2.12-8.15.0.jar /opt/spark-3.4.3/jars/
```

# Start
```shell
docker-compose up -d
```

# Stop
```shell
docker-compose down
```