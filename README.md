# 论文笔记

这个 repo 希望能够记录自己阅读论文的过程，其中的论文一部分来自于上课需要阅读的论文，这部分会比较偏安全和虚拟化。还有一部分论文是自己感兴趣，想去了解的，这部分可能比较偏虚拟化和分布式。论文笔记希望能够记录自己在读论文的时候的想法，其中包括但不限于论文的大致 idea，实现方式，以及自己对论文的评价等等。希望能够把每一篇论文的笔记限制在 1000 字以内。

## 目录(TOC)

   * [论文笔记](#论文笔记)
      * [目录(TOC)](#目录toc)
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
         * [存储(Storage)](#存储storage)
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
         * [Taint Tracing](#taint-tracing)
            * [TaintDroid](#taintdroid)
         * [ROP](#rop)
            * [Hacking Blind](#hacking-blind)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

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

### 存储(Storage)

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

#### Google Native Client

* [Native Client: A Sandbox for Portable, Untrusted x86 Native Code](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/34913.pdf)
* [论文笔记 in GitHub](https://github.com/gaocegege/NaCl-note)

这篇论文是在 CSP 课上读的， 因为需要做分享，所以读的相比于其他论文要仔细一些，之前在阅读的时候就写了一些阅读笔记，比较冗长，这里写一些这篇论文的大概 idea 的介绍，以及一些自己的评论。

Google Native Client(NaCl)，简单来说是一个在浏览器里跑 Native 代码的技术。类比技术是微软<del>臭名昭著</del>的 ActiveX。相比于 ActiveX 那种毫无安全性可言的实现，NaCl 使用了一些自己改良过的 Software Fault Isolation(SFI) 的技术，结合了 ptrace 这样的 System Call Interception 的工具，来实现了在浏览器里安全运行 Native 代码的功能。从实现角度来看，是先对代码进行静态检查，保证代码符合 NaCl 制定的一些规则，然后再把程序运行在一个沙箱内，Native 代码所有跟外界的通讯，包括系统调用，都会被封装或者拦截，使用这样的方式来实现了对 Native 代码的安全隔离。在 2009 年的时候，Google 组织了一个 Native Client Security Contest，鼓励开发者寻找 NaCl 的漏洞，最终发现了 20 多个漏洞但是没有一个可以从根本上破坏 NaCl 的保护。目前 Google Chrome 浏览器仍然支持以这样的方式来运行 Native 代码，只不过好像没有多少人在用的样子。Demo 很容易运行，感兴趣可以试一下，很简单就可以实现从 CPP 代码到 Javascript 代码的通信。

为了提高浏览器段代码运行的效率，还有另外一个流派的做法，那就是 [asm.js](http://asmjs.org/)，它的实现思路跟 NaCl 完全不一样，并不会在浏览器里执行 Native 代码，因此不会有这么多安全方面的问题需要考虑，而是通过修改 LLVM 的那一套工具链，把 Native 代码编译成 Javascript 的一个子集，运行这个子集的 Javascript 代码。这样的实现最高可以只比 Native 应用慢一倍，虽然不如 NaCl 媲美原生应用，但是也可以接受了。这是 Firefox 浏览器的路子。

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

### Taint Tracing

#### TaintDroid

[TaintDroid: An Information-Flow Tracking System for Realtime Privacy Monitoring on Smartphones](http://www.appanalysis.org/tdroid10.pdf)

```
// TODO Add the notes
```

### ROP

#### Hacking Blind

[Hacking Blind](www.scs.stanford.edu/~sorbo/brop/bittau-brop.pdf)

```
// TODO Add the notes
```
