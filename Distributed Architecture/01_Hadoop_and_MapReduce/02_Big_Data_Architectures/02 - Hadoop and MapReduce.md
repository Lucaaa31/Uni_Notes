# Apache Hadoop

Apache Hadoop is a scalable fault-tolerant distributed system for Big Data, here's some properties:
-   Distributed Data Storage
-   Distributed Data Processing
-   Borrowed concepts from the systems designed at Google
## Main Components

Hadoop has two core components:
-   **Distributed Big Data Processing Infrastructure based on the MapReduce programming paradigm**
    -   Provides high-level abstraction view, so that programmers do not need to care about scheduling and synchronization
    -   Fault-tolerant, it automatic manages node and task failures
-   **Hadoop Distributed File System (HDFS)**
    -   High availability distributed storage
    -   Fault tolerant

![[02 - Hadoop and MapReduce-1778183392503.webp ]](Example of the Hadoop components with two of replicas per chunk)

In this image the $C$ represents the **chucks**, they are units that the HDFS uses to divide large files.
## Distributed Big Data Processing Infrastructure
Hadoop programs, as already said, are based on the MapReduce programming paradigm. The MapReduce framework automatic handles the distributed problems like scheduling and synchronization. But it is important to know how it works in order to develop efficient applications.
# MapReduce
Let's start introduce MapReduce by going an example with one of the basic tasks: the word count.

## Word Count
The problem is described as it follows:
-   **Input:** A large textual file of words
-   **Problem:** Count the number of times each distinct word appears in the file
-   **Output:** A list of pairs in the following format \<word, number\>

The implementation of the solution depends from the complexity of the
input, we can identify two cases:
1.  **The entire file fits in main memory:** For this problem probably the best approach is using a **single node**. It is the most efficient one because complexity and overheads of distributed systems affects the performance when the files are small
2.  **The file is too large to fit in main memory**
### Word Count with a very large file
For solving this problem we want to split it in a set of (almost)
**independent sub-tasks** and execute them **in parallel** on a cluster
of servers.

We can suppose the following:
-   The cluster has 3 servers
-   The input file contains this phrase: "Toy example file for Hadoop. Hadoop running example."
-   The input file is split into 2 chunks
-   The number of replicas is 1

![[02 - Hadoop and MapReduce-1778183742778.webp]](This is the entire process for the Word Count problem!)

The entire process works in this way:
1.  The input is split and sent to the two servers
2.  The servers process the input and produce their local list of pairs \<word, number\>
3.  Then, they send it to the third server
4.  This server is in charge of **aggregating** all the local results into a global result

The server that aggregates the result has to receive all the local parts before compute, so in this phase we will need a synchronization operation.

## Scalability
We can define scalability along two dimensions:
-   **In terms of data:** Given twice the amount of data, the word count algorithm takes approximately no more than twice as long to run
-   **In terms of resources:** Given twice the number of servers, the word count algorithm takes approximately no more than half as long to run

For this example we have excluded the time needed to send local result to the node in charge of computing the final result, but in real time applications it has to be considered.
## Properties
These are the MapReduce-approach key ideas:
-   **Scale out, not up:** Increase the number of servers, instead of upgrading the resources
-   **Data locality:** Move the process (the code, the algorithm) to the data, because the network bandwidth is lesser then the physical memory
-   **Process data sequentially**, because the seek operations are expensive and usually the big data applications require to analyze all the records, so random access is useless

So, Hadoop/MapReduce is designed for:
-   Batch processing involving (mostly) full scans of the input data
-   Data-intensive applications:
    -   Read and process the whole Web
    -   Read and process the whole Social Graph
    -   Log analysis
In the other hand, Hadoop/MapReduce does not feet well with:
-   Iterative problems
-   Recursive problems
-   Stream data processing
-   Real-time processing
# The MapReduce Programming Paradigm
The MapReduce programming paradigm is based on the basic concepts of
Functional programming. Everything is based on two function with
predefined signatures, Map and Reduce:

-   **Map function:** it is applied over each element of the input and produces a set of **(key, value) pairs**. It can be viewed as a **transformation** and it is defined by the developer

-   **Reduce function:** it is applied to each pairs, produced by the Map, with the same key and produces a set of (key, value) pairs that are our final result. It can be seen as an **aggregate operation**, defined by the developer

