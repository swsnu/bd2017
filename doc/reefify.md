# A guide to REEF-ifying your project
*Joo Yeon Kim*

In this guide, we outline a few general steps to REEF-ify your project in a distributed environment. We assume your project to be along the lines of a backbone code/framework (ex. Spark) executing multiple instances of applications (ex. MapReduce, ML applications) in a distributed environment. For the scope of this guide, we simply refer your backbone code/framework that executes such applications to “code”.

### Writing your code:
REEF-ifying your project begins before importing REEF into your project. The more you design your code in a REEF-ifiable manner, the easier it is to run your code on REEF.

### What is REEF-ifiable?
Developers wishing to use REEF mostly desire to use it because it provides nice and simple abstractions around cluster resource managers. With our assumptions, it is helpful to think of the application (ex. MapReduce) running on your code to be a REEF client, and your code to be ported on driver and evaluators, the two main components of REEF that must be strictly decoupled.
1. Identify which part of the code acts as the driver and which part belongs to evaluators. Grouping code into these two high level packages helps to identify which part of your code runs where. It generally helps to maintain stateful components on the driver side and keep evaluator as ignorant and simple as possible. This way, the code that runs on the evaluators is made easily scalable.
2. In the driver, it helps to have a component that manages the pool of evaluators. Let’s call this component “evaluator manager” for the scope of this document. Other components may exist in the driver, such as managing the state of the application running on your project. These components usually exist as singleton objects.
3. The driver and evaluators can send and receive information using control messages. These control messages can be defined separately, say in a “common” package. We don’t have to do anything to this “common” package when REEF-ifying.
4. There are configurations that pertain to your code and configurations that pertain to your application. Clearly define these configurations in separate classes.

Before you begin, start by importing REEF modules to your project!

### Client-side
Assuming that you have an application that is submitted to your code, it is easy and straightforward to make the application be the client launching a REEF job.
1. Build code configuration (ex. how many evaluators you would like to use to execute the application) and application configuration (ex. MapReduce input data location in HDFS) using the classes you created as REEF Configuration class. You can easily do this by creating subclasses of REEF’s ConfigurationModuleBuilder.
2. Build driver configuration. Driver configuration includes mapping events such as “ON_DRIVER_STARTED” or “ON_EVALUATOR_ALLOCATED” to handlers that you define.
3. You may want to build other configurations such as network setup using NameServerConfiguration for NetworkConnectionService.
4. Merge all your configurations and submit them to DriverLauncher to start a REEF job.

### Driver-side
A good rule of thumb is to define the events to be included in driver configuration in a driver’s representative class. So we will start here.
1. Create a class named “{YOUR_PROJECT}Driver”
2. Define all the event handlers (StartTime, ActiveContext, AllocatedEvaluator, etc.). These event handlers will include calls to your code. Be careful when defining the event handlers. Think about how you want to manage your threads running your code, and rethink about to which extent you want to use REEF’s thread. There is too much room for flexibility here, so this part is up to you.
Regarding evaluator and context related event handlers: Work out how these will be related to your “evaluator manager” component. They will all probably have some reference to “evaluator manager” if not a part of it.
3. REEF provides a nice and easy-to-use dependency injector, Tang. To begin with, annotate the constructors of the singleton classes on driver side with @Inject. Class hierarchy/dependency will be managed automatically by Tang. If not, this might be a sign indicating that your classes may need some reorganization to make your code cleaner.
4. Make sure to close the active context when application execution is complete in order to finish the REEF job.

### Evaluator-side
The code you have in this package is probably mostly stateless and becomes stateful if necessary only after it begins running on an evaluator by communicating with the driver.
1. When an evaluator is allocated, submit the class that will remain running on an evaluator until the evaluator is removed as a REEF context. The class will mostly likely receive instructions from driver code to execute some part of your evaluator code, or a REEF task which is basically the application to be executed on the evaluator.
