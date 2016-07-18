
~~~
JAR='/usdfdsfdssr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar'

# 1TB
SIZE=10000000000

# -Dmapred.map.tasks=160
# -Dmapred.reduce.tasks=24

# Pre
kdestroy
kinit ambari-qa -kt /etc/security/keytabs/smokeuser.headless.keytab 
klist

# TeraGen
echo '#######################################################'
echo 'teragen'
echo '#######################################################'
hadoop dfs -rm -r -skipTrash /tmp/terasort-input-${SIZE}
time hadoop jar ${JAR} teragen -Dmapred.map.tasks=160 ${SIZE} /tmp/terasort-input-${SIZE}

# TeraSort
echo '#######################################################'
echo 'terasort'
echo '#######################################################'
hadoop dfs -rm -r -skipTrash /tmp/terasort-output-${SIZE}
time hadoop jar ${JAR} terasort -Dmapred.reduce.tasks=24 /tmp/terasort-input-${SIZE} /tmp/terasort-output-${SIZE}
~~~
