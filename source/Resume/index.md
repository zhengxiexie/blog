---
title: Resume
type: Resume
layout: Resume
date: 2023-01-07 21:50:07
---
<img style="position:relative;right:30px;float:right" src="/medias/pic/2.jpg" width="15%" height="48%">

# Keywords
* **TL;DR**
Chinese Name: Xie, Zheng  
English Name: Ben  
Birthday: 1989.5.12  
Good at English and Chinese. [listen](https://music.163.com/dj?id=2059907935&userid=52336738).  
Worked in Baidu, Xiaomi, Tencent, VMware, and other top-tier Internet companies, and is familiar with mainstream Internet technologies.  
Has 10 years of experience.  
Like distributed technologies, my paper *A Brief Discussion on CAP and Paxos Consensus Algorithm* was selected as one of Tencent's external technical engineering anthologies. [link](https://mp.weixin.qq.com/s/Fj4zERz9PEuNumd_SI0bEA).  
Passionate about innovation and published patents at VMware *VMWI609.WO-NAMED PORT SECURITY POLICY–A MORE FLEXIBLE TRAFFIC ISOLATION SOLUTION*.

# Skills
* **TL;DR**
Python/C/C++/PHP/Golang. Docker, Linux/Unix Zookeeper/SQS  
TCP/IP/HTTP/gRPC, Mysql/Redis/MongoDB, AWS/vSphere/NSX-T  
Distributed System Development  
Mainstream web technology

# Highlights
* **TL;DR**
Experience with the world’s top 500 Internet companies for 10 years  
Experience of the Chinese top 3 Internet companies for 3 years  
Good at English speaking and writing  
Fast learning ability

# Education
* **Bachelor of Computer Science and Technology**  
 
    Zhejiang University of Technology  
    2008/09 - 2012/06

# Company Experience
### 2021.4 - Now

*   **VMware JIL in vSphere k8s drivers Member of Technical Staff**

1.  Led the development and architecture design of nsx-operator, an open-source Kubernetes operator that bridges VMware NSX-T networking with Kubernetes clusters, enabling unified network policy management for both VMs and Pods. The project consists of various Kubernetes controllers managing custom resources including SecurityPolicy, VPC, Subnet, SubnetPort, NetworkInfo, and IPAddressAllocation.
2.  Designed and implemented SecurityPolicy CRD that extends Kubernetes NetworkPolicy to support NSX-T distributed firewall rules with advanced features including priority-based policy enforcement, VM/Pod selector with label-based targeting, multiple actions (Allow/Drop/Reject), IP blocks and CIDR-based rules, and named port support. The implementation integrates with NSX-T API to create distributed firewall rules and synchronizes realization state back to Kubernetes.
3.  Implemented VPC networking features including subnet management (Subnet, SubnetSet, SubnetBinding), IP address allocation with DHCP integration, custom gateway and DHCP server address configuration, subnet connectivity state management, shared subnet functionality for cross-namespace networking, and static route support for VPC traffic routing.
4.  Developed health monitoring system with inventory synchronization checks, realized state tracking for NSX-T resources, and comprehensive E2E test framework for validating VPC networking, security policies, and shared subnet functionality. Implemented sophisticated logging and debugging capabilities for resource cleanup operations.
5.  Built robust Kubernetes controller framework with event handlers, dependency watchers, webhook validation for CRD mutations, resource caching and state management, and error handling with automatic reconciliation and retry mechanisms.
6.  Contributed to patent innovation: *VMWI609.WO - Named Port Security Policy: A More Flexible Traffic Isolation Solution*, enabling security policies to reference application-defined port names instead of hardcoded port numbers for more maintainable network configurations.

### 2019.4 - 2021.4

*   **Senior engineer of Tencent cloud network group**

1.  Responsible for background development of Tencent Cloud VPC(virtual private cloud) network controller, VPC is the exclusive network space built based on Tencent Cloud, providing network services or resources. There is complete logical isolation between different private networks. You can customize the network environment, routing table, security policy, etc. At the same time, private networks support multiple ways to connect to the Internet, other VPCs, and local data centers, enabling easy deployment of networks in the cloud. The private cloud network is the core of the entire Tencent cloud network, and the controller is responsible for distributing hundreds of thousands of physical machines and gateway flow tables. Now Tencent Cloud operates 25 geographic regions and 53 available areas. At the controller level, the Mysql storage flow table is adopted, ZooKeeper is used as middleware to actively inform the physical machine and gateway to send and receive data, and custom protocol or Protobuf protocol is used for data communication with DPDK or Netlink.
2.  Responsible for background development of the control level of basic network security group. The security group is a virtual firewall with a stateful packet filtering function. It is used to set the network access control of the cloud server, load balancing, cloud database, and other instances, and control the in-out traffic at the instance level. It is an important may of network security isolation. Like the private cloud network controller, ZooKeeper is also adopted as the middleware notification security policy, and the physical machine uses the kernel NetFilter Framework to filter packets.
3.  Responsible for auto-testing cases for controller components.

### 2018.8 - 2019.4

*   **Senior engineer of SHAREit**

1.  Responsible for large-scale crawling of YouTube videos, changing the original stand-alone mode to distributed crawling AWS EC2 Autoscaling cluster is used to implement consumer competitive crawling and parsing of data, Mysql is used to store keyword crawling progress and number, Redis bitmap is used to implement Bloom filters for duplicated videos. AWS SQS is used to implement message queue buffering, and Dynamodb is used to implement underlying storage. AWS Lambda is used to trigger ElasticSearch index synchronization and support the server query. Improve the original data volume of less than 10,000 a day to 200,000 a day, DAU reaches 30 million in India and 10 million in Non-India.
2.  Responsible for the CDN address parsing of YouTube video source and storage of the clients-sourcing analysis, receiving 20,000 data per second from the client. Use the AWS Aurora database to store links that are parsed by the server and uploaded by the client Supports storage by country and city dimensions. Redis distributed locks are used to coordinate consumer competition for resources and prevent duplicate parsing. Timely remove expired links, timely detect 302-jump links and their availability, increase the rate of link release from the original less than 30/% to 99.99/%, and increase the success rate of the link to 80%. Further optimization is carried out to reduce the number of instances from 100 to 50. This project is awarded as the annual excellent project by the company.

### 2015.10 - 2018.8

*   **Senior engineer of Xiaomi TV**

1.  Mobile Pictorial is a built-in application for Xiaomi mobile phones, with an entrance that allows users to choose to open or close it. Participated in the design of the background architecture and operation platform, and the joint of Xiaomi recommendation system and data factory The master and slave MySQL is used as the underlying database for the Django operation platform to release data to Redis. Nginx does load balancing and distributes requests to the Tornado process in the background. Tornado is responsible for responding to Redis data to the client, and QPS reaches 5K.
2.  Mobile video realizes the function of watching TV series. Once a user subscribes to a new TV series, it will automatically dock with the Xiaomi push system to notify the user. The client registers the Topic with the EPISODE ID on the PUSH system, and the server regularly detects Whether there are new episodes, while the POST request pushes the system and sends notifications to the client.
3.  Currently, Xiaomi TV has sold 10 million sets in China, with QPS reaching 20,000. However, due to the aggregation of CP resources from all parties, it often crashes when playing videos and browsing data. To timely understand and record the client error log, the report barrier platform is designed independently Multi-producer and multi-consumer models are adopted for development, and the Kestrel message queue of Twitter is introduced to access Xiaomi Cloud FDS. OpenFalcon operation and maintenance monitoring are added to monitor the queue situation and request status in real-time. Asynchronous multiple Tornado process to accept the client from the compressed log data, and promptly return to the client the reported barrier ID, the log maximum size reaches 100k, the log data is sent to the Kestrel message queue, multiple consumers process to read data from the queue, write the FDS, and will return to the log after the FDS download link along with other metadata, improve the stability of the play resources. Since the Kestrel Python client is prone to fetch the same data when fetching data with high concurrency, distributed locking is introduced to solve this problem.
4.  PatChWall is the core business of Xiaomi TV. It is the place with the most user experience and also the main business. To expand its international business, Xiaomi TV cooperated with Google to develop ATV(Android TV), expanded into the markets of India, Indonesia, and Russia, and connected to more than 10 local CPs. Regarding Xiaomi TV background architecture, developed and deployed international business. The Service API uses the Python Tornado framework, the search engine uses Solr, the cache layer uses Redis consistent Hash, and the persistence layer uses the master-slave Mysql and Redis. The operating system is based on Django Admin, and it supports multiple languages. Combined with local laws and other special circumstances, start multiprocessor, for all posters processed into a uniform format, plus the effect of mask and edge blur. The international version of the code is called II8N CNS, il8N Service. In different countries, because we provide the platform, I proposed the scheme, using a set of the same code, but different configurations according to different countries with multiple sets of data, each country deployed an independent room, in this way, for the development, you just need to change a code. Data standardization also saves docking costs.
5.  Independently develop the function of apt upgrade. Calculating the time window according to the current time percentage, and calculating whether it falls Into the time window according to the hash value of the device id of the TV, saves bandwidth cost, gray level upgrade principle is adopted.

### 2014.8 - 2015.10

*   **Engineer of Knownsec**

1.  Monitor hacker attacks on Chinese government websites through crawlers, and provide alarm information to public security departments timely.
2.  Responsible for the back-end system of the star map project, which stored the vulnerability information of the website Since there is no self-built CDN, GridFS is adopted to store static data such as images.

### 2013.2 - 2014.8

*   **Engineer of Baidu Mobile**

1.  The main business is mobile games and application distribution. The background mainly uses C++ language and the UB framework developed by Baidu to provide interfaces for clients and the middle tier to support various businesses. QPS up to 3k.
2.  Independently develop the chess and card hall project, and design the activity of users competing for raffle tickets. Background Nginx and Openresty filter request flow, multiple application servers to implement application logic, lottery storage in Redis, using Redis transaction function and atomic operation to achieve inventory addition and subtraction.
3.  MySQL stores resource data and MongoDB is responsible for interface data.

### 2012.10 - 2013.2

*   **Engineer of Hundsun electronic bank**  
    Responsible for the development of low counter of bank financial management sales system.

# Contact
*   **Email**  
    [zhengxiexie2@gmail.com](mailto:zhengxiexie2@gmail.com)
*   **WeChat**  
    zhengxiexie
*   **QQ**  
    373618217
*   **Blog**  
    [https://zhengxiexie.github.io/](https://zhengxiexie.github.io/)