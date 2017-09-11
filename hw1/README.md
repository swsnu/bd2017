# [HW1] Logistic regression on REEF
**Due: 9/25 (Mon) 18:00 (This is a hard deadline)**

The first programming assignment is to build a machine learning (ML) application that runs on distributed environments! You will first start with REEF’s local runtime where you can run the application within a single machine (e.g., desktop, laptop, etc) as if you run it on distributed machines. We will also see how easily you can port the application to different runtime environment (e.g., YARN) using REEF.

Another beauty of REEF is to re-use the Evaluators; instead of allocating resources whenever to launch Tasks, REEF allows users to re-submit Tasks while bookkeeping the state from the previous Tasks. This functionality fits perfectly for the ML applications, which are iterative. In this assignment, we will preserve the dataset as Evaluators' states across different Tasks.

## Algorithm
You will implement a simple binary classification algorithm with a linear model of Logistic Regression using SGD (Stochastic Gradient Descent). If you are not familiar with the terms, you do not need to worry about it. We encourage you to watch [Andrew Ng’s lecture](https://www.youtube.com/playlist?list=PLfc5V2mDPVQSEMztg4GN4wGB8cx-b_zvi) to understand it. After spending in roughly 1.5 hours, you will learn the basics required for this assignment.

## Dataset
We will use the Sonar dataset, which is for discriminating sonar signals bounced off a metal cylinder (labeled as “M”) and those bounced off a rock (labeled as “R”). The dataset consists of 208 instances and each instance has 60 features (real numbers in [0.0, 1.0]) and a label (either "R" or "M"). You can find the detailed description [here]( https://archive.ics.uci.edu/ml/datasets/Connectionist+Bench+(Sonar,+Mines+vs.+Rocks)).

Since ML datasets are usually large-scale, most ML applications use distributed storage like Hadoop Distributed File System (HDFS) for loading dataset. In this project, however, we will access the dataset as a Java object. One instance of training data looks as follows:

```
package edu.snu.bd.lr.data;
/**
 * Represents an instance of Sonar dataset.
 */ 
public final class SonarData {
  /**
   * The feature values in the data.
   * features[i] <= 1.0 && features[i] >= 0.0, features.length == 60
   */
  private final double[] features;

  /**
   * Whether the sonar signals bounced off a metal cylinder (labeled as “M”)
   * and those bounced off a rock (labeled as “R”).
   * label = 1 for "M" and label = 0 for "R"
   */
  private final int label;
}
```

   ### Note (Can skip if you are in a hurry)
   Although REEF already has an API (namely DataLoading) for loading data from HDFS, we experienced that the program structure usually becomes too complex for a class assignment. So we decided to simplify this part so that students can spend more time on building the core logic of the REEF application. Note that, for the large dataset in GB or TB (or larger!) scale, we should use the storage like HDFS.

## Preparation
1. You must install the following softwares:
  * Git
  * Java 8 SDK
  * Apache Maven
  * Apache YARN 2.7 (install in [pseudo-distributed mode](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html#Pseudo-Distributed_Operation))
2. Setup your repository as described in the Submission guideline
3. Download the tarball ([hw1-reef.tar.gz](https://github.com/swsnu/bd2017/blob/master/hw1/hw1-reef.tar.gz)) and untar it. 
4. Run following commands then Maven will resolve all the necessary dependency.
  ```
  $ tar -xvzf hw1-reef.tar.gz
  $ cd hw1-reef
  $ mvn clean install
  ```
5. Load the maven project in your IDE as you learned in the tutorial.


### Tips for YARN installation
   * We recommend using Ubuntu 14.04 LTS or Mac OS X. When we tried to install YARN in some platforms such as Windows or latest version of linux, we spent our time just for troubleshooting. If your machine runs on those systems, we recommend that Virtual machines ([VirtualBox](https://www.virtualbox.org/wiki/Downloads) works well and it is free!).
   * You can find many guidelines or tutorials on the web (e.g., "How to install hadoop"), but hadoop has evolved a lot for years, thus you can make unnecessary changes to your system. We recommend you to stick to the tutorial in official site [5]. If something does not work, post a question on the discussion.
   * If you are not able to use 22 port for SSH login, try this trick in the hadoop mailing list ([link](http://mail-archives.apache.org/mod_mbox/hadoop-general/201211.mbox/%3CCAOcnVr27vFeY1mJAhWaRuQeBuwfuuAM39e1zjkaLj8uvzCUXRg@mail.gmail.com%3E))
  
  
## Specification
Our logistic regression runs as follows:
1. The Client launches REEF Driver\*.
1. The Driver allocates the `N` Evaluators, where `N` is configured to NumEvaluators\*\*.
1. The Driver submits Context and Service to Evaluators in 2, assigning each **disjoint** subset of data to be accessible through an object that is binded as `Service`\*\*\*.
1. The Driver submits Tasks with setting the model parameters as `Memento`\*\*\*\*. 
1. Tasks compute gradient by using the model parameters and data partition assigned to the Task. 
1. Tasks encode the gradient as a `byte[]` and return it. REEF internally sends this `byte[]` to the Driver, and you can access it through `CompletedTask.get()`.
1. Driver waits for all Tasks complete 5-6.
1. Aggregate the gradients to update the model parameters.
1. Repeat 5-8 until model converges. To save our time, let's limit the number of iterations to `MaxNumIterations`\**.

   \* You should write two versions: `LRClientLocal` for local runtime and `LRClientYARN` for YARN runtime (Hint: take a look at HelloREEF in REEF Examples).
   
   \*\* Those variables should be able to set via NamedParameter with a short name of `num_eval` and `max_num_iters`, respectively
   
   \*\*\* Hint: Take a look at `StatePassing` in REEF Tests
   
   \*\*\*\* Hint: Please refer to the REEF API doc for Task.


## Running your application
Once you implemented the application, you should be able to run the application on both local and yarn runtime.
```
$ bash ./run_local.sh # run your application on the local machine.
$ bash ./run_yarn.sh # run your application on the YARN runtime. You should set the necessary variables in the script
```

Note that when you run the application on YARN, the daemons should be running. Assuming your YARN is installed in your local machine as pseudo distributed mode, you should be able to see the daemons when you type `jps`:
```
$ jps
28723 Jps
7475 NodeManager
7096 ResourceManager
6925 SecondaryNameNode
6686 DataNode
6463 NameNode
```

## Grading criteria
1. The source code compiles with `mvn clean install`. Note that `checkstyle` plugin will help you write a clean code! ;)
1. Your application should be able to run both the local and yarn runtime.
1. The named parameters should be used properly.
1. The dataset should be split into disjoint partitions, each of which is assigned to an Evaluator.
1. Your code should compute gradients and update model parameters correctly.
1. Take a screenshot from Resource Manager Web UI (`${RM_IP_ADDR}:8088`) when you run the application on YARN.
1. Upload the file to your repository under `hw1` directory (**Make sure you pushed to the master branch!**)

## Submission
Due: 9/25 (Mon) 17:59 (This is a hard deadline)

You must create your own *private* repository under your account and **send them by email to `bd-tas@spl.snu.ac.kr`**.
**Be sure to add the TAs as collaborators in your repository settings!** (Detailed instructions below).
We will check the snapshot of the *master* branch of your Github repository at the deadline and grade it.
Please untar the maven project as described in [Preparation](#preparation) and keep the project structure as is.
Make sure to push your work on Github on time.


### Instructions on creating a private repository

#### I. Get GitHub Student Developer Pack (You can skip if you already did it)

1. Go to `https://education.github.com`.
2. Follow instructions to `Request a discount`.


#### II. Make your private repository
1. Go to your github profile: `https://github.com/YOUR_USERNAME`.
2. Under `Repositories`, click on `New`.
3. Fill in `bd17f-USERNAME` as your repository name, and mark is as `Private`.
4. Hit `Create repository`.
5. In terminal, go to the directory that you will be working in (e.g. `~/workspace/bd17f-USERNAME` or `~/bd17f-USERNAME`)
6. Type in and run the following commands, which is also shown on the page that you will be looking at after step 4:

```
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME
git push -u origin master
```

7. Under `Settings` then `Collaborators` tab, Add TAs as your collaborators: @yunseong, @johnyangk and @joosjeong.
8. You're all set! After finishing your homework, push your contents to your repository on time!

## References
* REEF Wiki https://cwiki.apache.org/confluence/display/REEF/Home
* REEF Repo https://github.com/apache/reef
* Logistic Regression lectures by Andrew Ng.
https://www.youtube.com/playlist?list=PLfc5V2mDPVQSEMztg4GN4wGB8cx-b_zvi
* Sonar dataset https://archive.ics.uci.edu/ml/datasets/Connectionist+Bench+(Sonar,+Mines+vs.+Rocks)
* Hadoop: Setting up a Single Node Cluster https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html#Pseudo-Distributed_Operation
* VirtualBox https://www.virtualbox.org/wiki/Downloads
* Changing SSH ports in Hadoop http://mail-archives.apache.org/mod_mbox/hadoop-general/201211.mbox/%3CCAOcnVr27vFeY1mJAhWaRuQeBuwfuuAM39e1zjkaLj8uvzCUXRg@mail.gmail.com%3E
* How to REEF-ify Guide https://github.com/swsnu/bd2017/blob/master/doc/reefify.md
