**bio-pipelines**
==================

<em> Sequenced samples of genetic material are usually subjected to bioinformatics analysis performed through a processing pipeline in which the reads are repeatedly transformed in individual stages using various specialized tools. The purpose of this process is to obtain a record of the identified genetic variants. Since many independent components are involved, the development of automation solutions is one of the main challenges of DNA analysis. In addition, the complete workflow might take even a few days using classical methods, thus computer clusters with high computing power are more often used to improve the efficiency of time-consuming procedures. This thesis presents a data pipeline built upon the Apache Airflow platform for the analysis of germline variants. The most computationally intensive stages are executed in a distributed environment to reduce the computation time of the process. The second area of the project concerns components integrated with the mentioned system, designed specifically to analyze sequence data and work with general-purpose applications based on the Apache Spark technology. This thesis explains the fundamental issues of modern genomics and bioinformatics. It describes each stage of the standard pipeline for variant detection and reviews published workflow management systems. It defines the technologies constituting the basis of the project and surveys three platforms: Snakemake, Cromwell, and Apache Airflow. Furthermore, it outlines the most crucial decisions concerning the final solution and presents its main features. The reader is introduced to the nomenclature of the Apache Airflow management system, along with details of the implementation of the thesis project and the applied software testing techniques. </em>


![image](https://user-images.githubusercontent.com/52524599/149842344-0d86f94c-5842-4ebe-93b0-409524fbb80d.png)


**How to run unit tests?**
============================

Tests are stored in the `tests` directory. 
 - directories `tests/resources/` expose testing files including FASTQ, SAM, VCF formats 
 - files `tests/test_*` are scripts that tests this project
  
 Procedure should be performed within the same terminal session. 
### 1. Set the environment variable
Open the terminal and run:
```bash 
export AIRFLOW__CORE__UNIT_TEST_MODE=true
```
This change the Airflow mode to UnitTest mode.

You should provide an absolute path to the Seqtender JAR file and set it as an environment variable `SEQTENDER_JAR` .

Tests performed for the VEP operators are perfomed using 98 version. You should provide an absolute path to the VEP cache as the the environment variable `VEP_CACHE_98`.

By default tests resolve an absolute path to the Cannoli JAR file (stored in the `tools/target/scala-2.12/BioPipeline-assembly-0.2-SNAPSHOT.jar` directory), but it can be set as an environement variable`CANNOLI_JAR`. 

### 2.Prepare unittests.db
`Unittests.db` is a database automatically generated by Airflow. You should update this database with our testing settings that are stored in `tests/resources/configtest.json`
Run:

``` bash
airflow variables -i tests/resources/configtest.json
```
---
**NOTE**

It may happen that some of the dependencies for BioPipeline are already configured in this database which may impact on the tests results. It is not recommended to insert any values manually.  

---

### 3.Run unit tests

Run: 
``` bash
 python3 -m unittest test_spark_config_operator test_cannoli_operators test_static test_dynamic test_seqtender_operators
```

