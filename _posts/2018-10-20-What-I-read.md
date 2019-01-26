
[Setup ProxySQL for High Availability (not a Single Point of Failure)](https://www.percona.com/blog/2017/01/19/setup-proxysql-for-high-availability-not-single-point-failure/)

* Percona Blog 에서 ProxySQL을 이용한 HA 구성 방안 제시
* Keepalived, ProxySQL, MySQL
  
[Facebook’s Instagram: Making the Switch to Cassandra from Redis, a 75% ‘Insta’ Savings](https://www.datastax.com/dev/blog/facebooks-instagram-making-the-switch-to-cassandra-from-redis-a-75-insta-savings)

> Some details on our cluster: It’s a 12 node cluster of EC2 hi1.4xlarge instances; we store around 1.2TB of data across this cluster. At peak, we’re doing around 20,000 writes per second to that specific cluster and around 15,000 reads per second. We’ve been really impressed with how well Cassandra has been able to drop into that role. We also ended up reducing our footprint, so that’s been a really good experience for us. We learned a lot from that first implementation and we were able to apply that knowledge to our most recent implementation. Every time someone pulls up their Instagram now, they’re hitting that 12 node Cassandra cluster to pull their data from; it’s really exciting.

* 2014년 내용이니 Instagram이 Facebook 데이터센터로 이동하기 전
* 12 개의 EC2 [hi1.4xlarge](https://aws.amazon.com/blogs/aws/new-high-io-ec2-instance-type-hi14xlarge/) (60.5 GB RAM, 2개의 1 TB SSD, 16 virtual cores)
* 20k writes per sec, 15k reads per sec

>I would recommend digging into the system and reading all of the Cassandra documentation, especially the stuff on the DataStax website. The best part is documentation that I’ve noticed is it has a lot of extra information about the internals; really understanding that is important. Any database or datastore you use, you’re really going to need to dig into the documentation in order to properly use it the way it’s intended. People often run into situations where they get themselves cornered by adopting a solution too quickly or incorrectly and not doing their homework.  Specifically, with a datastore, it’s really important that it’s the most stable and reliable part of your stack.

* "really understanding that is important"
