# CSL7110 – Machine Learning with Big Data  
## Assignment: Hadoop MapReduce & Apache Spark

**Student:** Shashi Saurav (M24AID048) 
**Course:** CSL7110 – Machine Learning with Big Data  
**Institution:** IIT Jodhpur  
**Environment:** Windows 11 + WSL (Ubuntu)  
**Frameworks:** Apache Hadoop, Apache Spark  


---

## Overview

This repository contains the implementation, execution, and analysis of Hadoop MapReduce
and Apache Spark tasks as part of the CSL7110 assignment.

The assignment is divided into three main parts:

1. Hadoop installation and WordCount (Questions 1–6)
2. WordCount on large dataset and HDFS operations (Questions 7–9)
3. Apache Spark installation and Author Influence Network analysis (Questions 10–12)

All programs were executed on a **local single-node Hadoop and Spark setup using WSL**.
Screenshots of executions are included in the submitted PDF as required.

---

## Environment Setup

- **OS:** Windows 11
- **Linux Layer:** WSL (Ubuntu)
- **Java:** OpenJDK 11
- **Hadoop Version:** Hadoop 3.x (single-node mode)
- **Spark Version:** Spark 3.x (local mode)
- **HDFS URL:** `hdfs://localhost:9000`

---

## Question 1 – Hadoop WordCount Example

- Successfully ran the **WordCount example** provided in the Apache Hadoop documentation.
- Verified correct execution by:
  - Creating input directory in HDFS
  - Running the WordCount JAR
  - Viewing output using `hdfs dfs -cat`

✔ Output confirmed correct word-frequency pairs.

---

## Question 2 – Map Phase Input & Output

- Input to the Map phase:
  - **Key:** Byte offset (`LongWritable`)
  - **Value:** Line of text (`Text`)
- Output from the Map phase:
  - **Key:** Word (`Text`)
  - **Value:** Count (`IntWritable`)

The transformation of lyrics input into `(word, 1)` pairs was analyzed and documented.

---

## Question 3 – Reduce Phase Input & Output

- Input to Reduce phase:
  - **Key:** Word (`Text`)
  - **Value:** Iterable list of counts (`Iterable<IntWritable>`)
- Output from Reduce phase:
  - **Key:** Word (`Text`)
  - **Value:** Total count (`IntWritable`)

Example outputs such as:



("up", 4)

("to", 3)

("get", 2)



were explained.

---

## Question 4 – WordCount.java Type Definitions

- Identified correct Hadoop data types:
  - `LongWritable`
  - `Text`
  - `IntWritable`
- Updated `map()` and `reduce()` method signatures accordingly.
- Set job output key and value classes using:
  - `job.setOutputKeyClass(Text.class)`
  - `job.setOutputValueClass(IntWritable.class)`

---

## Question 5 – Mapper Implementation

- Implemented `map()` function to:
  - Remove punctuation using `replaceAll`
  - Tokenize input lines using `StringTokenizer`
  - Emit `(word, 1)` pairs

Special care was taken to handle alphanumeric tokens correctly.

---

## Question 6 – Reducer Implementation

- Implemented `reduce()` function to:
  - Sum all counts for each word
  - Emit final `(word, total_count)` output
- Code compiled and executed successfully without errors.

---

## Question 7 – WordCount on Large Dataset (200.txt)

- Copied `200.txt` from local filesystem to HDFS:



hdfs dfs -copyFromLocal 200.txt /user/iitj/



- Executed WordCount job on the dataset.
- Merged output files into a single local file using:



hdfs dfs -getmerge output/ output.txt



✔ Output verified to contain correct word frequencies.

---

## Question 8 – HDFS Commands & Replication Factor

- Used HDFS commands such as:
- `hdfs dfs -ls`
- `hdfs dfs -rm`
- `hdfs dfs -copyFromLocal`
- Observed replication factor in file listings.

**Answer:**
Directories do not have a replication factor because **replication applies only to data blocks,
and directories do not store data blocks themselves**.

---

## Question 9 – Measuring Execution Time

- Modified WordCount code to record:
- Job start time
- Job end time
- Printed total execution time in milliseconds.
- Experimented with:
```

mapreduce.input.fileinputformat.split.maxsize

```

**Observation:**
- Smaller splits → more parallelism but higher overhead
- Larger splits → fewer tasks but less parallelism

---

## Question 10 – Apache Spark Installation

Installed Apache Spark on WSL (Ubuntu)
Verified installation using:
```

spark-shell

````
Configured Spark to access HDFS.

---

## Question 11 – Spark Data Loading

- Loaded Gutenberg book files from HDFS using:
```scala
sc.wholeTextFiles("hdfs://localhost:9000/user/iitj/200.txt")
````

* Verified file content using `take(1)`.

---

## Question 12 – Author Influence Network

### Objective

Construct a simplified author influence network based on publication year proximity.

### Methodology

1. Extracted:

   * Author name
   * Release year
2. Defined influence if two authors published within **X = 5 years**
3. Built directed edges `(author1 → author2)`
4. Computed:

   * In-degree
   * Out-degree

### Spark Representation

* Used **RDDs of tuples**
* Advantages:

  * Simple
  * Flexible
* Disadvantages:

  * No schema enforcement
  * Less optimized than DataFrames

### Results

* Dataset contained a **single author**
* Therefore:

  * Influence edges were empty
  * In-degree and out-degree results were empty

This behavior is **correct and expected**.

### Scalability Discussion

* Approach scales linearly with dataset size
* For large datasets:

  * Use DataFrames
  * Avoid Cartesian operations
  * Use broadcast joins
  * Partition data efficiently

---

## Files in This Repository


```

├── WordCount.java
├── wordcount.jar
├── output.txt
├── author_influence.scala
└── README.md
```


---

## Notes

* Screenshots of all executions are included in the assignment PDF.
* GitHub repository contains only source code and output files, as required.

---

## Conclusion

All assignment tasks (Questions 1–12) were successfully implemented, executed, and analyzed
using Hadoop and Spark on a local WSL-based setup.

