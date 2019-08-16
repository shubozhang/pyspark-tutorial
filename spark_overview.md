# Basic concepts of Apache Spark
## Outline
### Module 01: Introduction to Spark
* Explain the purpose of Spark
* List and describe the components of the Spark unified stack
* Understand the basics of Resilient Distributed Dataset (RDD)
* Downloading and installing Spark standalone
* Scala and Python overview
* Launch and use Spark’s Scala and Python shell

### Module 02:  Resilient Distributed Dataset and DataFrames
* Describe Spark’s primary data abstraction
* Understand how to create parallelized collections and external datasets
* Work with Resilient Distributed Dataset (RDD) operations and DataFrames
* Utilize shared variables and key-value pairs

### Module 03: Spark application programming
* Understand the purpose and usage of the SparkContext
* Initialize Spark with the various programming languages
* Describe and run some Spark examples
* Pass functions to Spark
* Create and run a Spark standalone application
* Submit applications to the cluster
* This lesson requires that you have your own lab environment to run the exercise.

### Module 04: Introduction to Spark libraries
* Understand and use the various Spark libraries

### Module 05: Spark configuration, monitoring and tuning
* Understand components of the Spark cluster
* Configure Spark to modify the Spark properties, environmental variables, or logging properties
* Monitor Spark using the web UIs, metrics, and external instrumentation
* Understand performance tuning considerations

### Module06: Introduction to Notebooks
* Understand how to use Zeppelin in your Spark projects
* Identify the various notebooks you can use with Spark

### Module07: RDD Architecture
* Understand how Spark generates RDDs
* Manage partitions to improve RDD performance

### Module08: Optimizing Transformations and Actions
* Use advanced RDD operations
* Identify what operations cause shuffling

### Module09: Caching and Serialization
* Understand how and when to cache RDDs
* Understand storage levels and their uses

### Module10: Develop and Testing
* Understand how to use sbt to build Spark projects
* Understand how to use Eclipse and IntelliJ for Spark development


### Module 01: Introduction to Spark
1.1 Why not MapReduce?
```
1). MapReduce takes more time. 
2). MapReduce needs more knowledge to implement. 
3). MapReduce jobs only work for a specific set of use cases. It does not fit too may jobs.
```
1.2 Why Spark
```
Apache Spark was designed as a computing platform to be fast, general-purpose, and easy to use. 
It extends the MapReduce model and takes it to a whole other level.

1). Fast: The speed comes from the in-memory computations. Applications running in memory allows for
    a much faster processing and response. Spark is even faster than MapReduce for complex applications on disks.

2). Easy to use: 
   a. Simple APIs for Scala, Python and Java. 
   b. Additional libraries which you can use for SQL, machine learning, streaming, and graph processing. 
   c. Spark runs on Hadoop clusters such as HadoopYARN or Apache Mesos, or even as a standalone with its own scheduler.

3). General-purpose: This generality covers a wide range of workloads under one system. 
    a. MapReduce types jobs or iterative algorithms 
    b. Interactive queries
    c. Process streaming data 

4). Other features:
    a. parallel distributed processing
    b. fault tolerance on commodity hardware,
    c. scalability
    d. the concept with aggressively cached in-memory distributed computing, low latency, high level APIs 
       and stack of high level tools. This saves time and money.
```

1.3 Who should use Spark?
```
1). Data scientist analyze and model the data to obtain insight. 
    a. ETL: extracting, transforming, and loading data into something for data analysis
    b. Use Spark for its ad-hoc analysis to run interactive queries that will give them results immediately. 
    c. Using SQL, statistics, machine learning and some programming, usually in Python, MatLab or R. 
    d. Based on obtained insights, there may need develop a production data processing application, a web application, 
       or some system to act upon the insight. Engineers will implement the application.


2) Engineers would use Spark's programming API to develop a system that implement business use cases. 
   a. Spark parallelize these applications across the clusters while hiding the complexities
      of distributed systems programming and fault tolerance. 
   b. Engineers can use Spark to monitor,inspect and tune applications.
```

1.4 Spark unified stack
```
1. Spark core:  is a general-purpose system providing scheduling, distributing, and monitoring of the applications
   across a cluster. 
1). It is designed to scale up from one to thousands of nodes. It can run over a variety of cluster managers:
    a. Hadoop YARN 
    b. Apache Mesos. 
    c. standalone
2). Then you have the components on top of the core that are designed to interoperate
    closely, letting the users combine them, just like they would any libraries in a software
    project. The benefit of such a stack is that all the higher layer components will inherit
    the improvements made at the lower layers. 
    Example: Optimization to the Spark Core will speed up the SQL, the streaming, the machine learning 
    and the graph processing libraries as well.

2. Spark SQL:  is designed to work with the Spark via SQL and HiveQL (a Hive variant of SQL).
   Spark SQL allows developers to intermix SQL with Spark's programming language supported
   by Python, Scala, and Java.

3. Spark Streaming:  provides processing of live streams of data. The Spark Streaming API closely
   matches that of the Sparks Core's API, making it easy for developers to move between applications
   that processes data stored in memory vs arriving in real-time. It also provides the same degree
   of fault tolerance, throughput, and scalability that the Spark Core provides.

4. Machine learning:  MLlib is the machine learning library that provides multiple types of machine
   learning algorithms. All of these algorithms are designed to scale out across the cluster as well.

5. GraphX:  is a graph processing library with APIs to manipulate graphs and performing graph-parallel computations.
```