**N.B.:** the map reduce is applied **isolated** for each elements, it does not have memories of the elements processed before
## Word count running example
Here's an example applied to the Word count example seen before, the
input is a list of words:
![[02 - Hadoop and MapReduce-1778183955116.webp]]

We can distinguish **three phases**:
-   **Map phase:** we apply a function on **each element**, in this case we produce set $<k, v>$ where $k$ is a word and $v$ is $1$, because it is applied isolated for each element

-   **Shuffle and Sort phase:** we group by key

-   **Reduce phase:** we apply a function to each group, in this case we sum the values

**N.B.:** the shuffle and sort phase is always the same, it is provided by the Hadoop system.
## Formal definition of Map and Reduces functions
The map and reduce functions are formally defined as follows:
-   map: $(k_1, v_1) \rightarrow [(k_2, v_2)]$
-   reduce: $(k_2, [v_2]) \rightarrow [(k_3, v_3)]$
So:
-   The map function **returns a list**
-   The reduce function is **invoked once for each distinct key**,
    receives the **list of values** of the key and **returns a list of
    keys-values**
## Pseudocode
```java
map(key, value):
  // key: offset of the word in the file
  // value: a word of the input document
  emit(value, 1)

reduce(key, values):
  // key: a word; values: a list of integers
  occurrences = 0
  for each c in values:
    occurrences = occurrences + c
  emit(key, occurrences)
```
# How to Write MapReduce programs in Hadoop

The programming language used is **Java**. Hadoop MapReduce programs consists in **three phases**:
-   Driver
-   Mapper
-   Reducer
Each parts in implemented using a **Java class**.
## Driver
The Driver is the main class and the entry point of the application. It
is characterized by the **main() method**, that accepts arguments from
command line. Other properties:
-   Configures the job
-   Submits the job to the Hadoop Cluster
-   Coordinates the workflow of the application
-   Runs on the client machine
## Mapper
The Mapper implements the map phase. It is characterized by the **map()
method**. As already seen:
-   Processes the (key, value) pairs of the input file and emits (key, value) pairs
-   Is invoked one time for each input (key, value) pair
It runs on the cluster.
## Reducer
The Reducer implements the reduce phase. It is characterized by the
**reduce() method**. As already seen:
-   Processes (key, \[list of values\]) pairs and emits (key, value) pairs
-   Is invoked one time for each distinct key
It runs on the cluster.
## Hadoop Implementation of the MapReduce phases
Here's the workflow of an execution:
1.  The Input key-value pairs are read from the HDFS file system
2.  The map method of the Mapper:
    -   Is invoked over each input key-value pair
    -   Emits a set of intermediate key-value pairs that are **stored in the local** file system on the computing server, they are not stored in HDFS
3.  The intermediate results:
    -   Are aggregated by means of a shuffle and sort procedure
    -   A set of (key, \[list of values\]) pairs is generated
    -   This data are transient, they are stored in the RAM
4.  The reduce method of the Reducer:
    -   Is applied over each (key, \[list of values\]) pair
    -   Emits a set of key-value pairs that are **stored in HDFS**, that are the final result

![[02 - Hadoop and MapReduce-1778184225155.webp|500]]
![[02 - Hadoop and MapReduce-1778184216932.webp|500]]
# MapReduce programs

## Driver
The Driver class extends the `org.apache.hadoop.conf.Configured` class and implements the `org.apache.hadoop.util.Tool` interface. For this class we implements **main()** and **run()** methods:
-   The **run()** method configures the job:
    -   Name of the Job
    -   Job Input and Output format
    -   Mapper class: Name, type of input and type of output (key, value) pairs
    -   Reducer class: same as the mapper
    -   Number of reducers
## Mapper
The Mapper class extends the `org.apache.hadoop.mapreduce.Mapper` class.
For this class we implements the **map()** method:

-   That is automatically called by the framework for each (key, value) pair of the input file
-   Processes its input (key, value) pairs by using standard Java code
-   Emits (key, value) pairs by using the `context.write(key, value)` method
## Reducer
The Reducer class extends the `org.apache.hadoop.mapreduce.Reducer`
class. For this class we implements the **reduce()** method:

-   That is automatically called by the framework for each (key, \[list of values\]) pair obtained by aggregating the output of the mapper(s)

-   Processes its input (key, \[list of values\]) pairs by using standard Java code

-   Emits (key, value) pairs by using the ``context.write(key, value) ``method

## Data Types

Hadoop has its own basic data types optimized for network serialization.
From the package `org.apache.hadoop.io`:

