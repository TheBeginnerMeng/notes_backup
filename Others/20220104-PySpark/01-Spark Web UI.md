# Spark Web UI - Understanding Spark Execution

Apache Spark provides a suite of  Web UI/User Interfaces (Jobs, Stages, Tasks, Storage, Environment, Executors and SQL) to monitor Spark/PySpark application, resource consumption of Spark cluster, and Spark configuration.

Two concepts: Transformations  Actions

**Your Spark application code**: A  set of instructions that instructs the driver to do a Spark job, and let the driver decide how to achieve it with the help of executors.

**Transformations**: Instructions to the driver.

**Actions**: trigger the execution.

Code Example:

![Spark Job Sample application](https://i0.wp.com/sparkbyexamples.com/wp-content/uploads/2020/07/1.-Transformation-and-Action.jpg?resize=1024%2C251&ssl=1)

Spark UI is separated into below tabs:

1. Spark Jobs
2. Stages
3. Tasks
4. Storage
5. Environment
6. Executors
7. SQL

When running the Spark application locally, Spark UI can be accessed using the http://localhost:4040/.

# 1. Spark Jobs Tab

![Spark Job UI](https://i2.wp.com/sparkbyexamples.com/wp-content/uploads/2020/07/4.Scheduling.jpg?resize=413%2C236&ssl=1)

​																					Jobs tab

## 1.1 Scheduling Mode

We have three Scheduling modes:

1. Standalone mode (running in a local machine)
2. YARN mode
3. Mesos

![Spark Job Scheduling UI](https://i0.wp.com/sparkbyexamples.com/wp-content/uploads/2020/07/5.NumberofJobs.jpg?resize=512%2C96&ssl=1)

​																	Spark Scheduling tab

## 1.2 Number of Spark Jobs

==keep in mind==: the number of Spark jobs = the number of actions in the application, each Spark job should have at least one Stage.

In our above application, we have performed 3 Spark jobs(0,1,2)

- Job 0: read the csv file.
- Job 1: Inferschema from the file.
- Job 2: Count Check.

## 1.3 Number of Stages

Each **Wide Transformation** results in a separate Number of Stages.

Spark job0 and Spark job1: have individual single stages 

Spark job2: we can see two stages that are because of the partition of data. Data is partitioned into two files by default.

## 1.4 Description

Description links the complete details of the associated SparkJob like Spark Job Status, DAG Visualization, Completed Stages.

# 2. Stages Tab

The stage tab displays a summary page that shows the current state of all stages of all Spark jobs in the spark application.

The number of tasks you could see in each stage is the number of partitions that spark is going to work on and each task inside a stage is the same work that will be done by spark but on a different partition of data.

![Stage 0](https://i1.wp.com/sparkbyexamples.com/wp-content/uploads/2020/07/7.Stage0_.jpg?resize=310%2C512&ssl=1)

​                                                                               Stage 0

## Stage detail

Details of stage showcase  Directed Acyclic Graph (DAG) of this stage.

vertices: represent the RDDs or DataFrame

edges: represent an operation to be applied

# 3. Tasks

Key things to look task page are:

1. Input Size - Input for the Stage
2. Shuffle Write - Output is the stage written.

# 4. Storage

The Storage tab displays the persisted RDDs and DataFrames. The summary pages shows the storage levels, sizes and partitions of all RDDs, and the detail page shows the sizes and using executors for all partitions in an RDD or DataFrame.

# 5. Environment Tab

This environment page has five parts. It is a useful place to check whether your properties have been set correctly.

1. **Runtime Information**: simply contains the runtime properties like versions of Java and Scala.
2. **Spark Properties**: lists the application properties like ‘spark.app.name’ and ‘`spark.driver.memory`’.
3. **Hadoop Properties**: displays properties relative to Hadoop and YARN. **Note**: Properties like [‘](https://spark.apache.org/docs/3.0.0-preview/configuration.html#execution-behavior)`spark.hadoop`’ are shown not in this part but in ‘Spark Properties’.
4. **System Properties**: shows more details about the JVM.
5. **Classpath Entries**: lists the classes loaded from different sources, which is very useful to resolve class conflicts.

# 6. Executors Tab

The Executors tab displays summary information about the executors that were created for the application, including memory and disk usage and task and shuffle information. The Storage Memory column shows the amount of memory used and reserved for caching data.

# 7. SQL Tab

If the application executes Spark SQL queries then the SQL tab displays information, such as the duration, Spark jobs, and physical and logical plans for the queries.