---
title: 浅谈 CAP 和 Paxos 共识算法
top: false
cover: true
img: /medias/featureimages/4.jpg
toc: true
mathjax: true
date: 2019-11-13 19:04:00
password:
summary:
tags: 
  - 分布式
  - CAP
  - 共识
  - Paxos
  - Raft
  - ZAB
categories: 技术
---

# 浅谈 CAP 和 Paxos 共识算法

分布式共识是整个分布式系统中最为重要的问题，没有之一，本人想按照自己的理解用大白话讲一讲，既然是大白话，就非常不严谨，也很粗浅，有说的不对的地方，欢迎大佬拍砖，头盔已戴好。

分布式、CAP、一致性、状态机、共识、Paxos、Raft、ZAB，想必这些名词大家已经烂熟于耳了，在感叹这些高逼格名词的同时，你是不是总感觉这些名词有点混淆不清、模棱两可的赶脚？如果有，请听我慢慢道来。（没有？好吧，您是大佬，请果断 command+w。）

---

# 什么是 CAP

![](Distributed_system_CAP-iteblog-ac864baa-9770-40b5-aaf0-432e011fafc6.png)

CAP 理论的背景介绍已经烂大街了，这里不过多介绍。我们谈谈如何理解它的问题，关于这方面，我觉得解释最清楚的是油管上某个小哥的视频：

[https://www.youtube.com/watch?v=Jw1iFr4v58M](https://www.youtube.com/watch?v=Jw1iFr4v58M)

## 用人话解释三个名词：

### **一致性**

如果刚刚向一个节点写入，那么之后，从另外一个节点读取的必须是刚刚写入的数据，不能是更老的数据。

### **可用性**

如果请求一个节点，这个节点必须能够给予回复，如果节点挂掉了，那就谈不上可用性了。

### **分区容忍性**

是否容忍网络分区，即可以允许节点和其它节点无法通信。

CAP 的意思就是说我们最多只能保证其中两个条件同时成立。

下面我们来看看为什么。

![](Untitled-75e05e42-d661-4446-9120-ae13749b1535.png)

如图所示，假如我们满足了分区容忍性，即虚线处表示两个节点发生了分区。

1. **假如要满足一致性**，那么，我们只能让请求另一个节点的操作暂时 hang 住，返回 client 失败或者超时的结果，这种情况多发生在银行柜台等对数据一致性要求很高的情境下，因为比起保证用户资金数目的正确性比暂时让用户无法操作要更重要一些。
2. **假如要满足可用性**，因为网络已经隔离，也就没办法达到一致性，这种情况多发生在互联网行业中，比如新闻等对数据一致性要求不高但对可用性要求高的情况下，毕竟，用户压根看不了新闻比看不到及时新闻要重要的多。

大家可以自己自由组合，最终会证明，三种条件不可能同时满足，其实大部分情况下，我们都是在一致性和可用性之间取舍而已。

![](Untitled-de737321-ffd8-4e8d-b3f7-3d5483db237e.png)

# Consistency = Consensus?

Consistency几乎被业界用烂了，以至于当我们在讨论一致性的时候，其实我们都无法确定对方所说的一致性是不是和自己的那个一致。😅

**Consistency：一致性，Consensus：协同，**这两个概念极容易混淆。

我们常说的一致性（Consistency）在分布式系统中指的是对于同一个数据的多个副本，其对外表现的数据一致性，如线性一致性、因果一致性、最终一致性等，都是用来描述副本问题中的一致性的。而共识（Consensus）则不同，简单来说，共识问题是要经过某种算法使多个节点达成相同状态的一个过程。在我看来，一致性强调结果，共识强调过程。

![](Distributed_system_Consistency_model-iteblog-392cde62-0df4-41ab-a200-8699a871dd03.jpg)

# 共识？状态机？什么鬼？

![](Untitled-a17735d5-49dd-4565-8bbf-271c080e18a5.png)

Ken Thompson

共识有个更高逼格的称呼：

### 基于状态机复制的共识算法

大牛们最擅长做的事就是把各种简单的问题用个高逼格的专业术语把你拒之门外，就好比 CCAV 里的各种指数其实说白了就是告诉你央行大妈又要放水了。

那么，状态机是什么？

状态机是有限状态自动机的简称，是现实事物运行规则抽象而成的一个数学模型。

说人话，好不？好，看下图，门，有两种状态，开着的和关着的。因此，在我看来状态是一种静态的场景，而转换赋予了其动态的变化。

![](Untitled-b4f0cf74-00d2-4eba-968f-619a4ed257da.png)

以此类比一下，如果一个节点当前的数据是 X，现在有了 add+1 的操作日志来了，那么现在的状态就是 X+1，好了，状态（X）有了，变化（操作日志）有了，这就是状态机。

分布式共识，简单来说，就是在一个或多个节点提议了一个状态应当是什么后，系统中所有节点对这个状态达成一致意见的整个过程。

共识是过程，一致是结果，明白了吗？

# 共识模型

## 主从同步：

![](Untitled-9ad779df-cde7-402f-8ca0-31cf1807e0e7.png)

我们都知道MySQL等业界常见数据库的主从同步（Master-Slave Replication)，主从同步分三个阶段：

