# Big Data and Deep Learning Systems
## Fall 2017

## Schedule
| Week | Lecture | Homework/Project |
|------|---------|------------------|
| Week1 9.5/7 | Introduction. Resource Manager: YARN, Mesos, Borg | |
| Week2 9.12/14 | Meta-framework: REEF, Dataflow processing: MR, Dryad | HW1 out |
| Week3 9.19/21 | Dataflow processing: Spark, Tez, Vortex, Naiad | Project proposal due |
| Week4 9.26/28 | Programming: Hive, DryadLINQ, Spark/Shark, Pig, FlumeJava, Beam | HW1 due, HW2 out |
| Week5 10.3/5 Choosuk - No class | | |
| Week6 10.10/12 | ML/DL framework: Parameter server/Tensorflow | Project progress check |
| Week7 10.17/19 | DL framework - Tensorflow/Caffe2/Torch | HW2 due, HW3 out |
| Week8 10.24/26 | Mid-presentation | |
| Week9 10.31/11.2 (10.31 SOSP - No class) | DL framework - Tensorflow/Caffe2/Torch | |
| Week10 11.7/9 | Stream processing: SparkStreaming, Storm, Heron, Flink, MIST | HW3 due |
| Week11 11.14/16 | Graph processing: Pregel, GraphLab, X-Stream, Arabesque | Project progress check |
| Week12 11.21/23 | Graph processing - Pregel, GraphLab, X-Stream, Arabesque | |
| Week13 11.28/30 | DS - GFS, Bigtable, Dynamo | |
| Week14 12.5/7 | Coordination - Chubby, Zookeeper | Project progress check |
| Week15 12.12/14 | TBD | Project report due |
| Week16 12.19 | Poster session | |

## Reading list

### Resource management
- [YARN](https://www.cs.cmu.edu/~garth/15719/papers/yarn.pdf). Apache Hadoop YARN: Yet Another Resource Negotiator. Vinod Kumar Vavilapalli, Arun C Murthy, Chris Douglas, Sharad Agarwal, Mahadev Konar, Robert Evans, Thomas Graves, Jason Lowe, Hitesh Shah, Siddharth Seth, Bikas Saha, Carlo Curino, Owen O’Malley, Sanjay Radia, Benjamin Reed, Eric Baldeschwieler. SOCC 2013.
- [Mesos](http://static.usenix.org/event/nsdi11/tech/full_papers/Hindman_new.pdf). Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center. Benjamin Hindman, Andy Konwinski, Matei Zaharia, Ali Ghodsi, Anthony D. Joseph, Randy Katz, Scott Shenker, Ion Stoica. NSDI 2011.
- [Borg](https://pdos.csail.mit.edu/6.824/papers/borg.pdf). Large-scale cluster management at Google with Borg. Abhishek Verma, Luis Pedrosa, Madhukar Korupolu, David Oppenheimer, Eric Tune, John Wilkes. EuroSys 2015.  

### Meta-framework
- [REEF](). Apache REEF: Retainable Evaluator Execution Framework. Byung-Gon Chun, Tyson Condie, Yingda Chen, Brian Cho, Andrew Chung, Carlo Curino, Chris Douglas, Matteo Interlandi, Beomyeol Jeon, Joo Seong Jeong, Gye-Won Lee, Yunseong Lee, Tony Majestro, Dahlia Malkhi, Sergiy Matusevych, Brandon Myers, Mariia Mykhailova, Shravan Narayanamurthy, Joseph Noor, Raghu Ramakrishnan, Sriram Rao, Russell Sears, Beysim Sezgin, Tae-Geon Um, Julia Wang, Markus Weimer, Markus Weimer, Youngseok Yang. ACM TOCS September 2017.

### Dataflow Processing Framework
- [MapReduce](https://www.usenix.org/legacy/event/osdi04/tech/full_papers/dean/dean.pdf). MapReduce: Simpliﬁed Data Processing on Large Clusters. Jeffrey Dean and Sanjay Ghemawat. OSDI 2004.
- [Dryad](http://cs.brown.edu/~debrabant/cis570-website/papers/dryad.pdf). Dryad: Distributed Data-Parallel Programs from Sequential Building Blocks. Michael Isard, Mihai Budiu, Yuan Yu, Andrew Birrell, Dennis Fetterly. Eurosys 2007.
- [Spark](https://www.cs.berkeley.edu/~matei/papers/2012/nsdi_spark.pdf). Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing. Matei Zaharia, Mosharaf Chowdhury, Tathagata Das, Ankur Dave, Justin Ma, Murphy McCauley, Michael J. Franklin, Scott Shenker, Ion Stoica. NSDI 2012.
- [CIEL](https://www.usenix.org/legacy/event/nsdi11/tech/full_papers/Murray.pdf). CIEL: a universal execution engine for distributed data-ﬂow computing. Derek G. Murray, Malte Schwarzkopf, Christopher Smowton, Steven Smith, Anil Madhavapeddy, Steven Hand. NSDI 2011.
- [Naiad](http://research.microsoft.com/pubs/201100/naiad_sosp2013.pdf). Naiad: A Timely Dataﬂow System. Derek G. Murray, Frank McSherry, Rebecca Isaacs, Michael Isard, Paul Barham, Martin Abadi. SOSP 2013.
- [Tez](https://www.cse.ust.hk/~weiwa/teaching/Fall16-COMP6611B/reading_list/Tez.pdf). Apache Tez: A Unifying Framework for Modeling and
Building Data Processing Applications. Bikas Saha, Hitesh Shah, Siddharth Seth, Gopal Vijayaraghavan, Arun Murthy, Carlo Curino. SIGMOD 2015.

### High-level Data Processing Programming 
- [Hive](http://infolab.stanford.edu/~ragho/hive-icde2010.pdf). Hive – A Petabyte Scale Data Warehouse Using Hadoop. Ashish Thusoo, Joydeep Sen Sarma, Namit Jain, Zheng Shao, Prasad Chakka, Ning Zhang, Suresh Antony, Hao Liu and Raghotham Murthy. ICDE 2010.
- [Pig](http://infolab.stanford.edu/~olston/publications/sigmod08.pdf). Pig Latin: A Not-So-Foreign Language for Data Processing. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins. SIGMOD 2008.
- [FlumeJava](http://pages.cs.wisc.edu/~akella/CS838/F12/838-CloudPapers/FlumeJava.pdf). FlumeJava: Easy, Efﬁcient Data-Parallel Pipelines. Craig Chambers, Ashish Raniwala, Frances Perry, Stephen Adams, Robert R. Henry, Robert Bradshaw, Nathan Weizenbaum. PLDI 2010.
- [DryadLINQ](https://www.usenix.org/legacy/event/osdi08/tech/full_papers/yu_y/yu_y.pdf). DryadLINQ: A System for General-Purpose Distributed Data-Parallel Computing Using a High-Level Language. Yuan Yu, Michael Isard, Dennis Fetterly, Mihai Budiu, Úlfar Erlingsson, Pradeep Kumar Gunda, Jon Currey. OSDI 2008.
- [SCOPE](http://research.microsoft.com/en-us/um/people/jrzhou/pub/Scope.pdf). SCOPE: Easy and Efficient Parallel Processing of Massive Data Sets. Ronnie Chaiken, Bob Jenkins, Per-Åke Larson, Bill Ramsey, Darren Shakib, Simon Weaver, Jingren Zhou. VLDB 2008. 
- [Beam](https://beam.apache.org/). Apache Beam.

### Machine Learning/Deep Learning














