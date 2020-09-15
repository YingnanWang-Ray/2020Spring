[toc]



# Software Engineering [Jiawei Wu]

## Programming many-cores: definitions and preliminaries



### 	Geeksforgeeks:

​	As far as I know, the multi-core architecture in a processor does not effect the program. The actual instruction execution is handled in a lower layer.



*Given that you have a multicore environment, Can I use any programming practices to utilize the available resources more effectively? How should I change my code to gain more performance in multicore environments?*



​	That is correct. Your program will not run any faster (except for the fact that the core is handling fewer other processes, because some of the processes are being run on the other core) unless you employ concurrency. If you do use concurrency, though, more cores improves the actual parallelism (with fewer cores, the concurrency is interleaved, whereas with more cores, you can get true parallelism between threads).



Making programs efficiently concurrent is no simple task. If done poorly, making your program concurrent can actually make it slower! For example, if you spend lots of time spawning threads (thread construction is really slow), and do work on a very small chunk size (so that the overhead of thread construction dominates the actual work), or if you frequently synchronize your data (which not only forces operations to run serially, but also has a very high overhead on top of it), or if you frequently write to data in the same cache line between multiple threads (which can lead to the entire cache line being invalidated on one of the cores), then you can seriously harm the performance with concurrent programming.

It is also important to note that if you have N cores, that DOES NOT mean that you will get a speedup of N. That is the theoretical limit to the speedup. In fact, maybe with two cores it is twice as fast, but with four cores it might be about three times as fast, and then with eight cores it is about three and a half times as fast, etc. How well your program is actually able to take advantage of these cores is called the parallel scalability. Often communication and synchronization overhead prevent a linear speedup, although, in the ideal, if you can avoid communication and synchronization as much as possible, you can hopefully get close to linear.



It would not be possible to give a complete answer on how to write efficient parallel programs on StackOverflow. This is really the subject of at least one (probably several) computer science courses. I suggest that you sign up for such a course or buy a book. I'd recommend a book to you if I knew of a good one, but the paralell algorithms course I took did not have a textbook for the course. You might also be interested in writing a handful of programs using a serial implementation, a parallel implementation with multithreading (regular threads, thread pools, etc.), and a parallel implementation with message passing (such as with Hadoop, Apache Spark, Cloud Dataflows, asynchronous RPCs, etc.), and then measuring their performance, varying the number of cores in the case of the parallel implementations. This was the bulk of the course work for my parallel algorithms course and can be quite insightful. Some computations you might try parallelizing include computing Pi using the Monte Carlo method (this is trivially parallelizable, assuming you can create a random number generator where the random numbers generated in different threads are independent), performing matrix multiplication, computing the row echelon form of a matrix, summing the square of the number 1...N for some very large number of N, and I'm sure you can think of others.

https://stackoverflow.com/questions/2467859/programming-for-multi-core-processors



### 	Paper: 

​	The number of cores integrated onto a single die is expected to climb steadily in the foreseeable future. This move to many-core chips is driven by a need to optimize performance per watt. How best to connect these cores and how to program the resulting many-core processor, however, is an open research question. Designs vary from GPUs to cache-coherent shared memory multiprocessors to pure distributed memory chips. 

https://www.neotextus.net/sc10.pdf



### 	Multi-core processor WIKI

https://en.wikipedia.org/wiki/Multi-core_processor



## Lock free concurrent data structures





### 	Reference paper: 

Concurrent data structures are the data sharing side of parallel programming. Data structures give the means to the program to store data, but also provide operations to the program to access and manipulate these data. These operations are implemented through algorithms that have to be efficient. In the sequential setting, data structures are crucially important for the performance of the respective computation. In the parallel programming setting, their importance becomes more crucial because of the increased use of data and resource sharing for utilizing parallelism.



The first and main goal of this chapter is to provide a sufficient background and intuition to help the interested reader to navigate in the complex research area of lock-free data structures. The second goal is to offer the programmer familiarity to the subject that will allow her to use truly concurrent methods.

https://arxiv.org/pdf/1302.2757.pdf