1.5 RDD: Resilient Distributed Dataset
```
1). Spark's primary core abstraction is called Resilient Distributed Dataset or RDD. 

2). Essentially it is just a distributed collection of elements that is parallelized across the cluster. 

3). Two types of RDD operations. Transformations and Actions. 
    a. Transformations are those that do not return a value. 
       In fact, nothing is evaluated during the definition of these transformation statements. 
       Spark just creates these Direct Acyclic Graphs or DAG, which will only be evaluated at runtime. 
       We call this lazy evaluation.
       The fault tolerance aspect of RDDs allows Spark to reconstruct the transformations used
       to build the lineage to get back the lost data.
    
    b. Actions are when the transformations get evaluted along with the action that is called for that
       RDD. Actions return values. For example, you can do a count on a RDD, to get the number
       of elements within and that value is returned.
    
       The DAG is just updated each time until an action is called. This provides fault tolerance. 
       For example, let's say a node goes offline. All it needs to do when it comes back online is to re-evaluate
       the graph to where it left off.

4) Caching is provided with Spark to enable the processing to happen in memory. If it does
   not fit in memory, it will spill to disk.
```

### Module 02 Resilient Distributed Dataset, DataFrames and Dataset
2.1 Resilient Distributed Dataset(RDD) features
```
1) fault tolerant
2) collection of elements that can be parallelized. 
3) Immutable. These are the fundamental primary units of data in Spark.
```

2.2 Two operations
```
1) When RDDs are created, a direct acyclic graph (DAG) is created. This type of operation is
   called transformations. Transformations makes updates to that graph, but nothing actually
   happens until some action is called. 

2) Transformations return a pointer to the RDD created. RDD1 -> RDD2

3) Actions return values that comes from the action. RDD -> value
```
2.3 There are three methods for creating a RDD.
```
1) You can parallelize an existing collection.
   This means that the data already resides within Spark and can now be operated on in parallel.
   As an example, if you have an array of data, you can create a RDD out of it by calling
   the parallelized method. This method returns a pointer to the RDD. So this new distributed
   dataset can now be operated upon in parallel throughout the cluster.

2) The second method to create a RDD, is to reference a dataset. 
   This dataset can come from an external storage source supported by Hadoop such as HDFS, Cassandra, HBase, Amazon S3, etc.

3) The third method to create a RDD is from transforming an existing RDD to create a new RDD. 
   In other words, let's say you have the array of data that you parallelized earlier. Now you want
   to filter out strings that are shorter than 20 characters. A new RDD is created using
   the filter method. Spark supports text files, SequenceFiles and any other Hadoop InputFormat.
   
Note: a. The parallelize method returns a pointer to the RDD. Remember, transformations operations
         such as parallelize, only returns a pointer to the RDD. It actually won't create that
         RDD until some action is invoked on it. 
      b. The transformation basically updates the direct acyclic graph (DAG). With this new RDD, 
         you can perform additional transformations or actions on it such as the filter transformation.   
      c. Example: Loading the file creates a RDD, which is only a pointer to the file. 
         The dataset is not loaded into memory yet. Nothing will happen until some action is called. 
      d. When the action is called, Spark goes through the DAG and applies all the transformation up until
         that point, followed by the action and then a value is returned back to the caller.
``` 

2.4 DAG: direct acyclic graph 
```
1) A DAG is essentially a graph of the business logic and does not get executed until an action is called 
    -- often called lazy evaluation.
    
2) To view the DAG of a RDD after a series of transformation, use the toDebugString method. It will display the series of transformation that Spark will go through once an action is called. 

3). Sample DAG starts as a textFile and goes through a series of transformation such as map and filter, 
    followed by more map operations. Remember, that it is this behavior that allows for fault tolerance. 
    If a node goes offline and comes back on, all it has to do is just grab a copy of this from a 
    neighboring node and rebuild the graph back to where it was before it went offline.
```
2.5 Execute an action
```
1) load in the text file is the data is partitioned into different blocks across the cluster.
2) The driver sends the code to be executed on each block. 
3) The executor on each workers that is going to be performing the work on each block.
4) Then the executors read the HDFS blocks to prepare the data for the operations in parallel.
5) After a series of transformations, you want to cache the results up until that point into memory. A cache is created.
6) After the first action completes, the results are sent back to the driver. 
7) To process the second action, Spark will use the data on the cache -- it doesn't need to
   go to the HDFS data again. It just reads it from the cache and processes the data from there.
8) Finally the results go back to the driver and we have completed a full cycle.

NOTE: Remember that Transformations are essentially lazy evaluations. Nothing is executed until
      an action is called. Each transformation function basically updates the graph and when an action
      is called, the graph is executed. Transformation returns a pointer to the new RDD.
```