-   **Text**, like Java String

-   **IntWritable**, like Java Integer

-   **LongWritable**, like Java Long

-   **FloatWritable**, like Java Float

-   And so on\...

The basic Hadoop data types implement the `org.apache.hadoop.io.Writable` and
`org.apache.hadoop.io.WritableComparable` interfaces.

We can define new data types by implementing these interfaces.

## InputFormat

The input of the MapReduce program is an HDFS file or an HDFS folder, but the input of the Mapper is a set of (key, values), so classe extending the `org.apache.hadoop.mapreduce.InputFormat` abstract class are used to read the input data and **"logically transform"** the input HDFS file in a set of (key, value) pairs.
The InputFormat class is used to:

-   Read input data and validate the compliance of the input file with
    the expected input-format

-   Split the input files into logical **Input Splits**

-   Provide the **RecordReader** implementation to be used to divide the
    logical input split in a set of (key,value) pairs (called records)
    for the mapper

![[02 - Hadoop and MapReduce-1778184500770.webp]]

A set of predefined classes extending the InputFormat abstract class are
available for standard input file formats:

-   **TextInputFormat**

-   **KeyValueTextInputFormat**

-   **SequenceFileInputFormat**

-   And so on\...

### TextInputFormat
The TextInputFormat is an InputFormat for plain text files, it works as it follows:

-   Files are broken into lines

-   One pair (key, value) is emitted for each line of the file:

    -   **Key** is the position (offset) of the line in the file

    -   **Value** is the content of the line

![[02 - Hadoop and MapReduce-1778184519030.webp|533]]

### KeyValueTextInputFormat
The KeyValueTextInputFormat is also an InputFormat for plain text files, but each line of the file must have this format **key \<separator\> value**, where the default separator is the TAB:

-   Files are broken into lines
-   One pair (key, value) is emitted for each line of the file:
    -   **Key** is the text preceding the separator
    -   **Value** is the text following the separator

![[02 - Hadoop and MapReduce-1778184551550.webp|617]]
## OutputFormat

The classes extending the `org.apache.hadoop.mapreduce.OutputForm` at abstract class are used to write the output of the MapReduce program in HDFS.

A set of predefined classes extending the OutputFormat abstract class
are available for standard output file formats:

-   TextOutputFormat

-   SequenceFileOutputFormat

-   And so on\...

### TextOutputFormat

The TextOutputFormat is an OutputFormat for plain text files, for each output (key, value) pair it writes one line in the output file in this format:

**key[\\t]{style="color: cyan"}value[\\n]{style="color: cyan"}**


# Combiner

As we already said, the output of the Mappers has to be sent to the Reducers via network, this could be very heavy. In order to reduce the amount of data in the network some **pre-aggregations** could be done using Combiners.

## Properties

It is important to say that the combiner **works only if the reduce function is commutative and associative**. Hadoop decides runtime if execute it or not, it is not configurable in the code. Because of this, it is important that the **MapReduce job doesn't depend on the Combiner
execution**.
## MapReduce programs - Combiner
The Combiner class extends the `org.apache.hadoop.mapreduce.Reducer` class. For this class we implements the **reduce()** method, it is automatically called by Hadoop for each (key, \[list of values\]) pair obtained by aggregating the local output of a Mapper.

The Combiner class is specified by using the `job.setCombinerClass()`
method in the **run() method of the Driver**.

## Combiner in the Word Count example
We suppose to have two mapper and one reduce. 
![[02 - Hadoop and MapReduce-1778184709452.webp|580]]

The combiner is called **locally** on the output of the Mapper.

The parameters of the applications are:

-   **args\[0\]**: number of instances of the reducer
-   **args\[1\]**: path of the input file
-   **args\[2\]**: path of the output folder

# Full Word Count Program
## Driver -- `WordCount.java`