- Master 接受写请求
- Master 复制日志至 Slave
- Master 等待，直到**所有从库**返回。

可见，主从同步模型存在致命问题：只要一个节点失败，则 Master 就会阻塞，导致整个集群不可用，保证了一致性，可用性缺大大降低了。

## 多数派：

每次写入大于 N/2个节点，每次读也保证从 N/2个节点中读。多数派的模型看似完美解决了多节点的一致性问题，不就是性能差点嘛，可是在并发的情况下就不一定了，如下图：

![](Untitled-43522eca-aed6-4148-bc7c-499ba25e2284.png)

在并发环境下，因为每个节点的操作日志写入顺序无法保证一致，也就无法保证最终的一致性。如图，都是向三个节点 inc5、set0 两个操作，但因为顺序不一样，最终状态两个是 0，一个是 5。因此我们需要引入一种机制，给每个操作日志编上号，这个号从小到大生成，这样，每个节点就不会弄错。（是否想到了网络中的分包与重组？）那么，现在关键问题又来了，怎么协同这个编号？貌似这是鸡生蛋、蛋生鸡的问题。😂

# Paxos

Paxos 模型试图探讨分布式共识问题的一个更一般的形式。

Lesile Lamport，Latex的发明者，提出了 Paxos算法。他虚拟了一个叫做 Paxos 的希腊城邦，这个岛按照议会民主制的政治模式制定法律，但是没有人愿意将自己的全部时间和精力放在这件事上。所以无论是议员、议长或者传递纸条的服务员都不能承诺别人需要时一定会出现，也无法承诺批准决议后者传递消息的时间。由于Paxos 让人太难以理解，Lamport觉得同行不能理解他的幽默感，于是后来又重新发表了朴实的算法描述版本[《Paxos Made Simple》](https://www.notion.so/CAP-Paxos-55dad97e36154e20895e26495fff49ec#891d9af87884442fbf2adaa36b9f9454)。

![](Untitled-4a4eaa95-1ce8-4bbd-89c9-700ad82641db.png)

该共识算法就整体来说，存在两个阶段，如图，第一个阶段是提议，第二个阶段是决定。

> 分布式系统要做到 fault tolorence，就需要共识模型，而节点达成共识，不仅需要节点之间的算法，还会取决于 client 的行为。比如即使副本系统使用multi-paxos在所有副本服务器上同步了日志序号，但如果Client被允许从非Leader节点写入数据，则整个副本系统仍然不是强一致的。

下面，重头戏来了，详细介绍 Paxos。

### 角色介绍：

**Client：**系统外部角色，请求发起者。如民众。

**Proposer:**  接受Client 请求，向集群提出提议（propose）。并在冲突发生时，起到冲突调节的作用。如议员，替民众提出议案。

**Accpetor:**  提议投票和接收者，只有在形成法定人数（Quorum，即 Majority 多数派）时，提议才会最终被接受。如国会。

**Learner:**  提议接受者，backup，备份，对集群的一致性没影响。如记录员。

### 步骤、阶段：

![](Untitled-a6750265-7683-4067-8d61-206f696acc69.png)

1. **Phase1a: Prepare**

    proposer 提出一个议案，编号为 N，N一定大于这个 proposer 之前提出的提案编号，请求 acceptor 的 quorum（大多数） 接受。

2. **Phase1b: Promise**

    如果 N 大于此 acceptor 之前接受的任何提案编号则接受，否则拒绝。

3. **Phase2a: Accept**

    如果达到了多数派，proposer 会发出 accept 请求，此请求包含上一步的提案编号和提案内容。

4. **Phase2b: Accepted**

    如果此 acceptor 在此期间没有收到任何大于 N 的提案，则接受此提案内容，否则忽略。

还记得上文中我们提到过，同步编号是非常重要的问题，绿色框出来的实际上就是同步编号的过程。通过这个编号，就知道每条操作日志的先后顺序。简单说来，**第一阶段，获取编号，第二阶段，写入日志。**可以看出来，Paxos 通过两轮交互，牺牲时间和性能来达到弥补一致性的问题。

现在我们考虑部分节点down 掉的情景。

![](Untitled-9d3289bb-0f85-43cb-913e-5c3690982bf5.png)

由于是多数派 accptor 达成了一致，第一阶段仍然成功获得了编号，所有最终还是成功的。

考虑 proposer down 掉的情景。

![](Untitled-03b14664-4015-4295-9be4-ed16042a68b7.png)

没关系，虽然第一个 proposer 失败，但下一个 proposer 用更大的提案编号，所以下一次 proposer最终还是成功了，仍然保证了可用性和一致性。

### 潜在问题：活锁

![](Untitled-12f560d2-d186-4995-8545-bad7909240b7.png)

Paxos 存在活锁问题。如图，当 第一个proposer 在第一阶段发出请求，还没来得及后续的第二阶段请求，紧接着第二个proposer 在第一阶段也发出请求，如果这样无穷无尽，acceptor 始终停留在决定顺序号的过程上，那大家谁也成功不了，遇到这样的问题，其实很好解决，如果发现顺序号被新的 proposer 更新了，则引入一个随机的等待的超时时间，这样就避免了多个 proposer 发生冲突的问题。

# Multi Paxos

由于Paxos 存在的问题：难实现、效率低（2 轮 rpc）、活锁。

因此又引入了Multi Paxos，Multi Paxos引入 Leader，也就是**唯一的 proposer**，所有的请求都需经过此 Leader。

![](Untitled-2a2f06c4-1305-44ec-98ea-94a532d36d56.png)

因为只有一个节点维护提案编号，这样，就省略了第一轮讨论提议编号的过程。

然后进一步简化角色。

![](Untitled-e36f718a-403c-40fc-adfe-423e7d7e6836.png)

Servers 中第左边的就是 Proposer，另外两个和自身充当 acceptor，这样就更像我们真实的系统了。Raft 和 ZAB协议其实基本上和这个一致，两者的差别很小，就是心跳的方向不一样。

# Raft和 ZAB

Raft 和 ZAB 协议将 Multi Paxos 划分了三个子问题：

- Leader Election
- Log Replication
- Safety

在 leader 选举的过程中，也重定义角色：

- Leader
- Follower
- Candidate

[这个动画网站](http://thesecretlivesofdata.com/raft/)生动展示了 leader 选举和日志复制的过程。在这里就不多讲了。

另外，[raft 官方网站](https://raft.github.io/)可以自己操作模拟选举的过程。

# 总结

今天，我们从 CAP 谈到 Raft 和 ZAB，中间穿插了各种名词，模型无论怎么变化，我们始终只有一个目的，那就是在一个 fault torlerance 的分布式架构下，如何尽量保证其整个系统的可用性和一致性。最理想的模型当然是 Paxos，然而理论到落地还是有差距的，所以诞生了 Raft 和 ZAB，虽然只有一个 leader，但我们允许 leader 挂掉可以重新选举 leader，这样，中心式和分布式达到了一个妥协。<br><br><br><br><br>


-------

*参考资料：*

[*http://blog.kongfy.com/2016/08/被误用的一致性/*](http://blog.kongfy.com/2016/08/%E8%A2%AB%E8%AF%AF%E7%94%A8%E7%9A%84%E4%B8%80%E8%87%B4%E6%80%A7/)

[*http://blog.kongfy.com/2016/05/分布式共识consensus：viewstamped、raft及paxos/*](http://blog.kongfy.com/2016/05/%e5%88%86%e5%b8%83%e5%bc%8f%e5%85%b1%e8%af%86consensus%ef%bc%9aviewstamped%e3%80%81raft%e5%8f%8apaxos/)

[*https://lotabout.me/2019/Raft-Consensus-Algorithm/*](https://lotabout.me/2019/Raft-Consensus-Algorithm/)

[*https://raft.github.io/*](https://raft.github.io/)

[*http://thesecretlivesofdata.com/raft/*](http://thesecretlivesofdata.com/raft/)