2.6 Transformation functions:
```
1). flatMap function:  is similar to map, but each input can be mapped to 0 or more output
    items. What this means is that the returned pointer of the func method, should return
    a sequence of objects, rather than a single item. It would mean that the flatMap would
    flatten a list of lists for the operations that follows. Basically this would be used
    for MapReduce operations where you might have a text file and each time a line is read in,
    you split that line up by spaces to get individual keywords. Each of those lines ultimately is
    flatten so that you can perform the map operation on it to map each keyword to the value of one.
    
2). join function:  combines two sets of key value pairs and return a set of keys to a
    pair of values from the two initial set. For example, you have a K,V pair and a K,W pair.
    When you join them together, you will get a K, (V,W) set.
    The reduceByKey function aggregates on each key by using the given reduce function. This
    is something you would use in a WordCount to sum up the values for each word to count
    its occurrences.
```
2.7 Action Functions:
```
Action returns values. 

1). collect function:  returns all the elements of the dataset as an array of the driver program.
    It is used for operation that returns a significantly small subset of data which can fit into memory,
    it will run out of memory.

2). count function: returns the number of elements in a dataset and can also be used to check
   and test transformations.

3). take(n) function:  returns an array with the first n elements. Note that this is currently
    not executed in parallel. The driver computes all the elements.

4). foreach(func) function:  run a function func on each element of the dataset.
```
2.8 RDD persistence. 
```
1). The cache function is actually the default of the persist function with the MEMORY_ONLY storage. 
    One of the key capability of Spark is its speed through persisting or caching. 

2). Each node stores any partitions of the cache and computes it in memory. When a subsequent action
    is called on the same dataset, or a derived dataset, it uses it from memory instead of
    having to retrieve it again. 

3). Future actions in such cases are often 10 times faster. The first time a RDD is persisted, 
    it is kept in memory on the node. 

4). Caching is fault tolerant because if it any of the partition is lost, it will automatically 
    be recomputed using the transformations that originally created it.
```
2.9 Two methods to invoke RDD persistence: persist() and cache(). 
```
1). The persist() method allows you to specify a different storage level of caching. 
For example, you can choose to persist the data set on disk, persist it in memory but as serialized objects to save
space, etc. 

2). Again the cache() method is just the default way of using persistence by storing deserialized objects in memory.

3). Basically, you can choose to store in memory or memory and disk. 
    If a partition does not fit in the specified cache location, then it will be recomputed on the fly. 

4). You can also decide to serialized the objects before storing this. 
    This is space efficient, but will require the RDD to deserialized before it can be read, 
    so it takes up more CPU workload. 

5). There's also the option to replicate each partition on two cluster nodes. 

6). Finally, there is an experimental storage level storing the serialized object in Tachyon. 
    This level reduces garbage collection overhead and allows the executors to be smaller 
    and to share a pool of memory.
```

2.10 storage levels
```
There are tradeoffs between the different storage levels. 

1). Basically if your RDD fits within the default storage level, by all means, use that.
    It is the fastest option to fully take advantage of Spark's design. 

2). If not, you can serialized the RDD and use the MEMORY_ONLY_SER level. Just be sure to choose a fast serialization
    library to make the objects more space efficient and still reasonably fast to access.

3). Don't spill to disk unless the functions that compute your datasets are expensive or it requires a large amount of space.

4). If you want fast recovery, use the replicated storage levels. All levels are fully fault
    tolerant, but would still require the recomputing of the data. If you have a replicated copy,
    you can continue to work while Spark is reconstruction a lost partition.

5. Finally, use Tachyon if your environment has high amounts of memory or multiple applications.
   It allows you to share the same pool of memory and significantly reduces garbage collection
   costs. Also, the cached data is not lost if the individual executors crash.
```

2.11 Shared Variables and key-value pairs operations
```
Spark provides two limited types of shared variables for common usage patterns: 
broadcast variables and accumulators. 

1). Normally, when a function is passed from the driver to a worker, a separate copy of the variables are used for each worker. 
    Broadcast variables allow each machine to work with a read-only variable cached on each machine. Spark attempts
    to distribute broadcast variables using efficient algorithms. As an example, broadcast variables
    can be used to give every node a copy of a large dataset efficiently.

2). The other shared variables are accumulators. 
    These are used for counters in sums that works well in parallel. 
    These variables can only be added through an associated operation.
    Only the driver can read the accumulators value, not the tasks. The tasks can only add to it. 

key-value pairs are available in Scala, Python and Java. 

1). In Scala, you create a key-value pair RDD by typing val pair = ('a' , 'b'). 
    To access each element, invoke the ._ notation. This is not zero-index, so the ._1 will 
    return the value in the first index and ._2 will return the value in the second index. 

2). Java is also very similar to Scala where it is not zero-index. 
    You create the Tuple2 object in Java to create a key-value pair. 

3). In Python, it is a zero-index notation, so the value of the first index resides in index 0 
    and the second index is 1.

There are special operations available to RDDs of key-value pairs. 
In an application, you must remember to import the SparkContext package to use PairRDDFunctions such as reduceByKey.

1). The most common ones are those that perform grouping or aggregating by a key. RDDs containing
    the Tuple2 object represents the key-value pairs. Tuple2 objects are simply created by
    writing (a, b) as long as you import the library to enable Spark's implicit conversion.
    If you have custom objects as the key inside your key-value pair, remember that you will
    need to provide your own equals() method to do the comparison as well as a matching hashCode()
    method.

2). Explain some of the syntax that you see
    on the slide. Note that in the first reduceByKey example with the a,b => a + b. This simply
    means that for the values of the same key, add them up together. In the example on the
    bottom of the slide, reduceByKey(_+_) uses the shorthand for anonymous function taking
    two parameters (a and b in our case) and adding them together, or multiplying, or any other
    operations for that matter.
```

#### 2.12 DataFrame:
1) Since spark 1.3, Spark SQL introduces a tabular data abstraction call DataFrame. 
Unlike an RDD, data is organized into named columns, like a table in a relational database.

2) It uses RDD's features that immutable, in-memory, resilient, distributed, and parallel. And more, it applies a
structure called schema to the data, allowing Spark to manage the schema and only pass data between noddes, 
in a much more efficient way than using java serialization.

3) It provides actions to run SQL queries which is not available in RDD.

4) A DataFrame is a data abstraction or a domain-specific language for working with **strctured and semi-structured data**.

5) DataFrames takes advantage of their schema that stores data in a more efficient manner than native RDDs.

#### 2.13 DataSet:
1. Dataset API is released since Spark 1.6.

2. It provides:
    * the familiar object-oriented programming style
    * compile-time type safety of the RDD API
    * the benefits of leveraging schema to work with structured data 

3. A dataset is set of **strctured data**, not necessarily a row but it could be of a particular type.
4. Java and Spark will know the type of the data in a dataset at compile time.
5. The Dataset API is **not available in Python**.


#### 2.14 DataFrame and Dataset
1. Starting in Spark 2.0, DataFrame APIs merge with Dataset APIs.