```java
package it.polito.bigdata.hadoop.wordcount;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

public class WordCount extends Configured implements Tool {

    @Override
    public int run(String[] args) throws Exception {
        Path inputPath;
        Path outputDir;
        int numberOfReducers;
        int exitCode;

        // Parse input parameters
        numberOfReducers = Integer.parseInt(args[0]);
        inputPath        = new Path(args[1]);
        outputDir        = new Path(args[2]);

        // Define and configure a new job
        Configuration conf = this.getConf();
        Job job = Job.getInstance(conf);

        // Assign a name to the job
        job.setJobName("WordCounter");

        // Set path of the input file/folder
        FileInputFormat.addInputPath(job, inputPath);

        // Set path of the output folder
        FileOutputFormat.setOutputPath(job, outputDir);

        // Set input format (TextInputFormat = textual files)
        job.setInputFormatClass(TextInputFormat.class);

        // Set job output format
        job.setOutputFormatClass(TextOutputFormat.class);

        // Specify the Driver class for this job
        job.setJarByClass(WordCount.class);

        // Set mapper class
        job.setMapperClass(WordCountMapper.class);

        // Set map output key and value classes
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        // Set reducer class
        job.setReducerClass(WordCountReducer.class);

        // Set reduce output key and value classes
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        // Set number of reducers
        job.setNumReduceTasks(numberOfReducers);

        // Set combiner class
        job.setCombinerClass(WordCountCombiner.class);

        // Execute the job and wait for completion
        if (job.waitForCompletion(true) == true)
            exitCode = 0;
        else
            exitCode = 1;

        return exitCode;
    } // End of the run method

    /* Main method of the driver class */
    public static void main(String args[]) throws Exception {
        int res = ToolRunner.run(new Configuration(),
                                 new WordCount(), args);
        System.exit(res);
    } // End of the main method

} // End of public class WordCount
```

## Mapper -- `WordCountMapper.java`

```java
package it.polito.bigdata.hadoop.wordcount;

import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

class WordCountMapper extends Mapper<
        LongWritable,   // Input key type
        Text,           // Input value type
        Text,           // Output key type
        IntWritable>    // Output value type
{
    /* Implementation of the map method */
    protected void map(
            LongWritable key,   // Input key type
            Text value,         // Input value type
            Context context) throws IOException, InterruptedException {

        // Split each sentence into words (whitespace as delimiter)
        String[] words = value.toString().split("\\s+");

        // Iterate over the set of words
        for (String word : words) {
            // Transform word to lowercase
            String cleanedWord = word.toLowerCase();

            // Emit one pair (word, 1) for each input word
            context.write(new Text(cleanedWord), new IntWritable(1));
        }
    } // End map method

} // End of class WordCountMapper
```

## Combiner -- `WordCountCombiner.java`

```java
package it.polito.bigdata.hadoop.wordcount;

import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

class WordCountCombiner extends Reducer<
        Text,           // Input key type
        IntWritable,    // Input value type
        Text,           // Output key type
        IntWritable>    // Output value type
{
    /* Implementation of the reduce method */
    protected void reduce(
            Text key,                       // Input key type
            Iterable<IntWritable> values,   // Input value type
            Context context) throws IOException, InterruptedException {

        int occurrences = 0;

        // Iterate over the set of values and sum them
        for (IntWritable value : values) {
            occurrences = occurrences + value.get();
        }

        // Emit the total number of occurrences of the current word
        context.write(key, new IntWritable(occurrences));
    } // End reduce method

} // End of class WordCountCombiner
```

## Reducer -- `WordCountReducer.java`

``` java
package it.polito.bigdata.hadoop.wordcount;

import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

class WordCountReducer extends Reducer<
        Text,           // Input key type
        IntWritable,    // Input value type
        Text,           // Output key type
        IntWritable>    // Output value type
{
    /* Implementation of the reduce method */
    protected void reduce(
            Text key,                       // Input key type
            Iterable<IntWritable> values,   // Input value type
            Context context) throws IOException, InterruptedException {

        int occurrences = 0;

        // Iterate over the set of values and sum them
        for (IntWritable value : values) {
            occurrences = occurrences + value.get();
        }

        // Emit the total number of occurrences of the current word
        context.write(key, new IntWritable(occurrences));
    } // End reduce method

} // End of class WordCountReducer
```

**N.B.:** the Reducer and the Combiner classes perform the same computation because the reduce() is the same. So, we do not need two different classes and we can specify that WordCounterReducer is also de combiner: `job.setCombinerClass(WordCountReducer.class)`. This is a pretty popular strategy.
# Personalized Data Types
Personalized Data Types are useful when the value of a key-value pair is a **complex data type**, as already said, they have to implements the `org.apache.hadoop.io.Writable` interface and the following methods:

-   `public void readFields(DataInput in)`
-   `public void write(DataOutput out)`

In order to properly format the output of the job usually also the `public String toString()` method is "redefined".

## Personalized Data Types - Example
Suppose to be interested in "complex" values composed of two parts:

-   `counter (int)`

-   `sum (float)`

