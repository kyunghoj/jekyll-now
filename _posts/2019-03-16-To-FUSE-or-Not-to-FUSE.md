---
layout: post
title: "To FUSE or Not to FUSE: Performance of User-Space File Systems"
categories: papers fuse storage filesystem
---

일주일에 논문 한 편은 읽자고 마음먹은 후 첫 번째로 뽑은 논문이다. FUSE에 대해 개념은 대충 들어 알고 있다고 생각했지만, 자세히 알아보려고 FUSE Project 홈페이지에 갔더니, FUSE에 대해 자세히 알려면 코드를 읽든가 아니면 이 논문을 추천한다고 해서 고르게 되었다. 그래서 논문을 읽은 목적은 새로운 아이디어를 얻는 것 보다는 지식 습득에 있었다. 

```
@inproceedings {202324,
    author = {Bharath Kumar Reddy Vangoor and Vasily Tarasov and Erez Zadok},
    title = {To {FUSE} or Not to {FUSE}: Performance of User-Space File Systems},
    booktitle = {15th {USENIX} Conference on File and Storage Technologies ({FAST} 17)},
    year = {2017},
    isbn = {978-1-931971-36-2},
    address = {Santa Clara, CA},
    pages = {59--72},
    url = {https://www.usenix.org/conference/fast17/technical-sessions/presentation/vangoor},
    publisher = {{USENIX} Association},
}
```

> Traditionally, file systems were implemented as part of OS kernels. However, as complexity of file systems grew, many new file systems began being developed in user space. Nowadays, user-space file systems are often used to prototype and evaluate new approaches to file system design. Low performance is considered the main disadvantage of user-space file sytems but the extent of this problem has never been explored systematically. As a result, the topic of user space file systems remains rather controversial: while some consider user-space file systems a toy not to be used in production, others develop full-fledged production file systems in user space.

파일시스템은 전통적으로 커널에 구현되어 있었지만, FUSE 라는 것이 나와서 파일시스템을 user space에서 구현할 수 있게 되었다. 그러다 보니 FUSE로 많은 파일시스템이 구현되었는데, 혹자는 이것이 그저 toy program혹은 prototype 용이라고 하고, 다른 이들은 production에서도 쓸 만하다고 한다. 그 차이를 나누는 주요한 이슈는 성능이지만, 지금까지 그 성능 문제에 대해서 조사된 적이 없다.

> We develop a simple pass-through stackable file system in FUSE and then evaluated its performance when layered on top of Ext4 compared to native Ext4. We used a wide variety of micro- and macro-workloads, and different hardware usinig basic and optimized configurations of FUSE. We found that depending on the workload and hardware, FUSE can perform as well as Ext4, but in the worst cases can be 3x slower. 

저자들은 Ext4 위에서 돌아가는 단순한 pass-through 파일시스템을 FUSE로 구현하여 그 성능을 평가했다. 다양한 하드웨어 환경과 마이크로/매크로 워크로드에서 FUSE의 성능을 측정해 보았다. 리눅스 기본 파일시스템인 Ext4 와 비슷한 성능을 보여주기도 하지만, 최악의 경우에는 3배쯤 느릴 수 있다. 

> Next, we designed and built a rich instrumentation system for FUSE to gather detailed performance metrics. The statistics extracted are applicable to any FUSE-based systems. We then used this instrumentation to identify bottlenecks in FUSE, and to explain why, for example, its performance varied greatly for different workloads.

성능 측정에 그치지 않고 instrumentation을 통해 병목이 어디인지 찾고 다른 workload에서 왜 다른 성능을 보이는지 이유를 알아내려고 한다. 

일단 FUSE가 무엇인지, 어떤 구조를 가지고 있는지 다음 그림을 살펴보자. 

<center>
<img src="../images/fast17-vangoor/figure_1.jpg" width="640" alt="Figure 1. FUSE high-level architecture"/>
</center>

