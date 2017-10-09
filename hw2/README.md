# [HW2] Apache Beam applications

**Due Oct. 16 (Mon), 6pm**


In this assignment you will
* find at least 2 datasets to analyze (>10MB)
* untar and use bd17f_hw2.tar.gz
* develop 2 new Apache Beam applications
* run the applications using Spark (and Vortex)
* write a report on system performance and correctness
* upload dataset/code/report to the private repository
* be graded based on the code and the report


## Find at least 2 datasets to analyze (>10MB)
* Find public datasets on the internet that you want to analyze
  * e.g., https://github.com/caesar0301/awesome-public-datasets
* You must find and use at least 2 datasets larger than 10MB

## Untar and use bd17f_hw2.tar.gz
* Download link: https://drive.google.com/file/d/0Bxj1zsZ2mX-_ZEJsUWdjUUstTVU/view 
* spark/vortex_mr_run.sh
  * You should be able to run this immediately without any installation on your part
    * We have pre-installed spark/vortex_app and spark/vortex_runtime jars for you
  * Report full error logs in https://github.com/swsnu/bd2017/issues if this doesn’t work for you
  * After running spark_mr_run.sh you should manually exit with Ctrl-C
* spark/vortex_app
  * This is where you develop your Apache Beam applications
  * Due to Maven dependency issues there’s a separate app (Maven project) for each runtime although both run the same Beam pipeline (e.g., MapReduce)
* spark/vortex_runtime
  * Spark 1.6.0 and Vortex 0.1 source code snapshots
* spark/vortex_output
  * When you run spark/vortex_mr_run.sh, check this directory for outputs
* diff_spark_vortex_outputs.sh
  * Diffs the outputs stored in spark/vortex_output 
  * Use this for correctness checking
* mr_input_data
  * Dataset for the sample MapReduce app provided in spark/vortex_app

## Develop 2 new Apache Beam applications
* Learn the Apache Beam programming model in https://beam.apache.org/
* Develop 2 new batch data processing apps using the Apache Beam SDK for analyzing the datasets you found
  * Be creative and do not develop well-known simple apps like WordCount
  * We will check if you’re copying existing Beam apps from the Internet
* We encourage you to use other Java libraries (e.g., machine learning algorithms, graph algorithms, linear solvers) inside Beam DoFns


## Run the applications using Spark and Vortex
* Learn how to run Beam applications using Spark and Vortex
  * Spark
    * https://beam.apache.org/documentation/runners/spark/
    * https://spark.apache.org/docs/latest/submitting-applications.html
  * Vortex
    * vortex_runtime/README.md
* Run the applications you developed with both Spark and Vortex
  * Local runs on a single computer are ok
* Compare running times (performance) and output data (correctness)


## Write a report on system performance and correctness
* Describe datasets, apps, and outputs
* Explain 3 system factors in Spark/Vortex that caused the performance difference/similarity
  * We understand that there are many factors that affect performance and that it is hard to pick 3 specific factors
  * This is more about evaluating your general understanding of the underlying Spark/Vortex runtimes
  * It is ok to make intelligent guesses based on the code and system logs
    * Spark: spark_runtime code, console log, web ui
    * Vortex: vortex_Runtime code, driver.stderr
* Evaluate the correctness of Spark/Vortex
  * If the outputs are the same, simply describe the outputs
  * If the outputs are different, explain which system processed data incorrectly and why
* The report should be in the PDF format no longer than 1 page


## Upload dataset/code/report to the private repository
* Upload to your ‘bd17f-USERNAME’ private repository
* Dataset: appname_input_data
* Code: 
  * Apps in spark/vortex_app
  * spark/vortex_appname_run.sh for running the app
* Report: PDF

## Grading criteria
* Usefulness and complexity of the apps (most important)
* spark/vortex_app compiles with mvn clean install
* spark/vortex_appname_run.sh runs and outputs data as described in the report
* Quality of the report
