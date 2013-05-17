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
bash$ mvn exec:java -Dexec.mainClass=org.apache.hadoop.fs.FsShell -Dexec.args="-put pom.xml /user/${USER}/"
```
You can check access to JobTracker equivalent to `hadoop job -list-active-trackers`:
```bash
bash$ mvn exec:java -Dexec.mainClass="org.apache.hadoop.mapred.JobClient" -Dexec.args="-list-active-trackers"
```
Finally, submit your project's MapReduce job using maven that may include generic options:
```bash
bash$ mvn exec:java -Dexec.mainClass=org.apache.hadoop.examples.WordCount -Dexec.args="-Dio.sort.mb=50 pom.xml wcout"
```


