 ## Main Components
The programming language used is **Java**. Hadoop MapReduce programs consists in **three phases**:
-   Driver
-   Mapper
-   Reducer
Each parts in implemented using a **Java class**.
### Driver
The Driver is the main class and the entry point of the application. It
is characterized by the **main() method**, that accepts arguments from
command line. Other properties:
-   Configures the job
-   Submits the job to the Hadoop Cluster
-   Coordinates the workflow of the application
-   Runs on the client machine
### Mapper
The Mapper implements the map phase. It is characterized by the **map()
method**. As already seen:
-   Processes the (key, value) pairs of the input file and emits (key, value) pairs
-   Is invoked one time for each input (key, value) pair
It runs on the cluster.
### Reducer
The Reducer implements the reduce phase. It is characterized by the
**reduce() method**. As already seen:
-   Processes (key, \[list of values\]) pairs and emits (key, value) pairs
-   Is invoked one time for each distinct key
It runs on the cluster.
### Hadoop Implementation of the MapReduce phases
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

## Combiner
As we already said, the output of the Mappers has to be sent to the Reducers via network, this could be very heavy. In order to reduce the amount of data in the network some **pre-aggregations** could be done using Combiners.

Combiner **works only if the reduce function is commutative and associative**. Hadoop decides runtime if execute it or not, it is not configurable in the code. Because of this, it is important that the **MapReduce job doesn't depend on the Combiner execution**.

---
## Components in Java

### Driver
The Driver class extends the `org.apache.hadoop.conf.Configured` class and implements the `org.apache.hadoop.util.Tool` interface. For this class we implements **main()** and **run()** methods:
-   The **run()** method configures the job:
    -   Name of the Job
    -   Job Input and Output format
    -   Mapper class: Name, type of input and type of output (key, value) pairs
    -   Reducer class: same as the mapper
    -   Number of reducers
### Mapper
The Mapper class extends the `org.apache.hadoop.mapreduce.Mapper` class.
For this class we implements the **map()** method:

-   That is automatically called by the framework for each (key, value) pair of the input file
-   Processes its input (key, value) pairs by using standard Java code
-   Emits (key, value) pairs by using the `context.write(key, value)` method
### Reducer
The Reducer class extends the `org.apache.hadoop.mapreduce.Reducer`
class. For this class we implements the **reduce()** method:

-   That is automatically called by the framework for each (key, \[list of values\]) pair obtained by aggregating the output of the mapper(s)
-   Processes its input (key, \[list of values\]) pairs by using standard Java code
-   Emits (key, value) pairs by using the ``context.write(key, value) ``method

### Combiner
The Combiner class extends the `org.apache.hadoop.mapreduce.Reducer` class. For this class we implements the **reduce()** method, it is automatically called by Hadoop for each (key, \[list of values\]) pair obtained by aggregating the local output of a Mapper.

The Combiner class is specified by using the `job.setCombinerClass()`
method in the **run() method of the Driver**.

---
## Data Types

Hadoop has its own basic data types optimized for network serialization.
From the package `org.apache.hadoop.io`:

-   **Text** $\rightarrow$ String
-   **IntWritable** $\rightarrow$ Integer
-   **LongWritable** $\rightarrow$ Long
-   **FloatWritable** $\rightarrow$ Float
-   And so on\...
The basic Hadoop data types implement the `org.apache.hadoop.io.Writable` and
`org.apache.hadoop.io.WritableComparable` interfaces.

We can define new data types by implementing these interfaces.

### InputFormat
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

#### TextInputFormat
The TextInputFormat is an InputFormat for plain text files, it works as it follows:
-   Files are broken into lines
-   One pair (key, value) is emitted for each line of the file:
    -   **Key** is the position (offset) of the line in the file
    -   **Value** is the content of the line

![[02 - Hadoop and MapReduce-1778184519030.webp|533]]

#### KeyValueTextInputFormat
The KeyValueTextInputFormat is also an InputFormat for plain text files, but each line of the file must have this format **key \<separator\> value**, where the default separator is the TAB:
-   Files are broken into lines
-   One pair (key, value) is emitted for each line of the file:
    -   **Key** is the text preceding the separator
    -   **Value** is the text following the separator

![[02 - Hadoop and MapReduce-1778184551550.webp|617]]
### OutputFormat
The classes extending the `org.apache.hadoop.mapreduce.OutputForm` at abstract class are used to write the output of the MapReduce program in HDFS.

A set of predefined classes extending the OutputFormat abstract class
are available for standard output file formats:

-   TextOutputFormat

-   SequenceFileOutputFormat

-   And so on\...

#### TextOutputFormat

The TextOutputFormat is an OutputFormat for plain text files, for each output (key, value) pair it writes one line in the output file in this format:

**key[\\t]{style="color: cyan"}value[\\n]{style="color: cyan"}**

---
## Personalized Data Types
Personalized Data Types are useful when the value of a key-value pair is a **complex data type**, as already said, they have to implements the `org.apache.hadoop.io.Writable` interface and the following methods:

-   `public void readFields(DataInput in)`
-   `public void write(DataOutput out)`

In order to properly format the output of the job usually also the `public String toString()` method is "redefined".

### Example
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
### Manage Complex Keys

Personalized Data Types can be used also to manage complex keys. In this is case the DataType has to implements the `org.apache.hadoop.io. WritableComparable` interface and implements:

-   `compareTo()`: because the keys must be compared or sorted
-   `hashCode()`: because they have to be splitted in groups