https://arxiv.org/abs/1302.2757



### 	Designing lock-free concurrent data structures

https://livebook.manning.com/book/c-plus-plus-concurrency-in-action/chapter-7/



### 	Introduction to Lock-Free Data Structures

https://www.baeldung.com/lock-free-programming



## Software transactional memory



### 	Related WIKI: 

https://en.wikipedia.org/wiki/Software_transactional_memory



### 	Related Paper: 

Called STM directly

https://course.ece.cmu.edu/~ece740/f13/lib/exe/fetch.php?media=shavit95-swtm.pdf



### 	Some application: 

http://book.realworldhaskell.org/read/software-transactional-memory.html



### 	An Introduction: 

https://berb.github.io/diploma-thesis/original/053_stm.html



## DSL stream programming

### 	Book Chapter 7: Programming Multicore and Many-core Computing Systems

https://uosc.primo.exlibrisgroup.com/discovery/fulldisplay?docid=cdi_hal_primary_oai_HAL_hal_00952318v1&context=PC&vid=01USC_INST:01USC&lang=en&search_scope=MyInst_and_CI&adaptor=Primo%20Central&tab=Everything&query=any,contains,DSL%20Stream%20Programming%20on%20Multicore%20Architectures%5C&offset=0



​		download source:

https://b-ok.org/book/2876976/d1ca39



### 	Book: Parallel Programming with Microsoft Visual C++: Design Patterns for Decomposition and Coordination on Multicore Architectures 

maybe not related

https://b-ok.cc/book/2736680/f698c8





### 	Dsl stream programming on multicore architectures

https://www.academia.edu/15293109/Dsl_stream_programming_on_multicore_architectures?auto=download







## Object oriented stream programming

### 	Book Chapter 9: Programming Multicore and Many-core Computing Systems

same book shown above, but in chapter 9

*I need  to read the chapter 9 above, then do the further searching; I am not sure the difference between Object oriented stream programming and OOP*



​	This chapter presents an approach unifying the concepts of object‐orientation (OO) and stream programming. The synthesis of object‐orientation and stream programming is a programming model suitable for mainstream software development. Integrating the concepts of stream programming into the object‐oriented paradigm offers great potential for both programmability and performance of parallel applications. The intuitive stream syntax provides an elegant way to express different types of parallelism on different layers. Important parallel patterns such as pipelines, master/ worker or divide and conquer can be efficiently implemented. The stream‐based syntax abstracts from threads, most synchronization, and performance‐critical parameters. This chapter illustrates these benefits with the example of XJava, a prototype object‐oriented stream programming (OOSP) language extending Java. In terms of the average programmer, OOSP reduces the risk of hard‐to‐find synchronization bugs as well as the need for performance optimizations while improving productivity and code maintainability.



### 	WIKI: Object-oriented programming

https://en.wikipedia.org/wiki/Object-oriented_programming





## Speculative parallelization techniques

### 	Book Chapter 10: Programming Multicore and Many-core Computing Systems

*Software-Based Speculative Parallelization*





### 	Paper: Speculative parallelization

​	The most promising technique for automatically parallelizing loops when the system cannot determine dependences at compile time is speculative parallelization. Also called thread-level speculation, this technique assumes optimistically that the system can execute all iterations of a given loop in parallel. A hardware or software monitor divides the iterations into blocks and assigns them to different threads, one per processor, with no prior dependence analysis. If the system discovers a dependence violation at runtime, it stops the incorrectly computed work and restarts it with correct values. Of course, the more parallel the loop, the more benefits this technique delivers. To better understand how speculative parallelization works, it is necessary to distinguish between private and shared variables. Informally speaking, private variables are those that the program always modifies in each iteration before using them. On the other hand, values stored in shared variables are used in different iterations.

https://www.computer.org/csdl/magazine/co/2006/12/04039262/13rRUB6SpVD



### 	New Data Structures to Handle Speculative Parallelization at Runtime

