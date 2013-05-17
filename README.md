mvn-mapr-example
================

Sample mavenized Hadoop Job project for MapR 2.1.2+. It demonstrates how to work with a MapR cluster on a dev machine 
without installing the mapr-core package.

Invoke maven to install the job jar in your local maven repo, it pulls all neccessary MapR bits as well: 
```bash
bash$ mvn install
```

Edit conf/mapr-clusters.conf to reflect your CLDB nodes location. Set MAPR_HOME to the repo top directory:
```bash
bash$ export MAPR_HOME=$(git rev-parse --show-toplevel)
```
Check access to filesystem equivalent to `hadoop fs -ls`:
```bash
bash$ mvn exec:java -Dexec.mainClass=org.apache.hadoop.fs.FsShell -Dexec.args="-ls /"
[INFO] Scanning for projects...
...
Found 3 items
drwxr-xr-x   - root wheel          0 2013-05-13 00:40 /hbase
drwxr-xr-x   - root wheel          2 2013-05-15 01:49 /user
drwxr-xr-x   - root wheel          1 2013-05-13 00:41 /var
```
Copy some file to your MapR-FS home directory:
```bash
bash$ mvn exec:java -Dexec.mainClass="org.apache.hadoop.fs.FsShell" \
                         -Dexec.args="-put pom.xml /user/${USER}/"
```
You can check access to JobTracker equivalent to `hadoop job -list-active-trackers`:
```bash
bash$ mvn exec:java -Dexec.mainClass="org.apache.hadoop.mapred.JobClient" \
                         -Dexec.args="-list-active-trackers"
```
Finally, submit your project's MapReduce job using maven that may include generic options:
```bash
bash$ mvn exec:java -Dexec.mainClass="org.apache.hadoop.examples.WordCount" \
                         -Dexec.args="-Dio.sort.mb=50 pom.xml wcout"
[INFO] Scanning for projects...
...
13/05/16 23:33:38 INFO input.FileInputFormat: Total input paths to process : 1
13/05/16 23:33:38 WARN snappy.LoadSnappy: Snappy native library not loaded
13/05/16 23:33:38 INFO mapred.JobClient: Creating job's output directory at wcout
13/05/16 23:33:38 INFO mapred.JobClient: Creating job's user history location directory at wcout/_logs
13/05/16 23:33:38 INFO mapred.JobClient: Running job: job_201305152312_0014
13/05/16 23:33:39 INFO mapred.JobClient:  map 0% reduce 0%
13/05/16 23:33:45 INFO mapred.JobClient:  map 100% reduce 0%
13/05/16 23:33:49 INFO mapred.JobClient:  map 100% reduce 50%
13/05/16 23:33:53 INFO mapred.JobClient:  map 100% reduce 100%
13/05/16 23:33:53 INFO mapred.JobClient: Job job_201305152312_0014 completed successfully
13/05/16 23:33:53 INFO mapred.JobClient: Counters: 25
13/05/16 23:33:53 INFO mapred.JobClient:   Job Counters 
13/05/16 23:33:53 INFO mapred.JobClient:     Launched reduce tasks=2
13/05/16 23:33:53 INFO mapred.JobClient:     Aggregate execution time of mappers(ms)=4621
13/05/16 23:33:53 INFO mapred.JobClient:     Total time spent by all reduces waiting after reserving slots (ms)=0
13/05/16 23:33:53 INFO mapred.JobClient:     Total time spent by all maps waiting after reserving slots (ms)=0
13/05/16 23:33:53 INFO mapred.JobClient:     Launched map tasks=1
13/05/16 23:33:53 INFO mapred.JobClient:     Data-local map tasks=1
13/05/16 23:33:53 INFO mapred.JobClient:     Aggregate execution time of reducers(ms)=6414
13/05/16 23:33:53 INFO mapred.JobClient:   FileSystemCounters
13/05/16 23:33:53 INFO mapred.JobClient:     MAPRFS_BYTES_READ=4392
13/05/16 23:33:53 INFO mapred.JobClient:     MAPRFS_BYTES_WRITTEN=3803
13/05/16 23:33:53 INFO mapred.JobClient:     FILE_BYTES_WRITTEN=19916
13/05/16 23:33:53 INFO mapred.JobClient:   Map-Reduce Framework
13/05/16 23:33:53 INFO mapred.JobClient:     Map input records=53
13/05/16 23:33:53 INFO mapred.JobClient:     Reduce shuffle bytes=1305
13/05/16 23:33:53 INFO mapred.JobClient:     Spilled Records=82
13/05/16 23:33:53 INFO mapred.JobClient:     Map output bytes=1530
13/05/16 23:33:53 INFO mapred.JobClient:     CPU_MILLISECONDS=470
13/05/16 23:33:53 INFO mapred.JobClient:     Combine input records=53
13/05/16 23:33:53 INFO mapred.JobClient:     SPLIT_RAW_BYTES=90
13/05/16 23:33:53 INFO mapred.JobClient:     Reduce input records=41
13/05/16 23:33:53 INFO mapred.JobClient:     Reduce input groups=41
13/05/16 23:33:53 INFO mapred.JobClient:     Combine output records=41
13/05/16 23:33:53 INFO mapred.JobClient:     PHYSICAL_MEMORY_BYTES=264675328
13/05/16 23:33:53 INFO mapred.JobClient:     Reduce output records=41
13/05/16 23:33:53 INFO mapred.JobClient:     VIRTUAL_MEMORY_BYTES=2229972992
13/05/16 23:33:53 INFO mapred.JobClient:     Map output records=53
13/05/16 23:33:53 INFO mapred.JobClient:     GC time elapsed (ms)=50
```


