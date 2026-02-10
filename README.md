
# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset. In this project we provide a input file and then we create a maaper and reducer logic to count the occurence of each word in the given input. There are sample input and Expected output for the sample input.

## Approach and implementation
1. Mapper Logic: We use StringTokenizer to create tokens from the input file and loop it using while loop to map all the words in the input file with key value pairs. In this mapper, it will not count characters that are smaller than 3.

2. Reducer Logic: Using the output of Mapper logic we increase a variable sum value as we encounter same words and retun them. this way we will get a list of words and the number of times it occured in the input file as output.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/data
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/data
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
```
# Output:


### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output


## Map Reduce Job Output:
```bash
root@72810288bb00:/opt/hadoop-3.2.1/share/hadoop/mapreduce# hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
2026-02-10 17:18:26,202 INFO client.RMProxy: Connecting to ResourceManager at resourcemanager/172.21.0.10:8032
2026-02-10 17:18:26,503 INFO client.AHSProxy: Connecting to Application History server at historyserver/172.21.0.5:10200
2026-02-10 17:18:26,945 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2026-02-10 17:18:27,024 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/root/.staging/job_1770743397586_0001
2026-02-10 17:18:27,296 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
2026-02-10 17:18:27,692 INFO input.FileInputFormat: Total input files to process : 1
2026-02-10 17:18:27,772 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
2026-02-10 17:18:27,833 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
2026-02-10 17:18:27,903 INFO mapreduce.JobSubmitter: number of splits:1
2026-02-10 17:18:28,153 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
2026-02-10 17:18:28,296 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1770743397586_0001
2026-02-10 17:18:28,297 INFO mapreduce.JobSubmitter: Executing with tokens: []
2026-02-10 17:18:28,493 INFO conf.Configuration: resource-types.xml not found
2026-02-10 17:18:28,493 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2026-02-10 17:18:30,149 INFO impl.YarnClientImpl: Submitted application application_1770743397586_0001
2026-02-10 17:18:30,295 INFO mapreduce.Job: The url to track the job: http://resourcemanager:8088/proxy/application_1770743397586_0001/
2026-02-10 17:18:30,301 INFO mapreduce.Job: Running job: job_1770743397586_0001
2026-02-10 17:18:45,224 INFO mapreduce.Job: Job job_1770743397586_0001 running in uber mode : false
2026-02-10 17:18:45,233 INFO mapreduce.Job:  map 0% reduce 0%
2026-02-10 17:18:53,489 INFO mapreduce.Job:  map 100% reduce 0%
2026-02-10 17:18:59,576 INFO mapreduce.Job:  map 100% reduce 100%
2026-02-10 17:18:59,668 INFO mapreduce.Job: Job job_1770743397586_0001 completed successfully
2026-02-10 17:18:59,887 INFO mapreduce.Job: Counters: 54
        File System Counters
                FILE: Number of bytes read=72
                FILE: Number of bytes written=458187
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=138
                HDFS: Number of bytes written=40
                HDFS: Number of read operations=8
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
                HDFS: Number of bytes read erasure-coded=0
        Job Counters 
                Launched map tasks=1
                Launched reduce tasks=1
                Rack-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=20188
                Total time spent by all reduces in occupied slots (ms)=24208
                Total time spent by all map tasks (ms)=5047
                Total time spent by all reduce tasks (ms)=3026
                Total vcore-milliseconds taken by all map tasks=5047
                Total vcore-milliseconds taken by all reduce tasks=3026
                Total megabyte-milliseconds taken by all map tasks=20672512
                Total megabyte-milliseconds taken by all reduce tasks=24788992
        Map-Reduce Framework
                Map input records=1
                Map output records=5
                Map output bytes=50
                Map output materialized bytes=64
                Input split bytes=106
                Combine input records=5
                Combine output records=5
                Reduce input groups=5
                Reduce shuffle bytes=64
                Reduce input records=5
                Reduce output records=5
                Spilled Records=10
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=125
                CPU time spent (ms)=860
                Physical memory (bytes) snapshot=645627904
                Virtual memory (bytes) snapshot=14777831424
                Total committed heap usage (bytes)=476577792
                Peak Map Physical memory (bytes)=365776896
                Peak Map Virtual memory (bytes)=5715021824
                Peak Reduce Physical memory (bytes)=279851008
                Peak Reduce Virtual memory (bytes)=9062809600
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters 
                Bytes Read=32
        File Output Format Counters 
                Bytes Written=40
root@72810288bb00:/opt/hadoop-3.2.1/share/hadoop/mapreduce# hadoop fs -cat /output1/*
2026-02-10 17:19:22,701 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
dataset 1
your    1
own     1
input   1
Create  1
root@72810288bb00:/opt/hadoop-3.2.1/share/hadoop/mapreduce# hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
2026-02-10 17:19:34,169 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
root@72810288bb00:/opt/hadoop-3.2.1/share/hadoop/mapreduce# exit 
exit
```
## Sample Input: 
 ```bash
   Create your own input dataset.
   ```

## Expected output: 
 ```bash
dataset 1
your    1
own     1
input   1
Create  1
   ```
