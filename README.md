# 论文笔记

## TOC

   * [论文笔记](#论文笔记)
      * [分布式(Distributed System)](#分布式distributed-system)
         * [调度器(Scheduler)](#调度器scheduler)
            * [Omega](#omega)
            * [Borg](#borg)
            * [Mesos](#mesos)
            * [Yarn](#yarn)
         * [Lock Service](#lock-service)
            * [Chubby](#chubby)
         * [一致性(Consensus)](#一致性consensus)
            * [Raft](#raft)
            * [Zookeeper](#zookeeper)
            * [Paxos](#paxos)
         * [Storage](#storage)
            * [Dynamo](#dynamo)
            * [Spanner](#spanner)
      * [虚拟化(Virtualization)](#虚拟化virtualization)
         * [虚拟机管理器(Hypervisor)](#虚拟机管理器hypervisor)
            * [Xen](#xen)
            * [kvm](#kvm)
         * [容器(Container)](#容器container)
            * [Google Native Client](#google-native-client)
            * [mbox](#mbox)
      * [系统(System)](#系统system)
         * [操作系统(Operating System)](#操作系统operating-system)
            * [Exokernel](#exokernel)
         * [CFI](#cfi)
            * [Control-Flow Integrity](#control-flow-integrity)
      * [安全(Security)](#安全security)
         * [虚拟机安全(Virtulization, Security)](#虚拟机安全virtulization-security)
            * [CloudVisor](#cloudvisor)
         * [ROP](#rop)
            * [Hacking Blind](#hacking-blind)

## 分布式(Distributed System)

### 调度器(Scheduler)

#### Omega

[Omega: flexible, scalable schedulers for large compute clusters](http://web.eecs.umich.edu/~mosharaf/Readings/Omega.pdf)

```
// TODO Add the notes
```

#### Borg

[Large-scale cluster management at Google with Borg](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/43438.pdf)

```
// TODO Add the notes
```

#### Mesos

[Mesos: A Platform for Fine-Grained Resource Sharing in the Data Center](https://people.eecs.berkeley.edu/~alig/papers/mesos.pdf)

```
// TODO Add the notes
```

#### Yarn

[Apache Hadoop YARN: Yet Another Resource Negotiator](https://www.sics.se/~amir/id2221/papers/2013%20-%20Apache%20Hadoop%20YARN%20-%20Yet%20Another%20Resource%20Negotiator%20(SoCC).pdf)

```
// TODO Wait to read
```

### Lock Service

#### Chubby

[The Chubby lock service for loosely-coupled distributed systems](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/chubby-osdi06.pdf)

```
// TODO Wait to read
```

### 一致性(Consensus)

#### Raft

[In Search of an Understandable Consensus Algorithm(Extended Version)](https://raft.github.io/raft.pdf)

```
// TODO Add the notes
```

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

###  Storage

#### Dynamo

[Dynamo: Amazon’s Highly Available Key-value Store](http://s3.amazonaws.com/AllThingsDistributed/sosp/amazon-dynamo-sosp2007.pdf)

```
// TODO Wait to read
```

#### Spanner

[Spanner: Google’s Globally-Distributed Database](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/spanner-osdi2012.pdf)

```
// TODO Wait to read
```

## 虚拟化(Virtualization)

### 虚拟机管理器(Hypervisor)

#### Xen

[Xen and the Art of Virtualization](http://www.cl.cam.ac.uk/research/srg/netos/papers/2003-xensosp.pdf)

```
// TODO Add the notes
```

#### kvm

[kvm: the Linux Virtual Machine Monitor](https://www.kernel.org/doc/ols/2007/ols2007v1-pages-225-230.pdf)

```
// TODO Add the notes
```

### 容器(Container)

#### Google Native Client

[Native Client: A Sandbox for Portable, Untrusted x86 Native Code](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/34913.pdf)

```
// TODO Add the notes
```

#### mbox

* [Practical and effective sandboxing for non-root users](https://people.csail.mit.edu/nickolai/papers/kim-mbox.pdf)
* [Open source in GitHub](https://github.com/tsgates/mbox)

```
// TODO Add the notes
```

## 系统(System)

### 操作系统(Operating System)

#### Exokernel

[Exokernel: An Operating System Architecture for Application-Level Resource Management](https://pdos.csail.mit.edu/6.828/2008/readings/engler95exokernel.pdf)

```
// TODO Add the notes
```

### CFI

#### Control-Flow Integrity

[Control-Flow Integrity. Principles, Implementations, and Applications](https://www.microsoft.com/en-us/research/wp-content/uploads/2005/11/ccs05.pdf)

```
// TODO Add the notes
```

## 安全(Security)

### 虚拟机安全(Virtulization, Security)

#### CloudVisor

[CloudVisor: Retrofitting Protection of Virtual Machines in Multi-tenant Cloud with Nested Virtualization](https://www.sigops.org/sosp/sosp11/current/2011-Cascais/printable/15-zhang.pdf)

```
// TODO Add the notes
```

### ROP

#### Hacking Blind

[Hacking Blind](www.scs.stanford.edu/~sorbo/brop/bittau-brop.pdf)

```
// TODO Add the notes
```