2. Dataset takes on two distinct APIs characteristics:
    * strongly-typed API: same as old Dataset that is a collection of strongly-typed JVM objects
    * untyped API: consider DataFrame as untyped view of a Dataset, which is a Dataset of Row where a Row is a generic
    untyped JVM object.

3. The Dataset API is **only available in JAVA and Scala, not in Python**.
4. For Python, you have to stick with the DataFrame API.


#### 2.15 DataFrame or RDD
1. MLlib is on a shift to DataFrame based API
2. Spark streaming is also moving towards, something called structured streaming which is heavily based on DataFrame API
3. RDD is not being deprecated. It is still the core and fundamental building block of Spark. Both DataFrame and dataset
 are built on top of RDDs.
4. Choose RDD:
    * low level transformation, actions, and control on our dataset are needed
    * Unstructured data, such media streams or streams of text
    * Need to manipulate our data with functional programming constyructs than domain specific expression.
    * Optimization and performance benefits available with DataFrames are not needed.

5. Choose Dataframe:
    * Rich semantics, high level abstractions, and domain specific APIs are needed
    * Processes require aggregation, averages, sum, SQL queries and columnar access on semi-structured data
    * We want the benefit of Catalyst optimization.
    * Unification and simplification of APIs across Spark libraries are needed

6. In summary:
    * Consider using dataframes over RDDs, if possible.
    * RDD will remain to be the one of the most critical core componets of spark, and it is the underlying building block for dataframes.


### Module 03: Spark application programming
3.1 SparkContext is the main entry point to everything Spark.
```
It can be used to create RDDs and shared variables on the cluster. When you start up the Spark Shell, the SparkContext
is automatically initialized for you with the variable sc. 
```
3.2 Run Spark
```
1). To run Spark applications in Python, use the bin/spark-submit script located in the Spark's
    home directory. This script will load the Spark's Java/Scala libraries and allow you
    to submit applications to a cluster. 

    For example, val conf = new SparkConf().setAppName(appName).setMaster(master).

    You set the application name and tell it which is the master node. The master parameter can
    be a standalone Spark distribution, Mesos, or a YARN cluster URL. You can also decide
    to use the local keyword string to run it in local mode. In fact, you can run local[16]
    to specify the number of cores to allocate for that particular job or Spark shell as 16.
   
2). For production mode, you would not want to hardcode the master path in your program.
    Instead, launch it as an argument to the spark-submit command.
    Once you have the SparkConf all set up, you pass it as a parameter to the SparkContext
    constructor to create the SparkContext
```

3.3 Passing functions to Spark. 
```
There are three methods to pass functions.
1). Anonymous function. 

2). Use static methods in a global singleton object. 
    This means that you can create a global object. Inside that object, you basically define the function func1. 
    When the driver requires that function, it only needs to send out the object, 
    then worker will be able to access it.

3. Pass reference to a method in a class instance, as opposed to a singleton object. 
   This would require sending the object that contains the class along with the method.
   To avoid this consider copying it to a local variable within the function instead of accessing it externally.
   
Example, say you have a field with the string Hello. You want to avoid calling that directly
inside a function as shown on the slide as x => field + x.
Instead, assign it to a local variable so that only the reference is passed along and
not the entire object shown val field_ = this.field.

For an example such as this, it may seem trivial, but imagine if the field object is not a simple
text Hello, but is something much larger, say a large log file. In that case, passing
by reference will have greater value by saving a lot of storage by not having to pass the
entire file.

You create the RDD from an external dataset or from an existing RDD. You use transformations
and actions to compute the business logic. You can take advantage of RDD persistence,
broadcast variables and/or accumulators to improve the performance of your jobs.
Here's a sample Scala application. You have your import statement. After the beginning
of the object, you see that the SparkConf is created with the application name. Then
a SparkContext is created. The several lines of code after is creating the RDD from a text
file and then performing the Hdfs test on it to see how long the iteration through the
file takes. 

Finally, at the end, you stop the SparkContext by calling the stop() function.
```

3.4 To submit your application to the Spark cluster
```
Use spark-submit command, which is located under the $SPARK_HOME/bin directory.

The class option is the main entry point to your class. If it is under a package name,
you must provide the fully qualified name. The master URL is where your cluster is located.
Remember that it is recommended approach to provide the master URL here, instead of hardcoding
it in your application code.

The deploy-mode is whether you want to deploy your driver on the worker nodes (cluster)
or locally as an external client (client). The default deploy-mode is client.

The conf option is any configuration property you wish to set in key=value format.
The application jar is the file that you packaged up using one of the build tools.

Finally, if the application has any arguments, you would supply it after the jar file.
Here's an actual example of running a Spark application locally on 8 cores. The class
is the org.apache.spark.examples.SparkPi. local[8] is saying to run it locally on 8
cores. The examples.jar is located on the given path with the argument 100 to be passed
into the SparkPi application.
```

### Module 04: Spark Libraries
4.1 Spark libraries including four libraries: 
```
1). SparkSQL, Spark Streaming, MLlib and GraphX. 
2). These libraries are an extension of the Spark Core API. 
3). Any improvements made to the core will automatically take effect with these libraries. 
4). One of the big benefits of Spark is that there is little overhead to use these libraries with Spark 
    as they are tightly integrated. 
```
4.2 Spark SQL

Catalyst Optimizer:
1. Spark SQL uses an optimizer called Catalyst to optimize all the queries written both in spark sql and dataframe DSL.
2. The optimizer makes queries run much faster than their RDD counterparts.
3. the catalyst is a modular library which is build as a rule based system. Each rule
in the framework focuses on the specific optimiztion. For example,
rule like CounstantFolding focuses on removing constant expression from the query.