```java
package it.polito.bigdata.hadoop.combinerexample;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

public class SumAndCountWritable implements
        org.apache.hadoop.io.Writable {

    /* Private variables */
    private float sum   = 0;
    private int   count = 0;

    /* Methods to get and set private variables of the class */
    public float getSum() {
        return sum;
    }

    public void setSum(float sumValue) {
        sum = sumValue;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int countValue) {
        count = countValue;
    }

    /* Methods to serialize and deserialize the contents of the
       instances of this class */

    @Override /* Serialize the fields of this object to out */
    public void write(DataOutput out) throws IOException {
        out.writeFloat(sum);
        out.writeInt(count);
    }

    @Override /* Deserialize the fields of this object from in */
    public void readFields(DataInput in) throws IOException {
        sum   = in.readFloat();
        count = in.readInt();
    }

    /* Specify how to convert the contents of the instances of this
       class to a String.
       Useful to specify how to store/write the content of this class
       in a textual file. */
    public String toString() {
        String formattedString =
                new String("sum=" + sum + ",count=" + count);
        return formattedString;
    }

} // End of class SumAndCountWritable
```

## Manage Complex Keys

Personalized Data Types can be used also to manage complex keys. In this is case the DataType has to implements the `org.apache.hadoop.io. WritableComparable` interface and implements:

-   `compareTo()`: because the keys must be compared or sorted
-   `hashCode()`: because they have to be splitted in groups
## Sharing parameters among Driver, Mappers and Reducers
The **configuration object** is used to share the configuration of the Hadoop environment across the driver, the mappers and the reducers of the application/job:

-   It stores a list of **(property-name, property-value)** pairs
-   These pairs are **specified in the Driver**
-   They are used for sharing some parameters with Mappers and Reducers

They are very useful for sharing **constant properties** that are only available during the execution of the program:

-   The Driver set them
-   The Mappers and Reducers can read but not modify them

### In The Program

In the Driver we have to do these things:
1.  Retrieve the configuration object: `Configuration conf = this.getConf();`
2.  Set personalized properties: `conf.set("property-name", "value");`
3.  Then in the Mapper and the Reducer: `context.getConfiguration().get("property-name")`

# Counters

Hadoop provides a set of basic, built-in, counters to store some **statistics** about jobs, mappers, reducers. Also, we can define personalized counters to compute the statistics that we want.

## User-defined Counters
They are defined using Java **Enum**, they are incremented in the Mappers and in the Reducers. The value is printed by the Driver at the end of the job.

For increment the counter we do: `context.getCounter(countername).increment(value);`. In the Driver we use the methods `getCounters()` and `findCounter()` methods for retrieve the final values of the Counters.

User-defined counters can be defined runtime by using the method `incrCounter(<group name>, <counter name>, <value>)`.

## Example of User-defined Counters
Here's an example of how implement them:
-   In the Driver we define it
```java
public static enum COUNTERS {
    ERROR_COUNT,
    MISSING_FIELDS_RECORD_COUNT
}
```

-   For increment it in the Mapper or in the Reducer

```java
context.getCounter(COUNTERS.ERROR_COUNT).increment(1);
```

-   For retrieving the final value at the end of the execution of the
    application

```java
Counter errorCounter = job.getCounters().findCounter(COUNTERS.ERROR_COUNT);
```

# Map-only Job

In some applications all the work can be performed by the **Mappers**. Hadoop allows the execution of only the Map jobs:

-   The Reduce and the shuffle and sort phases are not performed

-   The output of the Mappers is directly stored in HDFS

In order to implement a Map-only job we will have to set the number of
reducers to zero:

```java
job.setNumReduceTasks(0);
```

# Setup and Cleanup

Mappers classes are characterized by a setup and a cleanup methods. They are empty if not overridden.
-   **Setup:**
    -   Called once for each mapper **before** the map() method
    -   Used for setup the attributes of the Mapper instance in order to preserve his state
    -   **N.B.:** the map() method is stateless
-   **Cleanup:**
    -   Called once for each mapper **after** the map() method
    -   It can be used to emit (key, value) pairs based on the values of the attributes of the instance
Also the Reducer class is characterized by a setup and a cleanup method.
## In-Mappers Combiners
The in-mapper variables are used to perform the work of the combiner in the mapper, it can allow improving the overall performance of the application but it uses a lot of main memory.
![[02 - Hadoop and MapReduce-1778185002904.webp|556]]
