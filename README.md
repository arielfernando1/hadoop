## Ejemplos en Hadoop

[Rockyou](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)  es un fichero de texto que contiene 14.344.391 contraseñas. Este fichero se encuentra en el directorio `/data` del sistema de ficheros distribuido HDFS.

El archivo **hadop-mapreduce-examples.jar** contiene los ejemplos de MapReduce que se van a ejecutar en el cluster. Este fichero se encuentra en el directorio `$HADOOP_HOME/share/hadoop/mapreduce`.


### Ejemplo 1: WordMean

Este ejemplo calcula la longitud media de las palabras de un fichero de texto. Para ello, se utiliza el siguiente comando:

```bash
hadoop jar hadoop-mapreduce-examples.jar wordmean /data/rockyou.txt /wordmean
```

### Ejemplo 2: WordCount

Este ejemplo cuenta el número de palabras de un fichero de texto. Para ello, se utiliza el siguiente comando:

```bash
hadoop jar hadoop-mapreduce-examples.jar wordcount /data/rockyou.txt /wordcount
```

### Ejemplo 3: Grep

Este ejemplo busca una palabra en un fichero de texto. Para ello, se utiliza el siguiente comando:

```bash
hadoop jar hadoop-mapreduce-examples.jar grep /data/rockyou.txt /grep "123456"
```
> Tambien podemos utilizar regex para buscar palabras que empiecen por pass, por ejemplo:
```bash
hadoop jar hadoop-mapreduce-examples.jar grep /data/rockyou.txt /grep "pass.*"
```

### Ejemplo 4: RandomTextWriter

Escribe 10GB de datos de texto aleatorio por cada nodo del cluster. Para ello, se utiliza el siguiente comando:

```bash
hadoop jar hadoop-mapreduce-examples.jar randomtextwriter /randomtextwriter
```