Spark SQL join Vs. core Spark join
1. Spark SQL supports the same basic join types as core Spark.
2. Spark SQL Catalyst optimizer can do more of the heavy lifting for us to optimize the join performance.
3. We have to give up some of our control when using Spark SQL join. For example, spark sql can sometimes push down or re order operations to make the joins more efficient. The downside is that
we do not have controls ove the paritiioner for the dataframes, so we can not manually avoid shuffles as we did with core sparks joins.


Spark SQL Join Types:
    * inner
    * outer
    * left outer
    * right outer
    * left semi: only keep the keys that in left table
    
    
Spark SQL performance tuning:
    * Caching: caching the dataframe, which can be done by calling the cache method on the dataframe. ``responseDataFrame.cache()``
    * When caching a datafream, spark sql use san in memory columar storage for the dataframe.

```
1). Spark SQL allows you to write relational queries that are expressed in either SQL, HiveQL, 
    or Scala to be executed using Spark. 

2). Spark SQL has a new RDD called the SchemaRDD. 
    The SchemaRDD consists of rows objects and a schema that describes the type of data in each column
    in the row. You can think of this as a table in a traditional relational database.

3). You create a SchemaRDD from existing RDDs, a Parquet file, a JSON dataset, or using HiveQL
    to query against the data stored in Hive. You can write Spark SQL application using
    Scala, Java or Python.
    
4). The SQLContext is created from the SparkContext. You can see here that in Scala, the sqlContext
    is created from the SparkContext. In Java, you create the JavaSQLContext from the JavaSparkContext.

5). There are two ways to create these SchemaRDDs.
    a. The first method uses reflection to infer the schema of the RDD. 
       This leads to a more concise code and works well when you already know the schema while 
       writing your Spark application.
       
       It uses the case class in Scala to define the schema of the table. The arguments
       of the case class are read using reflection and becomes the names of the columns.
       
       First thing is to create the RDD of the person object. You load the text file in using the textFile method. 
       
       Then you invoke the map transformation to split the elements on a comma to get the individual columns of name and age. 
       
       The final transformation creates the Person object based on the elements.
       
       Next you register the people RDD that you just created by loading in the text file and performing the transformation as a table. Once the RDD is a table, you use the sql method provided by SQLContext to run SQL statements.
       
       Finally, the results that comes out from the select statement is also a SchemaRDD.
       That RDD, teenagers on our slide, can run normal RDD operations.

    b. The second method uses a programmatic interface to construct a schema and then apply that
       to an existing RDD. This method gives you more control when you don't know the schema
       of the RDD until runtime. 

       It is used when cannot define the case classes ahead of time. For example, when the structure of 
       records is encoded in a string or a text dataset will be parsed and fields will be projected different for different users.
       
       A schemaRDD is created with three steps.
       
       The first is to create an RDD of Rows from the original RDD. In the example, we create
       a schemaString of name and age.
       
       The second step is to create the schema using the RDD from step one. The schema is represented
       by a StructType that takes the schemaString from the first step and splits it up into
       StructFields. In the example, the schema is created using the StructType by splitting
       the name and age schemaString and mapping it to the StructFields, name and age.
       
       The third step convert records of the RDD of people to Row objects. Then you apply the
       schema to the row RDD. Once you have your SchemaRDD, you register that RDD as a table 
       and then you run sql statements against it using the sql method.
       The results are returned as the SchemaRDD and you can run any normal RDD operation on
       it. In the example, we select the name from the people table, which is the SchemaRDD.
       Then we print it out on the console.
```

4.3 Spark streaming
```
1). It gives you the capability to process live streaming data in small batches.

Utilizing Spark's core, Spark Streaming is scalable, high-throughput and fault-tolerant.

You write Stream programs with DStreams, which is a sequence of RDDs from a stream of data.

2). Input: There are various data sources that Spark Streaming receives from including, Kafka,
Flume, HDFS, Kinesis, or Twitter. 

3). Output: It pushes data out to HDFS, databases, or some sort of dashboard.

4). Steps: 
a. First the input stream comes in to Spark Streaming. 

b. Then that data stream is broken up into batches of data that is fed into the Spark engine for processing. 
Once the data has been processed, it is sent out in batches.

c. Spark Stream support sliding window operations. 
In a windowed computation, every time the window slides over a source of DStream, 
the source RDDs that falls within the window are combined and operated upon to produce the resulting RDD.

There are two parameters for a sliding window. The window length is the duration of the window
and the sliding interval is the interval in which the window operation is performed. Both
of these must be in multiples of the batch interval of the source DStream.

In the diagram, the window length is 3 and the sliding interval is 2. To put it in a different perspective, 
say you wanted to generate word counts over last 30 seconds of data, every 10 seconds. To do this, you would apply the reduceByKeyAndWindow operation on the pairs of DStream of (Word,1) pairs over the last 30 seconds of data.


Example. We want to count the number of words coming in from the TCP socket. 
1) First and foremost, you must import the appropriate libraries. 
2) Then, you would create the StreamingContext object. In it, you specify to use two threads with the batch
interval of 1 second. 
3) Next, you create the DStream, which is a sequence of RDDs, that
listens to the TCP socket on the given hostname and port. Each record of the DStream is a
line of text. You split up the lines into individual words using the flatMap function.
The flatMap function is a one-to-many DStream operation that takes one line and creates
a new DStream by generating multiple records (in our case, that would be the words on the
line). 
4) Finally, with the words DStream, you can count the words using the map and reduce
model. Then you print out the results to the console.

Note: when each element of the application is executed, the real processing doesn't actually happen yet. You have to explicitly tell it to start. Once the application begins, it will continue running until the computation terminates. The code snippets that you see
here is from a full sample found in the NetworkWordCount. To run the full example, you must first start up netcat, which is a small utility found in most Unix-like systems. This will act as a data source to give the application streams of data to work with. Then, on a different terminal window, run the application using the command shown here.
```