Application 들은 파일시스템과 직접 얘기하는 것이 아니라 [VFS](https://en.wikipedia.org/wiki/Virtual_file_system)라는 인터페이스를 통한다. 따라서 파일시스템 구현들은 [VFS](https://en.wikipedia.org/wiki/Virtual_file_system)가 정의하는 인터페이스를 구현한다. 따라서 위 그림과 같이, FUSE는 FUSE Driver가 커널모듈 형태로 존재하고, FUSE driver는 file system request 들을 user space에 있는 FUSE file-system daemon이라고 지칭되는 구현체에게 전달하는 역할을 한다. FUSE가 지원하는 request의 종류는 다음 테이블과 같다.

<center>
<img src="../images/fast17-vangoor/table_1.jpg" width="640" alt="Table 1. FUSE request types by group"/>
</center>

이 논문에서는 굵은 글씨로 표시된 것들만 자세히 설명하고 있다. 그 외에 것은 기존 VFS와 다르지 않은 의미를 가지고 있다. 특히 Permission 부분이 인상적이었는데, 

> When the kernel evaluates if a user process has permissions to access a file, it generates an `ACCESS` request. By handling this request, the FUSE daemon can implement custom permission logic. However, typically users mount FUSE with the *default_permissions* option that allows kernel to grant or deny access to a file based on its standard Unix attributes (ownership and permission bits). In this case no `ACCESS` requests are generated.

`ACCESS` request 를 처리하면 custom permission 로직을 구현할 수 있다. 예를 들어 분산시스템 환경에서 별도의 권한 및 접근 관리를 제공하는 서비스가 있다면, 이 부분을 이용할 수 있을 것이다. 물론 보통은 `default_permissions` 옵션을 이용해서 마운트 하게 되는데, 이때는 통상적인 Unix 의 permission 시스템(ownership 과 permission bits 기반)을 따르게 된다. 

<center>
<img src="../images/fast17-vangoor/figure_2.jpg" width="640" alt="Figure 2. FUSE queues organizations"/>
</center>

위 그림은 FUSE kernel driver 내부에서 request가 관리되는 queue 구조이다. 총 5개의 queue가 있다. (자세한 설명은 생략, 논문 참조 혹은 나중에 더 정리.)

몇 가지 중요한 성능 최적화에 대한 설명이다. 

> In its basic setup, the FUSE daemon has to `read()` request from and `write()` replies to `/dev/fuse`. Every such call requires a memory copy between the kernel and user space. It is especially harmful for `WRITE` requests and `READ` replies because they often process a lot of data. To alleviate this problem, FUSE can use *splicing* functionality provided by the Linux kernel [38]. Splicing allows the user space to transfer data between two in-kernel memory buffers without copying the data to user space. This is useful, e.g., for stackable file systems that pass data directly to the underlying file system.

성능에 대해서 조금만 알아보면 항상 나오는 주제인데, kernel와 user space 간에 memory copy 가 발생하면 성능이 떨어질 수 밖에 없다. 따라서 FUSE에서는 kernel의 [splicing](https://en.wikipedia.org/wiki/Splice_(system_call)) 이라는 기능을 사용하여 memory copy를 줄인다. 

> **Write back cache and max writes.** The basic write behavior of FUSE is synchronous and only 4KB of data is sent to the user daemon for writing. This results in performance problems on certain workloads; when copying a large file into a FUSE file system, `/bin/cp` indirectly casues every 4KB of data to be sent to userspace synchronously. The solution FUSE implemented was to make FUSE's page cache support a write-back policy and then make writes asynchronous. With that change, file data can be pushed to the user daemon in larger chunks of `max_write` size (limited to 32 pages).

또 다른 write 성능 개선을 위한 방법. write-back cache를 사용하고, 한꺼번에 여러개의 블록을 FUSE daemon 에 보내도록 했다.

여기까지는 FUSE 에 대한 소개였고, 이제 논문의 목적인 성능 측정을 하기 위해 Stackfs라는 것을 만들었다. Stackfs는 간단한 stackable passthrough file system으로, Ext4 위에서 request들을 그대로 전달하는 기능만을 가지고 있다고 생각하면 될 것 같다. 또한 (자세한 성능 측정을 위해) FUSE kernel module과 user-space library에 instrumentation도 추가했다. 

> Stackfs is a file system that passes FUSE requests unmodified directly to the underlying file system. The reason for Stackfs was twofold. (1) After examing the code of all publicly available [28, 43] FUSE-based file systems, we found that most of them are stackable (i.e., deployed on to of other, often in-kernel file systems). (2) We wanted to add as little overhead as possible, to isolate the overhead of FUSE's kernel and library. 

조금 더 자세한 설명: 왜 Stackfs 를 (그렇게) 만들었나? (1) 대부분의 FUSE 기반 파일시스템이 stackable file system이었다 (2) FUSE의 kernel과 라이브러리가 더하는 오버헤드를 측정하고 싶었다. 

> Complex production file systems often need a high degree of flexibility, and thus use FUSE's low-level API. As such file systems are our primary focus, we implemented Stackfs using FUSE's low-level API. This also avoided the overhead added by the high-level API. 

FUSE의 성능이 좋지 않다는 편견이 많은데, 저자들은 그것이 최적화가 적용되지 않은 옛날 버전에 대한 것이라고 생각했던 것 같다. 그래도 최적화가 적용되지 않은 버전과 적용된 버전 두 가지를 준비해서 실험했다. 

> FUSE has evolved significantly over the years and added several useful optimizations: writeback cache, zero-copy via splicing, and multi-threading. In our personal experience, some in the storage community tend to pre-judge FUSE's performance--assuming it is poor--mainly due to not having information about the improvements FUSE made over the years. We therefore designed our methodology to evaluate and demostrate how FUSE's performance advanced from its basic configurations to ones that include all of the latest optimizations. 


(To be updated...)