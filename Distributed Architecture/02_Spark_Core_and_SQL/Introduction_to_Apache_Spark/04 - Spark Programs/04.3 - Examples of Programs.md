## Count line
Count the number of lines of the input file, then it prints the results on the standard output:
- The name of the file is set to “myfile.txt”
```Python
from pyspark import SparkConf, SparkContext if __name__ == "__main__": 
# Create a configuration object and 
# set the name of the application 

conf = SparkConf().setAppName("Spark Line Count") 
# Create a Spark Context object 
sc = SparkContext(conf=conf) 

# Store the path of the input file in inputfile
inputFile= "myfile.txt"

# Build an RDD of Strings from the input textual file
# Each element of the RDD is a line of the input file
linesRDD = sc.textFile(inputFile)

# Count the number of lines in the input file
# Store the returned value in the local variable numLines
numLines = linesRDD.count()

# Print the output in the standard output
print("NumLines:", numLines)

# Close the Spark Context object
sc.stop()
```
Some note:
- `conf`, `sc`, `inputFile` and `numLines` are local Python variables that are allocated in the main memory of the same process instancing the Driver
	- Can be used to store only small objects/data
	- Maximum size = main memory
- `linesRDD` is a RDDs
	- It is used to store large collections of objects/data in the nodes of the cluster
		- In the main memory of the worker nodes, when it is possible
		- In the local disks of the worker nodes, when it is necessary
## Word Count
Word Count implemented by means of Spark:
- The name of the **input** file is specified by using a command line parameter (i.e., argv\[1\]) 
- The **output** of the application (i.e., the pairs (word, num. of occurrences) is stored in an output folder (i.e., argv\[2\])
```Python
from pyspark import SparkConf, SparkContext
import sys


if __name__ == "__main__":
 """
 Word count example
 """
 inputFile= sys.argv[1]
 outputPath = sys.argv[2]
 
 #Create a configuration object and
 #set the name of the application
 conf = SparkConf().setAppName("Spark Word Count")
 
 # Create a Spark Context object
 sc = SparkContext(conf=conf)
 
 # Build an RDD of Strings from the input textual file
 # Each element of the RDD is a line of the input file
 lines = sc.textFile(inputFile)
 
 # Split/transform the content of lines in a
 # list of words and store them in the words RDD
 words = lines.flatMap(lambda line: line.split(sep=' '))
 
 #Map/transform each word in the words RDD
 #to a pair/tuple (word,1) and store the result in the words_one RDD
 words_one = words.map(lambda word: (word, 1))
 
 # Count the num. of occurrences of each word.
 # Reduce by key the pairs of the words_one RDD and store
 # the result (the list of pairs (word, num. of occurrences)
 # in the counts RDD
 counts = words_one.reduceByKey(lambda c1, c2: c1 + c2)
 
 # Store the result in the output folder
 counts.saveAsTextFile(outputPath)
 
 # Close/Stop the Spark Context object
 sc.stop()

```