4.4 MLlib library 
```
it contains algorithms and utilities for classification, regression, clustering, collaborative filtering
and dimensionality reduction. 

Essentially, you would use this for specific machine learning use cases that requires these algorithms. 

In the lab exercise, you will use the clustering K-Means algorithm on a set of taxi drop off points to 
figure out potentially where the best place to hail a cab would be.
```

4.5 GraphX
```
1). It is basically a graph processing library which can used for social networks and language modeling. 

Graph data and the requirement for graph parallel systems is becoming more common, which is why the
GraphX library was developed. 

Specific scenarios would not be efficient if it is processed using the data-parallel model. 

A need for the graph-parallel model is introduced with new graph-parallel systems like Giraph and GraphLab to efficiently execute graph algorithms much faster than general data-parallel systems.

There are new inherent challenges that comes with graph computations, such as constructing
the graph, modifying its structure, or expressing computations that span several graphs. As
such, it is often necessary to move between table and graph views depending on the objective
of the application and the business requirements.

The goal of GraphX is to optimize the process by making it easier to view data both as a graph and as collections, such as RDD, without data movement or duplication.
```


### Module 05: Spark configuration,monitoring and tuning
5.1 Spark Cluster
```
Three main components of a Spark cluster. 
1) Driver, where the SparkContext is located within the main program. 
2) Run on a cluster via cluster manager. Spark's standalone cluster manager, Mesos or Yarn.
3) wWorker nodes where the executor resides. The executors are the processes that run computations 
   and store the data for the application. The SparkContext sends the application, defined as JAR or 
   Python files to each executor. Finally, it sends the tasks for each executor to run.
```

5.2 Spark Cluster Architecture
```
1). Each application gets its own executor. 
2). The executor stays up for the entire duration of the application. The benefit of this is
    that the applications are isolated from each other, on the scheduling side and running
    on different JVMs. However, this means that you cannot share data across applications.
    You would need to externalize the data if you wish to share data between the different
    applications.
    
3). Spark applications don't care about the underlying cluster manager. As long as it can acquire
    executors and communicate with each other, it can run on any cluster manager.
    Because the driver program schedules tasks on the cluster, it should run close to the
    worker nodes on the same local network. If you like to send remote requests to the cluster,
    it is better to use a RPC and have it submit operations from nearby.
```

5.3 Spark configuration
```
Three main locations for Spark configuration: 
1) Spark properties, where the application parameters can be set using the SparkConf object or through Java system properties.

2) Environment variables, which can be used to set per machine settings such as IP address. 
   This is done through the conf/spark-env.sh script on each node.

3) Logging properties, which can be configured through log4j.properties.
```

5.4 Setting Spark Properties
```
Two methods of setting Spark properties. 

1). Passing application properties via the SparkConf object. 
    As you know, the SparkConf variable is used to create
    the SparkContext object. In the example shown on this slide, you set the master node as
    local, the appName as "CountingSheep", and you allow 1GB for each of the executor processes.

2). Dynamically set the Spark properties. 
    Spark allows you to pass in an empty SparkConf when creating the SparkContext.
    You can then either supply the values during runtime by using the command line options
    --master or the --conf. 
    
3). Another way to set Spark properties is to provide your settings inside the spark-defaults.conf file. 
    The spark-submit script will read in the configurations from this file. 
    You can view the Spark properties on the application web UI at the port 4040 by default.

Note: properties set directly on the SparkConf take highest precedence, then flags passed to spark-submit 
or spark-shell is second and finally options in the spark-defaults.conf file is the lowest priority.
```

5.5 Monitor Spark Applications
```
Three ways to monitor Spark applications. 

1). Web UI. 
The default port is 4040. The port in the lab environment is 8088. 
The information on this UI is available for the duration of the application. 
If you want to see the information after the fact, set the spark.eventLog.enabled to true before starting the application. 
The information will then be persisted to storage as well.

2). Metrics
Metrics is another way to monitor Spark applications. 
The metric system is based on the Coda Hale Metrics Library. 
You can customize it so that it reports to a variety of sinks such as CSV.
You can configure the metrics system in the metrics.properties file under the conf directory.

3). External instrumentations to monitor Spark. 
Gangalia is used to view overall cluster utilization and resource bottlenecks. 
Various OS profiling tools and JVM utilities can also be used for monitoring Spark.


The Web UI is found on port 4040, by default, and shows the information for the current application while it is running.
The Web UI contains the following information. A list of scheduler stages and tasks. A summary of RDD sizes and memory usage. Environmental information and information about the running executors.

To view the history of an application after it has ran, you can start up the history server.
The history server can be configured on the amount of memory allocated for it, the various
JVM options, the public address for the server, and a number of properties.
```