https://uosc.primo.exlibrisgroup.com/discovery/fulldisplay?docid=cdi_proquest_journals_1783853480&context=PC&vid=01USC_INST:01USC&lang=en&search_scope=MyInst_and_CI&adaptor=Primo%20Central&tab=Everything&query=any,contains,Speculative%20parallelization&offset=0





## Programming frameworks for analysis and optimization

### 	Part 3 & 4: Programming Multicore and Many-core Computing Systems

### 	Paper: A Declarative Framework for Analysis and Optimization

​	DeepWeaver-1 is a tool supporting cross-cutting program analysis and transformation components, called “weaves”. Like an aspect, a DeepWeaver weave consists of a query part, and a part which may modify code. DeepWeaver’s query language is based on Prolog, and provides access to data-flow and control-flow reachability analyses. DeepWeaver provides a declarative way to access the internal structure of methods, and supports cross-cutting weaves which operate on code blocks from different parts of the codebase simultaneously. DeepWeaver operates at the level of bytecode, but offers predicates to extract structured control flow constructs. This paper motivates the design, and demonstrates some of its power, using a sequence of examples including performance profiling and domain-specific performance optimisations for database access and remote method invocation.

https://link.springer.com/chapter/10.1007/978-3-540-71229-9_15





## HW and SW co-design and co-verification



### 	Article: Hardware-Software Co-Design Reappears

https://semiengineering.com/hardware-software-co-design-reappears/



### 	Paper: A Framework for Automated HW/SW Co-Verification of SystemC Designs using Timed Automata

https://d-nb.info/1004208499/34



### 	Paper: Formal Techniques for Effective Co-verification of Hardware/Software Co-designs

http://www.kroening.com/papers/dac2017.pdf



### 	Book:Co-verification of Hardware and Software for ARM SoC Design

https://b-ok.org/book/486370/2968cd



### 	Slides: HW/SW Co-design (Design of Embedded Systems)

Design of Embedded Systems - Jaap Hofstede Version 3, September 1999

http://web.cecs.pdx.edu/~mperkows/temp/0022.Software-Hardware-Co-Design.pdf



### 	Chapter: Hardware/Software Co-verification

https://link.springer.com/chapter/10.1007/0-306-46995-2_6



### 	Paper: The Hardware-Software Co-design and Co-verification of SoC for an Embedded Home Gateway

​	Home network is becoming an important part of global information infrastructure, where the embedded home gateway is a key information device to implement the interconnection and Internet access between devices in a home. In this paper, a SoC (system-on-a-chip) for embedded home gateway (EHGSoC) is proposed, which provides an integrated processing platform with the support of multiple network protocols, multiple hardware interfaces and reconfigurable computing. The hardware-software co-design and co-verification of the EHGSoC is deeply explored, whereas a co-design methodology with reconfigurable architecture is a focus. The aim of this research is to develop an EHGSoC and its corresponding system products, which are low power, high performance-to-price ratio, open and dependable.

https://ieeexplore.ieee.org/document/4690764



### 	Article: HW/SW co-verification basics: Part 1 – Determining what & how to verify

https://www.embedded.com/hw-sw-co-verification-basics-part-1-determining-what-how-to-verify/

## Implications for distributed operating system



### 	Paper: DISTRIBUTED OPERATING SYSTEM- AN OVERVIEW

http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.838.9721&rep=rep1&type=pdf



### 	WIKI: Distributed operating system

https://en.wikipedia.org/wiki/Distributed_operating_system



### 	Paper: Your computer is already a distributed system. Why isn’t your OS

https://www.usenix.org/legacy/events/hotos09/tech/full_papers/baumann/baumann.pdf



### 	Geeksforgeeks: Features of Distributed Operating System

https://www.geeksforgeeks.org/features-of-distributed-operating-system/



### 	Slides: Distributed Operating Systems

https://www.cs.columbia.edu/~smb/classes/s06-4118/l26.pdf



### 	Slides: Introduction to Distributed Systems (DS)

https://www.uio.no/studier/emner/matnat/ifi/INF5040/h08/lectures/2008_08_26_IntroDS.pdf



































