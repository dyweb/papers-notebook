# 论文笔记

这个 repo 希望能够记录自己阅读论文的过程，其中的论文一部分来自于上课需要阅读的论文，这部分会比较偏安全和虚拟化。还有一部分论文是自己感兴趣，想去了解的，这部分可能比较偏虚拟化和分布式。论文笔记希望能够记录自己在读论文的时候的想法，其中包括但不限于论文的大致 idea，实现方式，以及自己对论文的评价等等。希望能够把每一篇论文的笔记限制在 1000 字以内。

## 目录(TOC)


* [分布式(Distributed System)](#分布式distributed-system)
    * [调度器(Scheduler)](#调度器scheduler)
      * [Mesos](#mesos)
      * [Omega](#omega)
      * [Borg](#borg)
      * [Yarn](#yarn)
      * [Sparrow](#sparrow)
      * [Apollo](#apollo)
      * [Hawk](#hawk)
      * [Mercury](#mercury)
      * [ERA](#era)
      * [Efficient Queue Management for Cluster Scheduling](#efficient-queue-management-for-cluster-scheduling)
      * [Tarcil](#tarcil)
      * [Eagle](#eagle)
      * [Canary](#canary)
      * [Profiling a warehouse-scale computer](#profiling-a-warehouse-scale-computer)
    * [Graph Computation](#graph-computation)
      * [Wukong](#wukong)
    * [Lock Service](#lock-service)
      * [Chubby](#chubby)
    * [一致性(Consensus)](#一致性consensus)
      * [Raft](#raft)
      * [Zookeeper](#zookeeper)
      * [Paxos](#paxos)
    * [存储(Storage)](#存储storage)
      * [BigTable](#bigtable)
      * [Dynamo](#dynamo)
      * [Spanner](#spanner)
      * [Distributed Transaction with RDMA and HTM](#distributed-transaction-with-rdma-and-htm)
      * [Key-Value Store with RDMA](#key-value-store-with-rdma)
      * [RDF Store with RDMA](#rdf-store-with-rdma)
      * [Ambry](#ambry)
* [虚拟化(Virtualization)](#虚拟化virtualization)
    * [虚拟机管理器(Hypervisor)](#虚拟机管理器hypervisor)
      * [Xen](#xen)
      * [kvm](#kvm)
    * [容器(Container)](#容器container)
      * [mbox](#mbox)
      * [Slacker](#slacker)
* [沙箱(Sandboxing)](#沙箱sandboxing)
    * [系统调用拦截(System Call Interposition)](#系统调用拦截system-call-interposition)
      * [Janus](#janus)
      * [Ostia](#ostia)
    * [软件故障隔离(Software-based Fault Isolation)](#软件故障隔离software-based-fault-isolation)
      * [SFI](#sfi)
      * [Google Native Client](#google-native-client)
      * [Language-Independent Sandboxing](#language-independent-sandboxing)
* [系统(System)](#系统system)
    * [文件系统(File System)](#文件系统file-system)
      * [RAMCloud](#ramcloud)
      * [Optimistic Crash Consistency](#optimistic-crash-consistency)
      * [F2FS](#f2fs)
      * [PMFS](#pmfs)
    * [操作系统(Operating System)](#操作系统operating-system)
      * [Exokernel](#exokernel)
    * [Memory](#memory)
      * [Transactional Memory](#transactional-memory)
    * [CFI](#cfi)
      * [Control-Flow Integrity](#control-flow-integrity)
    * [Tail Latency](#tail-latency)
      * [The Tail at Scale](#the-tail-at-scale)
    * [Lock](#lock)
      * [Non-scalable locks are dangerous](#non-scalable-locks-are-dangerous)
    * [Bug](#bug)
      * [STACK](#stack)
* [安全(Security)](#安全security)
    * [虚拟机安全(Virtulization, Security)](#虚拟机安全virtulization-security)
      * [CloudVisor](#cloudvisor)
    * [Taint Tracing](#taint-tracing)
      * [TaintDroid](#taintdroid)
    * [ROP](#rop)
      * [Hacking Blind](#hacking-blind)
* [大数据](#大数据)
    * [框架](#框架)
      * [Hadoop](#hadoop)
      * [Spark](#spark)
    * [Data Processing](#data-processing)
      * [Realtime Data Processing at Facebook](#realtime-data-processing-at-facebook)
* [网络](#网络)
    * [Network Function Virtualization(NFV)](#network-function-virtualizationnfv)
      * [Click](#click)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

## 分布式(Distributed System)

### 调度器(Scheduler)

* [Comparison of Container Schedulers](https://medium.com/@ArmandGrillet/comparison-of-container-schedulers-c427f4f7421)
* [集群调度框架的架构演进之路](http://www.infoq.com/cn/articles/scheduler-architectures)

在我看来，分布式是研究如何让程序能够在多台机器上运行拥有更好的性能的一个方向。那如果要实现这一点，调度很关键。

目前我读过的与分布式调度器有关的论文有 Mesos, Omega, Yarn 和 Borg。这其中 Mesos 是最早的，它来自伯克利大学。其最亮的地方在于两层调度的框架，使得调度跟框架是松耦合的。目前很多公司都是在用它的，而且有很多基于 Mesos 的创业公司，比如 Mesosphere 等等。Omega 跟 Mesos 的第二作者是一个人。Omega 具体来说也不知道算是谁写的，应该算是大学和谷歌一起做的研究。Omega 发表在 EuroSys'13 上，是在基于 Mesos 的基础上，提出了一种完全并行的调度的解决方案，能够在调度上有更好的性能。不过论文是采取的模拟实验来进行验证的，不知道是否有生产上的使用。Borg 是发表在 EuroSys'15 上的，是谷歌真正一直在使用的集群管理工具。可以说谷歌为什么可以用廉价的机器来达到很高的可用性，有很大一部分是因为 Borg。Borg 和 Omega 是不开源的，而 Mesos 是开源的，不过 Borg 有一个开源的继任者，就是目前大名鼎鼎的 Kubernetes。Kubernetes 下面用 Docker Conatainer，而不是 Linux 内核的那些用来做进程级别的性能隔离的特性来实现的。不过最近 Kubernetes 似乎在跟 Docker 撕逼，因为 Docker 公司的一些强势吧，可能之后不会只支持 Docker Conatiner。Kubernetes 在很长的时间内都不是 production ready 的，只能支持 100 个节点，跟 Borg 的 10K 完全没法比，不知道现在是什么情况。

#### Mesos

* [Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center](https://people.eecs.berkeley.edu/~alig/papers/mesos.pdf)
* [Apache Mesos](https://github.com/apache/mesos)

Mesos 是调度器绕不过去的一篇论文，因为它是目前在工业界应用最广泛的开源系统之一。Mesos 跟 exokernel 的思想很像，分离了管理和保护。之前所有的调度器，都是由自己来决定是否把资源给某个任务的，在 mesos 中是 framework 决定是否接受资源的要约。在容灾上，mesos 是通过 zookeeper 做 leader election 的，而且 master 维护的状态是 soft 的，因此 master 没有单点故障的问题，而对于 node 的故障，会反馈给 framework，让他们来处理。Mesos 就像是一个最小的 kernel，所有的决定都是『用户态』的 framework 来做的。

Mesos 是在 node 上给每一个 framework 运行一个 executor 的，使用 Linux Containers 等容器技术来做隔离。最后通过在一个集群上跑多个 framework 的方式来验证 mesos 的可用性。这篇文章整体来说比较简单，也可能是因为听过太多次关于 mesos 的架构了。

#### Omega

[Omega: flexible, scalable schedulers for large compute clusters](http://web.eecs.umich.edu/~mosharaf/Readings/Omega.pdf)

```
// TODO Add the notes
```

#### Borg

[Large-scale cluster management at Google with Borg](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/43438.pdf)

Borg 是谷歌发表在 EuroSys'15 上的一篇文章，讲述了其内部是如何做集群管理的。Borg 是我看过的第一篇关于集群管理的论文。首先介绍下 Borg 的特点，Borg 是一个用来做集群管理的工具，它的目标就是让跑在它上面的应用能够拥有很好的可用性和可靠性的同时，能够提高机器的利用率，而且这些是在一个非常大规模的机器环境下。在 Borg 被设计的时候，还没有对虚拟化的硬件支持，也就是 Intel VT-x 等等那些硬件特性，所以 Borg 是使用了进程级别的隔离手段，也就是 Control Group。

Borg 的架构其实还挺简单的，是比较经典的 Master/Slave 架构，其中在 Master 部分，有两个抽象的进程，一个是 Borg Master，一个是 Borg Scheduler。Borg Master 是有5个备份的，每个都会在内存里维护集群里所有对象的状态。他们5个组成了一个小集群，用 Paxos 算法做一致性的，会选举出一个 leader 来处理请求，这点是之前 Kubernetes 做不到的。

调度器方面的实现比较简单，就是一个队列，根据优先级做 round-robin。这里读起来感觉没什么新意，就不多说了。调度的平均时间大概是 25s，其中 80% 的时间在下载包。谷歌也是实诚，下载安装包的时间都算到调度里面去。

谷歌写的论文一向是简单易懂，特别良心的。所以要是对集群感兴趣，可以去看下这篇论文，花两三个小时就能看完了。

#### Yarn

[Apache Hadoop YARN: Yet Another Resource Negotiator](https://www.sics.se/~amir/id2221/papers/2013%20-%20Apache%20Hadoop%20YARN%20-%20Yet%20Another%20Resource%20Negotiator%20(SoCC).pdf)

```
// TODO Wait to read
```

#### Sparrow

* [Sparrow: Distributed, Low Latency Scheduling](https://people.eecs.berkeley.edu/~keo/publications/sosp13-final17.pdf)
* [Sparrow in GitHub](https://github.com/radlab/sparrow)

Sparrow 是一个与前面的调度器架构都不一样的实现，是去中心化的架构。之前的所有调度器，无论是 monolithic 的还是后面 Mesos 那样两层的架构，都是有一个中心化的调度器在运行，这样的方式会使得调度器的效率不是那么高。 Sparrow 是 AMPLab 的又一力作，发表在 SOSP'13 上，它不是一个 general-purpose 的调度器，而是针对 short job 这一特殊的 workload。其灵感来源于一个负载均衡方面的经典论文，k choices。这篇文章的 idea 是，在 k 台机器里选一个最好的，而不是在 n 里选一个最好的，可以大大降低负载均衡的 overhead，同时也对负载的分配跟最优解差不了多少。

Sparrow 核心的思想就是，在分配 task 的时候，随机选择几个 worker，然后选一个最合适的。因为在 Sparrow 的应用场景里，所有的 task 都是很短的，因此只需要考虑任务队列的长度就差不多了。针对多 task 的分配问题，Sparrow 对其进行了批处理来优化，比如 task 为 2 的时候，不是进行两次选择，每次在 k 个 worker 里选择 1 个，而是在 2k 个 worker 里选择 2 个。

这篇论文读起来很轻松，idea 比较简单，但是是跟之前完全不一样的思路。讲道理，其实 idea 很容易想，在读 k choices 的时候就想这样的思想能不能用在调度器上，结果人家早就想到了。

目前 Sparrow 开源在 GitHub 上，但是没听说哪个公司在把它用在生产系统上。它的最大的问题就是 workload 比较单一，只能对 short job 有比较好的效果。

#### Apollo

* [Apollo: Scalable and Coordinated Scheduling for Cloud-Scale Computing](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-boutin_0.pdf)
* [Slides in USENIX](https://www.usenix.org/sites/default/files/conference/protected-files/osdi14_slides_boutin.pdf)

```
// TODO Wait to read
```

#### Hawk

* [Hawk: Hybrid Datacenter Scheduling](https://www.usenix.org/system/files/conference/atc15/atc15-paper-delgado.pdf)
* [Slides in USENIX ATC 2015](https://project.inria.fr/epfl-Inria/files/2016/02/talk-pameladelgado.pdf)

有一种寻找 idea 的方法，就是在已有的两种极端的思想中做一个你中有我，我中有你的取舍，所有计算机的问题，都是 trade off 嘛，两种极端肯定是为了不同的追求，然后提出一个折中的方案，往往是一种取巧的想 idea 的方式。类似的例子有 monolithic kernel，micro kernel 和 hybrid kernel。这篇文章也是这样，它是把去中心化和中心化的调度器做了一个混合。之前提到，去中心化的实现只适合 short job 的 workload，Hawk 在调度 long job 的时候会使用中心化的调度器，在调度 short job 的时候会使用去中心化的调度器。

除此之外，Hawk 为了两者能够更好地在一起工作还做了一些微小的工作，比如在一个 worker 做完所有的事情后，会偷别的 worker 的 short job 来做，果然资本主义的 worker 都是积极进取的。

哦对了，这篇文章的验证是使用谷歌开放的自己的 tracing 数据集来做的，这个数据集也是开源的，在 [https://github.com/google/cluster-data](https://github.com/google/cluster-data) 可以找到。

#### Mercury

* [Mercury: Hybrid Centralized and Distributed Scheduling in Large Shared Clusters
](https://www.usenix.org/system/files/conference/atc15/atc15-paper-karanasos.pdf)
* [Slides in USENIX ATC 2015](https://www.usenix.org/sites/default/files/conference/protected-files/atc15_slides_karanasos.pdf)

Mercury 跟 Hawk 是约等于一模一样的 idea，都是总结了中心化的调度器和去中心化各自的优缺点后，提出了一种 hybrid 的方法。Mercury 是由微软提出的，PPT 做的更好看一些。它在 YARN 上进行了一个实现，Hawk 则是实现在了 Spark 上。说明论文还是有实现有实验才行。

#### ERA

* [ERA: A Framework for Economic Resource Allocation for the Cloud](https://arxiv.org/pdf/1702.07311.pdf)

```
// TODO Add the notes
```

#### Efficient Queue Management for Cluster Scheduling

* [Efficient Queue Management for Cluster Scheduling](http://dl.acm.org/citation.cfm?doid=2901318.2901354)

```
// TODO Add the notes
```

#### Tarcil

* [Tarcil: Reconciling Scheduling Speed and Quality in Large Shared Clusters](http://web.stanford.edu/~cdel/2015.socc.tarcil.pdf)
* [Tarcil: High Quality and Low Latency Scheduling in Large, Shared Clusters](https://web.stanford.edu/group/mast/cgi-bin/drupal/system/files/2014.techreport.tarcil_0.pdf)

```
// TODO Add the notes
```

#### Eagle

* [Eagle: A Better Hybrid Data Center Scheduler](https://pdfs.semanticscholar.org/36b3/3a2e5ee77e6b191aa8bfaf4a5aac450d1b57.pdf)
* [Job-aware Scheduling in Eagle: Divide and Stick to Your Probes](https://infoscience.epfl.ch/record/221125/files/socc2016-final189.pdf)

```
// TODO Wait to read
```

#### Canary

* [Canary: A Scheduling Architecture for High Performance Cloud Computing](http://hci.stanford.edu/cstr/reports/2016-01.pdf)

```
// TODO Wait to read
```

#### Profiling a warehouse-scale computer

* [Profiling a warehouse-scale computer](http://delivery.acm.org/10.1145/2760000/2750392/p158-kanev.pdf)

```
// TODO Wait to read
```

### Graph Computation

#### Wukong

[Fast and Concurrent RDF Queries with RDMA-based Distributed Graph Exploration](http://ipads.se.sjtu.edu.cn/lib/exe/fetch.php?media=publications:wukong-osdi16.pdf)

Wukong 是去中心化的设计，server 可以独立处理请求，每个 server 有两部分，一部分是 query engine，另一部分是 store。

Wukong 一共有两种 index，Type 和 Predicate。

> What are the bottlenecks of existing RDF systems?

一共有两种不同的实现，分别是 Triple store and triple join 和 Graph store and graph exploration。前者是以 triple 的方式来将 RDF 数据存储在关系型数据库中，因此查询有两个步骤，scan 和 join。scan 会分为子查询，最后再借由 hash join 之类的 join 的操作将查询的结果 join 在一起。由此可知如果数据非常大的时候，最后的 join 会是很大的问题。

第二种方式是以图的方式来存储和查询 RDF。这样的方式以 Trinity.RDF 为代表，有一些剪枝的优化。但是最后也会有一个 final join 的过程。

纵观之前的实现，最后的 join 是一个最大的问题。

> What are the differences between Wukong and prior graph-based designs? What are the benefits?

最大的不同在于索引的存储方式。之前的基于图的设计都是用独立的索引数据结构来存索引，但是 Wukong 是把索引同样当做基本的数据结构（点和边）来存储。并且会考虑分区来存储这些索引。

这样做有两个好处，第一点就是在进行图上的遍历或者搜索的时候可以直接从索引的节点开始，不用做额外的操作。第二点是这样使得索引的分布式存储变得非常简单，复用了正常的数据的存储方式。

> What is full-history pruning and what's the difference compared with the prior pruning approach? Why can Wukong adopt full-history pruning?

Full-history 就是说所有的历史记录都会被记录下来。之前是只记录一次的。之所以可以这样做是因为一方面 RDF 的查询都不会有太多步，而且 RDMA 在低于 2K bytes 的时候性能都是差不多的，所以 Wukong 可以这样做。

### Lock Service

#### Chubby

[The Chubby lock service for loosely-coupled distributed systems](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/chubby-osdi06.pdf)

```
// TODO Wait to read
```

### 一致性(Consensus)

#### Raft

* [In Search of an Understandable Consensus Algorithm(Extended Version)](https://raft.github.io/raft.pdf)
* [Slides](https://speakerdeck.com/benbjohnson/raft-the-understandable-distributed-consensus-protocol)

这篇论文是公认的好读，好实现，但是也只是相对于 Paxos 来说的。本身还是具有一定的背景才能读懂的。因为暂时还没正式读过 Paxos，所以不好作比较。但是 Raft 是利用了 State Machine 的思想，将分布式一致性问题分成 Leader 选举和日志同步两个问题，这样的做法很简单粗暴。

我觉得能理解 Raft 是不难的，但是明白听他的正确性是如何保证的才比较难。越是简洁的东西背后的数学证明应该越复杂，并没有了解过。

#### Zookeeper

[ZooKeeper: Wait-free coordination for Internet-scale systems](http://static.cs.brown.edu/courses/csci2270/archives/2012/papers/replication/hunt.pdf)

```
// TODO Wait to read
```

#### Paxos

* [Paxos Made Live - An Engineering Perspective](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/paxos_made_live.pdf)
* [The Part-Time Parliament](http://research.microsoft.com/en-us/um/people/lamport/pubs/lamport-paxos.pdf)
* [Paxos Made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)

```
// TODO Wait to read
```

### 存储(Storage)

#### BigTable

[Bigtable: A Distributed Storage System for Structured Data](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)

> What is the relationship between table , tablet and sstable?

Table 就是用户可见的表，它有 Row，Column，和不同 Version 的 Value。Table 会从逻辑上划分为多个 Tablet 以方便存储。

Tablet 是一个存储单元。在 BigTable 的架构中，是由一个 Master Server 和多个 Tablet Server 组成的。而 Tablet 这一层抽象有些类似传统数据库概念中的 sharding，跟在 HBase 中的 Region 是差不多的思想。Tablet 的读写是由 Tablet Server 来处理的。

SSTable 是一种文件的格式，全称是 "Sorted Strings Table"，是指按照 Key 的排序在文件中存储 <Key, Value> 对。在逻辑上 SSTable 会包含多个 Block，每个 Block 为了方便寻址会有一个 index，所有的 Block index 会写在 SSTable 文件的最后，每当 SSTable 文件被 open 的时候，会将索引加载到内存里，这样每次 Lookup 的时候只有一次硬盘读取。SSTable 也支持全部读取到内存里，这样在 Lookup 的时候没有任何硬盘的读取。这个跟 HBase 中 HFile 的文件格式实现很相似。

> Describe what will happen when a read operation or write operation arrives.

读取和写入是以 Tablet 作为一个 Unit 进行的。

在进行写操作的时候，Tablet Server 会先做一些检查，保证请求的合法以及权限问题。权限的检查是通过检查一个在 Chubby 中的列表进行的，这个列表会被 Client Library 缓存住。一次被允许的写操作会先进入 Commit Log，在处理 Log 的时候采取了批处理来提高吞吐。在操作被 Commit 后，它的内容会被插入 MemTable 里，当 MemTable 的 size 超过一个阈值的时候，会让当前的 MemTable 进入一个 frozen 的状态，随后创建一个新的 MemTable，Frozen 的 MemTable 就可以以 SSTable 的形式写入 GFS。

在进行读操作的时候，Tablet Server 会在做了一些检查保证合法后，在 MemTable 和 SSTable 的一个 merge 后的 view 中来进行读操作，这样可以保证可以读到最新的值。

> Describe which applications are BigTable suitable for and not suitable for.

BitTable 适合那些对可用性要求比较高的业务场景，同时对于跨行的事务性没有要求的应用。但是正因为没有跨行事务的支持，所以我觉得引用场景很局限。目前在谷歌内部应该也逐渐被 Spanner 和 F1 所取代吧。

#### Dynamo

[Dynamo: Amazon’s Highly Available Key-value Store](http://s3.amazonaws.com/AllThingsDistributed/sosp/amazon-dynamo-sosp2007.pdf)

```
// TODO Wait to read
```

#### Spanner

[Spanner: Google’s Globally-Distributed Database](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/spanner-osdi2012.pdf)

> What is external consistency? What’s the difference between external consistency and serializability?

External consistency 在文中的描述是：如果事务 2 发生在事务 1 提交之后，那事务 2 的时间戳要比事务 1 提交的时间戳要大，也就是 linearizability。

Serializability 是数据库隔离性上的一个级别，这意味着数据库中所有的事务都是可以被序列化来执行的，只有完全没有冲突的事务才可以并发地执行。

External consistency 是一致性上的概念，在分布式场景下，external consistency 是更难做到的，因为时序对时间的精度要求很高，在分布式场景下，有可能出现因为不同机器系统时间不一致导致事务 2 拿到一个比事务 1 提交的时间戳更小的时间戳。

Serializability 是隔离性上的概念，如果做到了 external consistency，就一定可以做到 serializability。

> How does Spanner achieve the external consistency?

Spanner 之前的 Percolator 和 Spanner 都是使用全局的时钟来解决 external consistency 的。但是 Spanner 创新地使用了原子钟和 GPS 来作为全局的时钟，以此来实现 external consistency。

在事务的执行中，Spanner 会保证，每个事务的 commit timestamp 都会在其 start 和 commit 之间。Spanner 依赖的底层容器集群系统 Borg 会维护一个 True Time API，这个 API 会返回精度为 ε 的时间区间 `[t - ε, t + ε]`。因此每个事务会在 start 和 commit 的时候分别调用一次 True Time API，拿到两个时间区间 `[t1 - ε,t1 + ε]` 和 `[t2 - ε,t2 + ε]`，因此在区间 `[t1 + ε,t2 - ε]` 之间的时间都是可用的，如果 t1 和 t2 很接近，那最多需要等 2ε。

> What will happen if the TrueTime assumption is violated? How the authors argue that TrueTime assumption should be correct?

这会导致 True Time API 不能再用来保证 external consistency，文章中提到，CPU 造成的问题比时钟问题多六倍，因此跟硬件造成的错误相比，时钟造成的问题不值一提，可以被视为是值得信任的。

#### Distributed Transaction with RDMA and HTM

[Fast In-memory Transaction Processing using RDMA and HTM, Xingda Wei, Jiaxin Shi, Yanzhe Chen, Rong Chen, Haibo Chen SOSP 2015](https://pdfs.semanticscholar.org/408b/8d34b7467c0b25b27fdafa77ee241ce7f4c4.pdf)

>How does DrTM detect conflicts between remote reads and local writes?

在检测事务的冲突上，DrTM 使用了 HTM 和 RDMA 两种技术，HTM 是一个硬件的特性，在硬件级别提供了有限的事务性内存的支持。RDMA 是 Remote Direct Memory Access，提供了远程直接访问内存，不阻塞 CPU 的操作。

在处理事务冲突时，是在 transaction layer 去做的。对于远程的读和本地的写操作引起的冲突，最简单的方法是用 RMDA 锁住一个 remote record，不管是读还是写。但是这样会大大降低并行性，因此文章进行了一些改进，引入了基于租约的锁，来保证读共享。而在读和本地写产生冲突时，读会通过 RDMA abort 本地的 HTM 事务，从而避免冲突。

> Why DrTM has the deadlock issue and how does it avoid the deadlock?

首先，在 DrTM 的 fallback handler 中，不能像传统的实现那样，一个简单的锁就可以解决问题，而是 fallback handler 通过 2PL，对于任何 record 都是以 remote 的形式进行访问。这里就有可能产生死锁。因为涉及到 remote locks 的顺序。

为了避免这个问题，DrTM 声明了一个全局的释放和申请锁的顺序，避免了死锁的问题。

> What’s the limitation of DrTM?

首先，DrTM 没有做到很好的可用性，这是它最大的局限性。还有就是需要硬件特性的支持，导致在很多现有的硬件上没有办法完全复刻 DrTM 的工作，而需要一些适配性的工作。

#### Key-Value Store with RDMA

[Using One-Sided RDMA Reads to Build a Fast, CPU-Efficient Key-Value Store, Christopher Mitchell, Yifeng Geng, Jinyang Li USENIX ATC 2013](https://www.usenix.org/system/files/conference/atc13/atc13-mitchell.pdf)

> What makes in-memory key-value store systems suitable for RDMA optimization?

因为 In Memory 的 Key-value Store 的大多数请求都是读操作，因此对于 RDMA 来说，这样的特点使得其实现比较简单，只需要对 get 请求做修改就可以了，这样既可以利用 RDMA 的优点，又不需要对系统做过多的修改。

> How does Pilaf ensure the data consistency of 'get' operations? Does it have any problems?

利用了一个『Self verifying』的数据结构，它包括一个 root 和很多 pointer，然后会记录一个 checksum。client 通过检查 checksum 可以检测到读写不一致。当遇到了数据竞争时，client 会自动地进行重试操作。

文中提到有两个应用场景会有问题，一个是在 server 修改 hash table 的时候 client 也在读 hash table，这会导致 client 从不合法的内存地址读取内容。

另外是 client 的指针引用可能非法的。比如当 server 在删除一个 key-value 对的时候，client 自身维护的引用就会已经是失效的。

> Why does Pilaf need two round-trips to perform a 'get' operation? Can we reduce the number of round-trips to one using existing RDMA operations? Why/How?

因为涉及到两次读操作，一次是读哈希表，一次是读真正的 key-value 的内容。可以考虑合并两个内存块，但是这样应该会使得可以存储的空间变小。

#### RDF Store with RDMA

[Fast and Concurrent RDF Queries with RDMA-based Distributed Graph Exploration, Jiaxin Shi, Youyang Yao, Rong Chen, Haibo Chen, Feifei Li OSDI 2016](https://www.usenix.org/system/files/conference/osdi16/osdi16-shi.pdf)

> What are the bottlenecks of existing RDF systems?

一共有两种不同的实现，分别是 Triple store and triple join 和 Graph store and graph exploration。前者是以 triple 的方式来将 RDF 数据存储在关系型数据库中，因此查询有两个步骤，scan 和 join。scan 会分为子查询，最后再借由 hash join 之类的 join 的操作将查询的结果 join 在一起。由此可知如果数据非常大的时候，最后的 join 会是很大的问题。

第二种方式是以图的方式来存储和查询 RDF。这样的方式以 Trinity.RDF 为代表，有一些剪枝的优化。但是最后也会有一个 final join 的过程。

纵观之前的实现，最后的 join 是一个最大的问题。

> What are the differences between Wukong and prior graph-based designs? What are the benefits?

最大的不同在于索引的存储方式。之前的基于图的设计都是用独立的索引数据结构来存索引，但是 Wukong 是把索引同样当做基本的数据结构（点和边）来存储。并且会考虑分区来存储这些索引。

这样做有两个好处，第一点就是在进行图上的遍历或者搜索的时候可以直接从索引的节点开始，不用做额外的操作。第二点是这样使得索引的分布式存储变得非常简单，复用了正常的数据的存储方式。

> What is full-history pruning and what's the difference compared with the prior pruning approach? Why can Wukong adopt full-history pruning?

Full-history 就是说所有的历史记录都会被记录下来。之前是只记录一次的。之所以可以这样做是因为一方面 RDF 的查询都不会有太多步，而且 RDMA 在低于 2K bytes 的时候性能都是差不多的，所以 Wukong 可以这样做。

#### Ambry

* [Ambry: LinkedIn’s Scalable Geo-Distributed Object Store](http://dprg.cs.uiuc.edu/docs/SIGMOD2016-a/ambry.pdf)
* [LinkedIn 开源其分布式对象存储系统 Ambry](http://www.infoq.com/cn/news/2016/06/LinkedIn-Ambry)
* [Slides by me](https://docs.google.com/presentation/d/1o1lkn_QmsDvnHfETHmPUgb-nG3hoRXiXqtn09jAV1XI/edit?usp=sharing)

Ambry 是一个针对 Media 的分布式对象存储系统，是 LinkedIn 做的一项工作。LinkedIn 作为一个社交产品，会有很多媒体数据需要处理，在之前是采取了文件系统存数据，Oracle 存元数据的方法。随着规模的扩大发现不行，于是就有了这个系统。

Ambry 是一个比较中规中矩的分布式系统，比较让人印象深刻的只有一些 threshold。因为社交数据有一个特性：越是冷的数据会越冷，因此 Ambry 针对这个观察做了一些优化，同时在负载均衡上用了 threshold + round-robin 的方式，很 simple 但是效果不错。除此之外在数据存储上有分层的概念在里面，索引是逐层进行的。最上面是 bloom filter，然后是顺序的 segment，最下面是 partition 里真实的数据。

文章很简单，算是很多工程上的点拼在了一起写的论文，有点像最近读的 F2FS，此外 Ambry 感觉也参考了很多文件系统的设计，有一些共性在里面。比如 log structured update 和 journal 等等。

## 虚拟化(Virtualization)

### 虚拟机管理器(Hypervisor)

#### Xen

* [Xen and the Art of Virtualization](http://www.cl.cam.ac.uk/research/srg/netos/papers/2003-xensosp.pdf)
* [CSP 课堂笔记之 Hypervisor](http://gaocegege.com/Blog/csp/xen-kvm)

Xen 是非常著名的 Hypervisor，它提出了 para-virtualization 的想法。之前实现虚拟机，都是通过 full-virtualization 的方式，但是那个时候的 X86 其实并不能很好地支持 full-virtualization。举个例子，有些指令原本是应该在 VMM 中被执行的，但是有时会因为指令在不同 ring 有不同的表现，因此并不能成功地 trap 到 VMM 中。为了更加优雅地解决这样的问题，Xen 引入了 hypercall，修改了 Guest 的 OS。通过另外的方式来解决这个问题。

虚拟化最主要的资源是 CPU，Memory 和 IO，Xen 对于三者都有一些比较有趣的地方。其中我觉得最有趣的是对于设备的支持，引入了 Domain 0，前后端驱动的设计让人觉得很自然，而且也避免了把驱动放在 VMM 里，会因为驱动 bug 把 VMM 弄崩的可能。

#### kvm

* [kvm: the Linux Virtual Machine Monitor](https://www.kernel.org/doc/ols/2007/ols2007v1-pages-225-230.pdf)
* [CSP 课堂笔记之 Hypervisor](http://gaocegege.com/Blog/csp/xen-kvm)

```
// TODO Add the notes
```

### 容器(Container)

#### mbox

* [Practical and effective sandboxing for non-root users](https://people.csail.mit.edu/nickolai/papers/kim-mbox.pdf)
* [Open source in GitHub](https://github.com/tsgates/mbox)

```
// TODO Add the notes
```

#### Slacker

* [Slacker: Fast Distribution with Lazy Docker Containers](https://www.usenix.org/system/files/conference/fast16/fast16-papers-harter.pdf)

```
// TODO Wait to read
```

## 沙箱(Sandboxing)

[Sandboxing in Linux: From Smartphone to Cloud](http://www.ijcaonline.org/archives/volume148/number8/borate-2016-ijca-911256.pdf)

沙箱跟容器其实是有点血缘关系的，要做容器肯定要实现隔离，而沙箱就是专门做隔离的。之所以把他们两个分开介绍是因为沙箱本身是一个很复杂的方向，有很多的种类，而容器只是使用了沙箱技术中的某几种。

沙箱技术大致可以被分为两类，其中第一类是基于隔离的沙箱，该类型的沙箱将应用的执行环境从操作系统中隔离出来，形成一个独立的执行环境。第二类是基于规则的沙箱，该类型的沙箱并不是完全关注于对于应用程序的隔离上，而是用规则的方式控制每个应用的权限，基于规则的沙箱之间可以分享操作系统的逻辑资源。

我读的论文主要都是第一类的沙箱，其中主要的技术是 capabilities, system call interposition, software-based fault isolation 等。

上面的链接是一个综述性质的文章，主要阐述了 Linux 平台上可以用来实现沙箱的内核 feature。它对沙箱的定义和功能介绍的比较简单易懂，不妨一读。

###  系统调用拦截(System Call Interposition)

System Call Interposition，顾名思义，就是拦截和过滤系统调用的技术。在沙箱的实现过程中，系统调用是一个很关键的部分。如何能够保证应用只能进行被授权的系统调用，是这个方向的研究做的事情。

#### Janus

* [A Secure Environment for Untrusted Helper Applications(Confining the Wily Hacker)](https://www.usenix.org/legacy/publications/library/proceedings/sec96/full_papers/goldberg/goldberg.pdf)
* [Janus: an Approach for Confinement of Untrusted Applications](http://www2.eecs.berkeley.edu/Pubs/TechRpts/1999/CSD-99-1056.pdf)
* [Traps and Pitfalls: Practical Problems in System Call Interposition Based Security Tools](http://www.isoc.org/isoc/conferences/ndss/03/proceedings/papers/11.pdf)

链接中的第一篇论文是 1996 年发表的，是 system call interposition 方向上最经典的论文，它提出了一个系统，Janus。这个工具可以根据用户定义的 policy 来对应用的请求调用进行过滤，后面的两篇都是后续的关于 Janus 的论文。

在那个时候，互联网很流行，在互联网上获得的内容，可以直接用本地的 helper application 打开。因为内容本身是不可信的，所以这样的一种模式连带着 helper application 也是不可信的，因此 Janus 希望对这样的应用进行限制和隔离，使得应用具有最小的特权，当其被恶意程序攻击后，不会影响整个操作系统。

Janus 的目标有三点，第一个是安全不多说，第二个是灵活，就是要求对系统调用的限制可以达到参数级别，比如 open 在某些参数时可以被调用，其他就不可以，还有就是可配置，允许为不同应用配置不同的策略。

它的实现比较简单，是借助了内核中的 ptrace，然后实现了一个 kernel module 以及一个用户态的 engine。在开始的时候，Janus 会先读取 policy 文件，然后会 fork 子进程，然后父进程会 select 关于子进程的各种事件。子进程会执行 helper app 的逻辑，当有了一个系统调用，会先给 Janus 的内核模块处理，内核模块会跟用户态的 engine 交互，确定请求是不是合法的，合法会交由内核去处理，否则会 deny 掉请求。

下面的两篇论文中提到了一些关于 Janus 的缺点，包括 ptrace 和 Janus 本身的缺点，主要是涉及一些 system call 的监控问题，以及 deny 的方式问题，就不再说了，已经很长了。

#### Ostia

[Ostia: A Delegating Architecture for Secure System Call Interposition](http://benpfaff.org/papers/ostia.pdf)

Ostia 是在 Janus 等等那一溜论文后面发表的，因此引用了 Janus 中提到的那三篇论文。它最大的贡献，在于提出了一种新的架构，然后解决了之前的基于 filter 的架构不能解决的问题。

它跟 Janus 其实是一挂的工具，都是要在内核态修改一些东西来做的。但是两者的不同在于实现的架构，Janus 是在内核中要有一个负责 tracing 的模块，比如 ptrace。然后在用户态有一个 policy engine，两者会交互，其中是否 deny 请求的逻辑在 policy engine 中，而内核中的模块主要是做 process monitor 的。

Ostia 的实现在我看来参考了虚拟化的一些思想，当一个系统调用到内核的时候，会回调在调用者用户态内存空间中的一个 handler，然后会转发给 agent，然后 agent 会负责校验权限和访问。具体的实现还在看。

### 软件故障隔离(Software-based Fault Isolation)

#### SFI

[Efficient Software-Based Fault Isolation](https://crypto.stanford.edu/cs155/papers/sfi.pdf)

这篇文章是在 1993 年发表的，也正是这篇文章最早提出了 "sandboxing" 一词。这篇文章主要是介绍了用软件方式来实现隔离的方法。传统的隔离是在操作系统这一层来做的，以前进程之间可以通过 RPC 的方式进行，而隔离是通过虚拟内存来实现的。但是这样做的 overhead 特别大，因此这篇文章想实现在同一内存空间内的错误隔离。SFI 的实现，使用了处理器的段寄存器。段寄存器最初是为了解决 Intel 8086 处理器体系架构中数据总线与地址总线的宽度不一致而引入的，因为不一致导致寻址不能在单个指令周期内完成，因此 Intel 引入了段寄存器，将整个内存空间分为了4个段，段寄存器存放每个段的前 N 位的地址，使用段内偏移量，而不是全部的物理地址来描述内存地址，解决了这一问题。在目前处理器的架构中，不再存在地址宽度不统一的问题，因此内存分段成为了可选的特性。而 SFI 通过对段寄存器的访问限制，将程序的控制流严格地限制在一个 Fault Domain 的代码段中。在进行控制的跳转时，会强制使用段寄存器进行检测，如果访问的地址的前 N 位与段基地址不一致，则会触发异常，说明应用程序正在尝试逃出自己的 Fault Domain。

#### Google Native Client

* [Native Client: A Sandbox for Portable, Untrusted x86 Native Code](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/34913.pdf)
* [论文笔记 in GitHub](https://github.com/gaocegege/NaCl-note)

这篇论文是在 CSP 课上读的， 因为需要做分享，所以读的相比于其他论文要仔细一些，之前在阅读的时候就写了一些阅读笔记，比较冗长，这里写一些这篇论文的大概 idea 的介绍，以及一些自己的评论。

Google Native Client(NaCl)，简单来说是一个在浏览器里跑 Native 代码的技术。类比技术是微软<del>臭名昭著</del>的 ActiveX。相比于 ActiveX 那种毫无安全性可言的实现，NaCl 使用了一些自己改良过的 Software Fault Isolation(SFI) 的技术，结合了 ptrace 这样的 System Call Interception 的工具，来实现了在浏览器里安全运行 Native 代码的功能。从实现角度来看，是先对代码进行静态检查，保证代码符合 NaCl 制定的一些规则，然后再把程序运行在一个沙箱内，Native 代码所有跟外界的通讯，包括系统调用，都会被封装或者拦截，使用这样的方式来实现了对 Native 代码的安全隔离。在 2009 年的时候，Google 组织了一个 Native Client Security Contest，鼓励开发者寻找 NaCl 的漏洞，最终发现了 20 多个漏洞但是没有一个可以从根本上破坏 NaCl 的保护。目前 Google Chrome 浏览器仍然支持以这样的方式来运行 Native 代码，只不过好像没有多少人在用的样子。Demo 很容易运行，感兴趣可以试一下，很简单就可以实现从 CPP 代码到 Javascript 代码的通信。

为了提高浏览器段代码运行的效率，还有另外一个流派的做法，那就是 [asm.js](http://asmjs.org/)，它的实现思路跟 NaCl 完全不一样，并不会在浏览器里执行 Native 代码，因此不会有这么多安全方面的问题需要考虑，而是通过修改 LLVM 的那一套工具链，把 Native 代码编译成 Javascript 的一个子集，运行这个子集的 Javascript 代码。这样的实现最高可以只比 Native 应用慢一倍，虽然不如 NaCl 媲美原生应用，但是也可以接受了。这是 Firefox 浏览器的路子。

目前业界有了相对统一的思路，WebAssembly。WebAssembly 跟 asm.js 是相同的路子，得到了各种公司的支持。WebAssembly 由 asm.js 的团队和 NaCl 的团队一起开发，NaCl 的团队更多关注在安全上，这也是他们的特长。因此可以说，Native Client 这个 feature 大概是被抛弃了，但是一些安全的实现，还是由原班人马在为 WebAssembly 做贡献。其实这篇论文的主题也不是说 Native Client 的实现有多好，而是强调它是怎么做到安全的，讲道理这样的实现方式，我觉得是 asm.js 那个流派比较好，因为侵入性小一点。

#### Language-Independent Sandboxing

[Language-Independent Sandboxing of Just-In-Time Compilation and Self-Modifying Code](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.207.6665&rep=rep1&type=pdf)

```
// TODO Add the notes
```

## 系统(System)

### 文件系统(File System)

#### RAMCloud

[Fast Crash Recovery in RAMCloud](http://web.stanford.edu/~ouster/cgi-bin/papers/ramcloud-recovery.pdf)

>Why RAMCloud uses a log-structured strategy for data in DRAM?

RAMCloud 在备份上，没有完全使用内存做备份，而是在内存中有一个备份，在硬盘上也有一个备份。这样的备份策略省了内存，但是造成了两个问题：

* 备份相对慢速，可能会影响系统的正常操作
* 系统 crash 后恢复要从硬盘中开始，会比较慢

Log-Structured Strategy 就是为了解决第一个问题而采用的。请求到了内存里是以日志的形式记录，随后会将 entry 分发给各个备份，各个备份在收到后会直接返回，然后再处理，约等于实现了异步的操作。因此备份时的 overhead 被 hide 了，但是这样引入了新的问题，可能导致系统 crash 后没有把 buffer flush 到真正的存储中。软件不行硬件来凑，文中提到了两种方法来保证 entry 到了 buffer 中就是持久化的，一种是 DIMM + super-capacitor，一种是加电池。

综上，日志主要是为了解决同步备份的时候因为 hierarchy 产生的性能问题。

>Which policy does RAMCloud use to place segment replicas and how to find the segment replicas during recovery?

以往的实现是中心化的 coordinator，这样会造成性能瓶颈。所以 RAMCloud 采纳了去中心化的思想，利用了随机化和微调的方式，来分散备份。有些类似 k choises 的选择，RAMCloud 会先随机选择一些，然后再从中选出最合适的，并且加入了 reject 的机制来保证在乐观并发的情况下不会产生竞争的问题。同时为了保证尽可能贴近最优解，RAMCloud 考虑了硬盘速度以及硬盘上已经有的 segment 的数量来进行微调，保证尽可能的均匀。

recovery 的时候以往的实现是在 coordinator 里维护一个中心化的表，这样的做法向上面提到的一样会造成瓶颈，因此 RAMCloud 在 recovery 的时候会问所有的备份，备份会返回一个它存储的副本列表，整个过程是并行的而且 RAMCloud 用了自称 fast 的 RPC，因此整个过程不会特别慢。

>Does RAMCloud support random access? If so, please explain how.

支持不支持，支持。

RAMCloud 在每个 master 上都维护有一个哈希表，结构为 `<table identifier, object identifier>`，通过哈希表，可以进行随机的访问。

#### Optimistic Crash Consistency

[Optimistic Crash Consistency](http://research.cs.wisc.edu/adsl/Publications/optfs-sosp13.pdf)

这是一篇发表在 SOSP'13 上的论文，主要的工作是在 Ext 4 这样的基于 Journal 来做 Crash Recovery 的文件系统的基础上实现了一种 Optimistic 的 Crash Recovery 的方法，这样的方法能够在保证 Crash Consistency 的同时，大幅度提高性能。但是计算机所有的提高都是 tradeoff 的结果，Optimistic Crash Consistency 是牺牲了数据的 Freshness，也就是新鲜度。

Crash Consistency，就是指文件系统在 Crash 之后，其中的数据还是不是一致的。这里的一致指的是 metadata 和 data 等等数据之间的一致。如果不一致的情况发生了，往往意味着硬盘丢了数据，或者文件系统找不到硬盘上的数据。

在基于 Journal 的文件系统中，一次写入磁盘的操作，要写入的数据有 data，以及在 Journal 中的一份 metadata 的冗余，还有 Journal 中的 commit block，以及最后的 metadata。这四个数据要保证写入顺序才能确保 Crash Consistency。因此要保证数据写入的顺序，就要借助磁盘的 flush 操作，来强制地把数据从磁盘的 cache 刷到真正的磁盘中才行。但是这样会导致很大的性能问题，这篇论文提出了使用 checksums，asynchronous durability notifications，delayed writes 等技术来使得文件系统不需要强制的 flush 操作。但是这样的实现，就会牺牲掉数据的 Freshness，在之前 Ext 4 的实现中，Crash 之后最多丢一个 transaction 的数据，现在可能丢 k 个。但是在性能上比带 flush 的 ext 4 提高了 4-10 倍。

#### F2FS

[F2FS: A New File System for Flash Storage](https://www.usenix.org/system/files/conference/fast15/fast15-paper-lee.pdf)

随着 NAND Flash 的发展，现在很多持久存储都渐渐地变成了 SSD。之前的文件系统都是针对 HDD 来设计的，因此并没有针对 FLash 存储的一些硬件特性进行优化，而本文则是提出了一个为了 Flash 存储设计的文件系统，现在以及被并入了 Linux Kernel。整篇论文是很多比较偏工程的点拼接起来的，因此读起来不像是其他文件系统的论文那么晦涩。

Flash 存储在读上面的速度众所周知非常快，而且它并不是像 HDD 那样的机械结构，用磁头来进行读写，而是电子的，因此像内存一样拥有一定的并行性。与此同时，在写数据时，Flash 存储并不是 in place 的写入，而是需要写入一个新的地方，然后修改地址使其生效。这一点非常重要，它导致了 “Wandering Tree“ 的问题，这是 Log structured fs 在 SSD 上存在的一个问题：因为 Flash 存储的写是 out of place 的，因此每次写操作都会使得数据块的地址发生变化，所以需要递归地修改 direct block, indirect block 等等一系列块的内容。为了解决这个问题，F2FS 引入了一个新的表：Node Address Table(NAT)。Indirect block 是指向 NAT 的，这样每当一个 data block 被污染的时候，会更新 direct block 和 NAT 中的表项，这样就防止了在 indirect node block 中的传播。

同时为了利用 Flash 存储的并行性，F2FS 采取了 multi-head logging，并不只有一个 log，而是有多个 log，根据数据的更新频率来写 log，这样提高了性能。

#### PMFS

* [System software for persistent memory](http://esos.hanyang.ac.kr/files/courseware/graduate/System_software/a15-dulloor.pdf)
* [Slides](http://embedded.dankook.ac.kr/~choijm/course/201501TOS/0519_PMFS.pdf)

现在有一种新的概念，Non-volatile Memory（非易失性内存）。它是指通过给内存加电容等等方式来实现内存持久性的一种方式。而也有很多研究是基于此来设计文件系统，这篇文章就是这样的一个文件系统。

很惭愧，这篇文章没怎么听课，也没怎么好好读，不过可以讲讲大致的实现。它最让人印象深刻的地方是在保证 Consistency 的时候，使用了 Hybrid 的方式，综合使用了 COW，Journaling 等等方式。还有就是引入了一个新的硬件 primitive，读的不深就不瞎说了。

### 操作系统(Operating System)

#### Exokernel

[Exokernel: An Operating System Architecture for Application-Level Resource Management](https://pdos.csail.mit.edu/6.828/2008/readings/engler95exokernel.pdf)

```
// TODO Add the notes
```

### Memory

#### Transactional Memory

* [Transactional memory: architectural support for lock-free data structures](http://www.cs.utexas.edu/~pingali/CS395T/2009fa/lectures/herlihy93transactional.pdf)

事务性内存已经不是一个新鲜的概念了，从大二以来就一直听说这个词，但一直没有了解过相关的内容。这篇论文是 93 年的，应该是比较老的一篇关于硬件支持事务性内存的论文。而现在在 Intel Haswell 的 CPU 架构中已经有了对事务性内存的支持，但是大家还是有些相关性的。

```c
// Critical Section
x=VALIDATE;
if (x) {
      COMMIT;
} else {
      ABORT;
}
```

上面的代码是事务性内存的一个编程范式，原本是使用锁来进行同步，现在所有的操作可以被理解为是一个事务，只有在事务提交的时候才会生效，而如果遇到了冲突则会被 Abort，所有修改完全被 Discard。

本文是基于嗅探的缓存一致性协议修改来的，文章中在硬件上实现这一点的时候加入了一个新的 L1 Cache，叫做 Tx Cache。因此非事务修改走 regular cache，事务走 Tx Cache。

事务缓存有四个标签，Normal, XCommit, XAbort, Empty。事务提交的时候 XCommit -> Empty, XAbort -> Normal。事务 Abort 的时候，XAbort -> Empty, XCommit -> Normal。

它其实是利用了现存的缓存一致性协议，对于冲突的判断，都是用它来完成的。要理解这篇论文就必须对缓存一致性有比较清楚的认识。而因为实现利用了 Cache，所以在发生 Context Switch 等需要 Flush Cache 的情况下时，事务是一定会被 Abort 的，所以这篇文章的实现主要面向的场景是 short critical section，同时没有 IO 读写。

不过，实现因为是完全基于现有的实现（缓存一致性），因此可以跟非事务性内存的同步操作一起进行，因为其他不在事务中的操作是一样会触发缓存操作的。这算是一个这样实现带来的好处，很关键。

### CFI

#### Control-Flow Integrity

[Control-Flow Integrity. Principles, Implementations, and Applications](https://www.microsoft.com/en-us/research/wp-content/uploads/2005/11/ccs05.pdf)

```
// TODO Add the notes
```

### Tail Latency

#### The Tail at Scale

[Dean, Jeffrey, and Luiz André Barroso. "The tail at scale." Communications of the ACM 56.2 (2013): 74-80.](https://cacm.acm.org/magazines/2013/2/160173-the-tail-at-scale/abstract)

> Why latency variability is amplified by scale?

这是受到系统的特性影响的。在文中给出的例子，是假如一个用户访问一个服务器，99% 的请求是在 10ms 内被处理完的，而有 1% 的请求是在 1s 内被处理的。在这种情况下，针对一个服务器的请求还是可以接受的。但是现在绝大多数应用都是需要多个服务共同服务最后才返回给终端用户的，因此在这样的情况下，用户感知到的延迟是所需要的服务中延迟最大的值。因此，在这样的情况下，用户感知到的延迟是随着系统规模的增大而提高的。这里的 scale 指的是系统的规模。

Please briefly outline some effective tail-tolerant techniques.

比如最简单的，使用多台服务器，处理完全相同的逻辑，用 replica 的方式降低延迟。用户的请求会发送给所有的服务器，当任何一个服务器处理完请求，就返回。这样的方式要求应用要自己处理幂等请求，逻辑也会变得复杂一些。在之上可以做一些优化，比如在一个服务器处理完请求后，通知其他服务器终止对幂等请求的处理等等，不过这样也更加增加了实现的难度。

还有一些减少 daemon 进程的方法比如使用 unikernel 等等更加精简的内核，但是这样的做法成本就更高了。

Why tolerating latency variability for write operations is easier?

因为一般写都是异步的，而且可以容忍一定的不一致。以及在一致性的写上一般都会使用 Paxos, ZAB, raft 之类的算法。而这些算法本身就是 tail tolerant 的。

### Lock

#### Non-scalable locks are dangerous

[Non-scalable locks are dangerous, Silas Boyd-Wickizer, M. Frans Kaashoek, Robert Morris, and Nickolai Zeldovich Proceedings of the Linux Symposium. 2012.](https://www.kernel.org/doc/ols/2012/ols2012-zeldovich.pdf)

>Why does the performance of ticket lock collapse with a small number of cores? (Hint: cache coherence protocol)

在 ticket lock 的实现中，是维护两个 ticket 变量，当前 ticket 和 next ticket。在 unlock 操作中是将当前 ticket 自增从而使得下一个拿到了 ticket 的 core 获取到了锁。

凡是遵循文章中提出的硬件缓存一致性模型的系统，都会遇到 ticket lock 的 rapid collapse。文章提出的硬件缓存一致性模型用目录的方式类比了 CPU 中的缓存，并且用马尔可夫链模型，对 ticket lock 的问题进行了分析。其中每一个节点代表有几个 core 在等的状态，Arrival and Service Rates 分别代表了在不同状态下锁的获得和释放。其中到达率跟在没有在等的 core 的数量 (n - k) 成正比，服务率和在等的 core 的数量 (k) 成反比。所以随着 k 的增大，服务率是减小的。这使得模型得到了一个数学上的结论，锁的获取时间和正在等待锁的 core 的数量是成正比的。

因此随着 core 的增加，在 serial section 很小的时候，Sk = 1/(s + ck/2) 中 k 对 Sk 的贡献越大，因此越容易受到 k 的影响。这也就是为什么，在 serial section 很小的时候，只是多了很少一些在等待锁的 cores，就出现了性能的雪崩。

>Why does MCS lock have better scalability than ticket lock?

在 ticket lock 中，所有的 core 都去依靠同一个变量来获得锁，而在 MCS lock 中，前一个 core 在 release 的时候会修改下一个 core 的 locked 变量，通知下一个 core 来获得锁。这样每个锁都只依靠属于自己的一个变量，这样的实现是缓存一致性无关的，有着更好的 scalability。

### Bug

#### STACK

[Towards Optimization-Safe Systems: Analyzing the Impact of Undefined Behavior](https://pdos.csail.mit.edu/papers/stack:sosp13.pdf)

Undefined behavior 是编程语言规范对某段代码可能产生的某些执行结果未定义。Unstable code 就是在程序实际的执行过程中，由于涉及到undefined behavior，从而无法被编译器翻译（直接略过）的代码段。

STACK会在Assumption Δ被允许和不允许的情况下分别模拟编译。

* 先模拟假设不成立的情况进行一次编译；
* 模拟假设成立的情况进行一次编译；
* 查看前两步的执行结果有没有区别，有区别的地方就是 unstable code。

如果执行第二步时得不到准确的结果，那么会漏报一些unstable code；如果执行第一步时得不到准确的结果，就会产生误报(false warning / false positive)。目前stack给出的Undefined behavior pattern 可能不齐全。

对于程序员来说，通过fix bug或者去掉一些会被编译器当做是undefined behavior的代码；对于编译器来说，可以集成一些现有的bug-finding的工具，或者利用STACK的方式来判定unstable code；完善编程语言的specification，定义更多的代码执行规则，减少undefined behavior的产生。

STACK为了使可扩展性更高，在计算Δ = ∀e:Reach(e) → ¬Undef(e)的时候做了一些近似运算，使最后得到的结果可能会漏掉一些unstable code。STACK为了简化和滤过某些查询用到的constraint solver如果发生了timeout，也会出现漏报的情况。因此，STACK为了更好的扩展性，牺牲了一定的可靠性（精度）。

## 安全(Security)

### 虚拟机安全(Virtulization, Security)

#### CloudVisor

[CloudVisor: Retrofitting Protection of Virtual Machines in Multi-tenant Cloud with Nested Virtualization](https://www.sigops.org/sosp/sosp11/current/2011-Cascais/printable/15-zhang.pdf)

```
// TODO Add the notes
```

### Taint Tracing

#### TaintDroid

* [TaintDroid: An Information-Flow Tracking System for Realtime Privacy Monitoring on Smartphones](http://www.appanalysis.org/tdroid10.pdf)
* [Realtime Privacy Monitoring on Smartphones](http://www.appanalysis.org/)

Taint 分析，就是指把一些敏感数据标注出来，在程序执行的过程中确保这些被标注的敏感数据不会被泄露出去的技术。TaintDroid 是一个在 Andriod 做 Taint 分析的工具，之前的 Taint 分析工具，overhead 非常大，而 TaintDroid 通过分层的思想，在不同层做不同粒度的 Taint 跟踪，大大降低了运行时的 overhead。

论文有一个配套的 demo，是可以运行的，感兴趣的话可以自己试试看，这里也有一个 [Demo 视频](https://www.youtube.com/watch?v=qnLujX1Dw4Y)。很有趣的是这篇论文是 Intel Labs 有参与的，不是很懂他们怎么会想到做这样的事情。

### ROP

#### Hacking Blind

* [Hacking Blind](www.scs.stanford.edu/~sorbo/brop/bittau-brop.pdf)
* [【转载】Blind Return Oriented Programming (BROP) Attack - 攻击原理](http://ytliu.info/blog/2014/09/28/blind-return-oriented-programming-brop-attack-gong-ji-yuan-li/)

这篇论文看上去就很酷，实现很让人亮眼。最简单的 ROP，就是寻找一个个的 gadget，然后把 gadget 连接起来。然后让控制流走到这些 gadget 里，就 OK 了。但是这篇论文是如何在远程来劫持控制流，来实现 ROP 攻击。攻击者不了解远程的系统，因此首先系统要有一个已知的 stack overflow 的漏洞，然后要求攻击的进程在死了后会重启，而且 ASLR 后的地址不变。

其实条件是很苛刻的，而且也不懂为什么一个攻击者可以在不了解远程系统的同时知道系统的 stack overflow 漏洞。整体攻击的过程，是先 Dump 服务器的内存，然后再进行常规的 ROP，其中 Dump 内存的操作非常精巧，感觉只有 ROP 高级玩家才能想出这样的做法，具体可以看看上面链接的论文，是我们学院 IPADS 实验室的一个学长写的，很清楚。

## 大数据

### 框架

#### Hadoop

* [MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)
* [The Hadoop Distributed File System](http://pages.cs.wisc.edu/~akella/CS838/F15/838-CloudPapers/hdfs.pdf)

```
// TODO Wait to read
```

#### Spark

* [Spark: Cluster Computing with Working Sets](https://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf)
* [Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)

```
// TODO Wait to read
```

### Data Processing

#### Realtime Data Processing at Facebook

* [Realtime Data Processing at Facebook](http://dblp.org/rec/html/conf/sigmod/ChenWIJLSWWWY16)

## 网络

### Network Function Virtualization(NFV)

#### Click

[The Click modular router, Robert Morris, Eddie Kohler, John Jannotti, M. Frans Kaashoek SOSP 1999](https://pdos.csail.mit.edu/papers/click:tocs00/paper.pdf)

> Why must Click provide both push' andpull' methods? Can we eliminate one of the two operations? How/Why?

Push 和 Pull 是两种连接方式。 Push 是从源 Element 到下游的元素。在 Push 连接方式里，上游的元素会提交一个包到下游的元素。Pull 是从目的地元素到上游元素。在 Pull 的连接方式里，是下游的元素发送包请求到上游的元素。Push 和 Pull的共同使用可以使包转发连接的适当终止，可以很好地解决路由器控制流问题。例如包调度的决定——选择哪个队列去请求一个包对于组合的 Pull 元素来说是非常容易实现的。另外，系统不应该向繁忙的转发接口发送包，否则，这个接口就必须存储包，并且路由器会失去处理这些包的能力（丢弃，修改优先级等）。这个约束可以由简单的给转发接口一个 Pull 输入实现。然后这个接口就可以控制包转发，并且可以在它准备好的时候请求包。

> One limitation of Click is the difficulty of scheduling CPU time among pull and push paths. Why is it difficult? What would you do to improve it?

Click 的调度器就是一个 Pull Element，它有多个输出，而只有一个输入。至于 CPU 时间调度的问题， 是说 Click 没办法处理多个设备同时接收或者发送数据的情况。目前的处理方法是 linux 处理大部分这样的调度，剩下的交由 Click 来做。最终所有这些都应该由一个单一的机制来控制。关于 improve 可以把 linux 里相关的逻辑作为一个 Element 引入，不知是否可行。

> How can batch processing be applied to Click? Be specific and consider the impact on latency and throughput.

APSys 2012 上有一篇论文『The Power of Batching in the Click Modular Router』，它尝试了 psio，netmap，PF_ring 等等开源的 IO batching 的工具，最后选择了 psio。同时为了实现计算的 batching，它修改了现有的 Click。

这样做提高了 throughput，但是加大了 latency。这也跟 batching 的程度有关，理论上来说是可以控制的。