5.6 Spark Tuning
```
Spark programs can be bottlenecked by any resource in the cluster. Due to Spark's nature
of the in-memory computations, data serialization and memory tuning are two areas that will
improve performance. 

1). **Data serialization**
It is crucial for network performance and to reduce memory use. 
It is often the first thing you should look at when tuning Spark applications.

Spark provides two serialization libraries. 
**Java serialization** (default) provides a lot more flexibility, but it is quiet slow and leads to large serialized objects.  

**Kyro serialization** is much quicker than Java, but does not support all Serializable types. 
It would require you to register these types in advance for best
performance. To use Kyro serialization, you can set it using the SparkConf object.

2). **Memory tuning**
Consider three things:
a. The amount of memory used by the objects (whether or not you want the entire object to fit in memory). 
b. The cost of accessing those objects.
c. The overhead garbage collection.

You can determine how much memory your dataset requires by creating a RDD, put it into cache,
and look at the SparkContext log on your driver program. Examining that log will show you
how much memory your dataset uses.

Few tips to reduce the amount of memory used by each object. 
a. Try to avoid Java features that add overhead such as pointer based data structures and wrapper objects. 
b. If possible go with arrays or primitive types and try to avoid nested structures.
c. Serialized storage can also help to reduce memory usage. The downside would be that it
   will take longer to access the object because you have to deserialized it before you can use it.

d. You can collect statistics on the garbage collection to see how frequently it occurs
   and the amount of time spent on it. To do so, add the line to the SPARK_JAVA_OPTS environment variable.

e. The level of parallelism should be considered in order to fully utilize your cluster. It
   is automatically set to the file size of the task, but you can configure this through optional
   parameters such as in the SparkContext.textFile. You can also set the default level in the
   spark.default.parallelism config property. Generally, it is recommended to set 2-3 tasks
   per CPU core in the cluster.

f. Sometimes when your RDD does not fit in memory, you will get an OutOfMemoryError. In cases
   like this, often by increasing the level of parallelism will resolve this issue. By increasing
   the level, each set of task input will be smaller, so it can fit in memory.

g. Using Spark's capability to broadcast large variables greatly reduces the size of the
   serialized object. A good example would be if you have some type of static lookup table.
   Consider turning that into a broadcast variable so it does not need to be passed on to each
   of the worker nodes.

h. Spark prints the serialized size of each tasks in the master. Check that out to examine if
   any tasks are too large. If you see some tasks larger than 20KB, it's worth taking a look
   to see if you can optimize it further, such as creating broadcast variables.
```


### Module 06: RDD Architecture
* How Spark generates RDDs
* Managing partitions to improve RDD performance
* What makes an RDD resilient
* How RDDs are broken into jobs and stages
* Serialize tasks


### Module 07: 
* Advanced RDD operations
* The operations that cause shuffling
* How to avoid shuffling when possible
* How to group, combine, and reduce key value pairs

Slide 1: Advanced RDD operations:

8.1 Statistical operations on numerical RDDs
```
1). histogram, mean, stdev, sum,variance,max,min
2). stats() returns all of the statistic values
```

mapPartitions, mapPartitionsWithIndex
--Map by partition(many to many) instead of single value/pari (one to one)
--Useful when map use function has a hifh overhead cost
 example: connect to a database once per partition instead of for each record
 
 
foreachPartition
--use for batching operations


Approximate calculations
--get approximate results for very large data sets
```
def countApproxDistinct(relativeSD: Double = 0.5): Long
```

fold(zeroValue)((acc,value)=>acc)
-- Similar to reduce, but has initial "zeroed" accumulator
--Functions uses the accumulator and RDD element to update accumulator


aggregate(zeroValue)(accumulate, combine)
--Perform complex aggregations
--accumulate by partition then combine results


countByValue
```
val text = sc.textFile(README.md")
// following operations are identical
val counts = text.flatMap(_.split("")).map((_,1)).reduceByKey(_+_).collectAsMap()
val counts = text.flatMap(_.split("")).countByValue()
```

Slide two: Operations on key/value RDDS

reduceByKey
--reduce over  all values of a key
-- combine results for each key in partition


countByKey
--calls reduce(_+_)
--collects results from every partition in a map sent back to driver
--mostly for testing /development convenience

aggregateByKey
-- aggregate over each key
--combine for each key of each partition

foldByKey


groupByKey
-- group all values by key from all partitions into memory
--potential cause of OOM errors
-- Intuitive, but avoid using when possible

lookup
-- Return all values for specified key

mapValues
--Apply a map function to each value without changing key
--Spark optimizer knows partitions are still good after


repartitionAndSortWithinPartitions
--More effificent than calling repartition then sortByKey


slide three: example-calculate avg per key

calculate the average value for each key

-- method 1: use groupByKey()
```
val keyedRDD: RDD[(String, Int)]=???
keyedRdd.groupByKey().mapValues(I => I.sum / I.size)
```

-- method 2: better way to use aggregateByKey()
```
val results = keyedRdd.aggregateByKey((0,0))(
    (acc,value)=>(acc._1 + value, acc._2 + 1),
    (acc1, acc2)=>(acc1._1 + acc2._1, acc1._2 + acc2._2)
)

results.mapValues(i => i._1 / i._2)
```

example 2: get the first value of each key

--use groupByKey()
```
val keyedRdd:RDD[(String,Trip)]=???
keyedRdd.groupByKey().mapValues(I=>I.head)
```

-- better way to use reduceByKey()
```
val keyedRdd:RDD[(String,Trip)]=???
keyedRdd.reduceByKey((a,b)=>{
    a.startDate <b.startDate match {
        case true => a
        case true => b
    }
})
```


slide four: Asynchronous actions

actions are blocking operations

Asynchronous versions of basic actions

Implemented as futures:
-- collectAsync
-- countAsync
--foreachAsync
--foreachPartitionAsync
--taskAsync

will still complete serially with default FIFO scheduler
-- must use FAIR scheduler to take advantage of async


slide five: Map vs MapValues

HashPartitioned key-value RDD -> map -> reduceByKey (a full shuffle is going to occur)

(If  you just need to operate on the values of each key/pair, always use mapValues)
reduceBykey won't run a full shuffle and be aware of the partitioning of the keys

Slide six: Batching output operations

user saveAsHadoop APIs when possible

foreach: one record at a time

foreachPartition: one partition at a time

If you need to save to DB, look at DBOutputFormat

Use forEachPartion for sending to message queues, REST endpoints, etc.
```
val rdd = ???
rdd.foreachPartition { p=>
    val conn = initConnection()
    val json_collection = serialize(p)
    coon.post(json_collection)
}
// serializing a whole partition
```



slide seven: broadcast variables

share a value with every machine
- read-only
- can be used to avoid shuffling

broadcast variable accessible form any RDD (advantage: avoid shuffling)

shared via peer-to-peer BitTorrent protocol
- Nodes communicating with each other to optimize throughput
- Doesn't overload master

