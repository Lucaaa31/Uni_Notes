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




---
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

![[02 - Hadoop and MapReduce-1778184709452.webp|580]]
The combiner is called **locally** on the output of the Mapper.
The parameters of the applications are:
-   **args\[0\]**: number of instances of the reducer
-   **args\[1\]**: path of the input file
-   **args\[2\]**: path of the output folder
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