why not just pass a local variable in closure?
- Entire closure serialized and sent to every node
- Delays startup and waste of space


slide eight: broadcast example
```
val stations = input2.map(_.split(","))
                     .map(Station.parse(_))
                     .keyBy(_.id)
                     .collectAsMap()
                     
val sbc = sc.broadcast(stations)
val joined = trips.map(trip => {
    (trip, sbc.value.getOrElse(trip.startTerminal, Nil),
     sbc.value.getOrElse(trip.endTermainal,Nil))
})

// No shuffle !
```
 
###Module 09: Caching and Serialization
* How and when to cache RDDs
* Storage levels and their uses
* How to optimize memory usage with serialization options
* Share RDDs with Tachyon


slide one: Persistence

Spark's power comes from memory caching
- Minimal disk I/O

Important to know when to persist RDDs
- Memory/dis combination for large data
- save to disk to avoid losing results of exprensive operation

Good idea to persist after filter, other prep for downstream processing

Call unpersist() when no longer needed
- Spark uses LRU algorithm to make room when needed
```
val input1 = sc.textFile("data/trips/*.csv")

val trips = input1.filter(!_.startsWith("Trip ID")).map(_.split(","))
                  .map(trip.parse(_))
                  .keyBy(_.startTermainal)
                  .partitionBy(new HashPartitioner(4))
                  
```

cache() == persist(StorageLevel.MEMORY_ONLY)


slide two: Storage cost

Persistence uses memory

Raw java objects are fast but take up most space

Instead, we can serialize:
- Less space at cost of CPU
- Stored as byte array
- Also helps with garbage collection(1 array vs many objects)

Defaults to Java Serialization (Python uses pickle)

Kryo is more efficient, but more configuration( or compatibility ) involved

compress to save further space at CPU cost of decompressing

Primitive types store better than java/scala collections and heavily nested classes


slide three: Storeage level comprarison

RDD from 36MB CSV (Using java serialization)
- deserialized 162.8 MB
- serialized 52.3 MB

Usering Kryo
- serialized 34.1 MB (smaller than the original file)

Simple Kryo configuration
```
val conf = new SparkConf().setAppName("lab4")
conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
conf.registerKryoClasses(Array(classOf[Trip],classOf[Station]))

val sc = new SparkContext(conf)

val data: RDD[Trip] = _

data.persist(StorageLevel.MEMORY_ONLY_SER)
```

slide four: can we share RDDs?

Not conventionally
- RDD tied to SparkContext

Tachyon project: Off-heap cache for Spark

Periodically save to or read from Tachyon

Can persist RDDs for other spark apps to use
```
rdd.persist(StorageLevel.OFF_HEAP)
```

slide five: Memory configuration

spark.rdd.compress(default=false)

spark.shuffle.consolidateFiles(default=false)
- Set true on ext4 or xfs file systems
- May degrade performance on ext3 with > 8 cores

spark.shuffle.spill(default=true)
- Limit memory used during reduces by spilling to disk
- spark.shuffle.memoryFraction(default=0.2)
- spark.shuffle.spill.compress(default=true)


spark.storage.memoryFraction(default=0.6)
- how much storeage can be dedicated to in-memory persistence


spark.storeage.unrollFraction(default=0.2)
- memory dedicated to "unrolling" serialized data


###Module 10: Develop and Testing
* How to use sbt to build Spark projects
* How to use Eclipse and IntelliJ for Spark development
* How to unit test your spark projects, and manage dependencies.

slide one: build tools

sbt (simple build tool)
- build-in REPL (build and run from terminal)
- Simple, but ppowerful and extensible
- Plugins for Eclipse and support in IntelliJ

Maven:
- Not as powerful or customizable as sbt

sbt.build file
```
name := "adv-spark-labs"
version:= "0.0.1"
scalaVersion := "2.10.4"
val sparkVersion="1.2.1"

libraryDeprendencies ++=Seq("org.apache.spark" %% "spark-core" % sparkVersion)
```

use sbt to modify name
```
> set name := "my-project"
```

```
> sbt eclipse
```

slide: SBT and Intellij

Much cleaner sbt integration with plugin

Scala console build-in

Supports scalatest

Debug support using remote debugger


slide: Unit testing your spark apps

Isolate your RDD operations
- Encapsultate logic in its own object / class
- Same code is run during test as in application


Use standard unit testing tools, like scalatest

Spark packages: spark-test-base
- Helper classes to facilitate testing Spark apps


```
libraryDependencies ++= Seq(
    "org.apache.spark"%% "spark-core" % sparkVersion % "provided",
    "com.holdenkarau" %% "spark-testing-base" % "1.2.0_1.1.1_0.0.6" % "test",
    "org.scalatest"%% "scalatest"% "2.2.1"% "test"
)
```

examples:
```Scala
class SampleTests extends FunSuite with SharedSparkContext {
    
    test("word count") {
        val input = "hello hello world"
        val expected = Map("hello"->2, "world"-> 1)
        
        val counts = sc.makeRDD(Seq(input)).flatMap(_.split(""))
                                           .map((_,1))
                                           .reduceByKey(_+_)
                                           .collectAsMap
        
        assertResult(expected){
            counts
        }                                   
    }
    
    test("line parser") {
        val input = "helloworld"
        val expected = Seq("hello", "world")
        var result = input.split(" ")
        
        assertResult(expected) {
            result
        }
    }

}
```



slide: Manager dependencies

additional libraries must by distributed to workers

with spark-submit, use --jar flag
- can reference http, ftp, hdfs

bundle everything into one big "fat jar"

Easy in sbt
- addSbtPlugin("com.eed3wsi9n" % "sbt-assembly" % "0.11.2")
- run sbt assembly
- 
Done


