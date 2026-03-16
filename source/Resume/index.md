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
Has 14 years of experience.  
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

1.  Led the development and architecture design of [nsx-operator](https://github.com/vmware-tanzu/nsx-operator.git), an open-source Kubernetes operator that bridges VMware NSX-T networking with Kubernetes clusters, enabling unified network policy management for both VMs and Pods. The project consists of various Kubernetes controllers managing custom resources including SecurityPolicy, VPC, Subnet, SubnetPort, NetworkInfo, and IPAddressAllocation. The project follows declarative design philosophy - users describe desired state via K8s CRs, and nsx-operator reconciles actual state to match desired state.
2.  Designed and implemented SecurityPolicy CRD that extends Kubernetes NetworkPolicy to support NSX-T distributed firewall rules with advanced features including priority-based policy enforcement, VM/Pod selector with label-based targeting, multiple actions (Allow/Drop/Reject), IP blocks and CIDR-based rules, and named port support. The implementation integrates with NSX-T API to create distributed firewall rules and synchronizes realization state back to Kubernetes. Implemented automatic K8s label to NSX Group mapping for fine-grained network security control at Pod/VM level.
3.  Implemented VPC networking features including subnet management (Subnet, SubnetSet, SubnetBinding), IP address allocation with DHCP integration, custom gateway and DHCP server address configuration, subnet connectivity state management, shared subnet functionality for cross-namespace networking, VPC Connectivity Profile support, and static route support for VPC traffic routing. Shared Subnet allows cross-namespace network resource sharing, a key feature of VPC networking.
4.  Developed health monitoring system with inventory synchronization checks, realized state tracking for NSX-T resources, system health reporting, and comprehensive E2E test framework for validating VPC networking, security policies, and shared subnet functionality. Migrated health reporting logic from NCP (Network Control Plane) and optimized E2E test execution time.
5.  Built robust Kubernetes controller framework with event handlers, dependency watchers, webhook validation for CRD mutations, resource caching and state management using NSX Resource Store to reduce API calls, and error handling with automatic reconciliation and retry mechanisms. Implemented Finalizer mechanism to ensure proper NSX resource cleanup during CR deletion, preventing orphan resources.
6.  Developed backup/restore functionality supporting pre-created VPC/Subnet and AddressBinding, ensuring correct Pod state restoration on NSX side. Implemented annotation-based state tracking to handle edge cases such as VIF MAC address preservation and SubnetPort IP address retention.
7.  Contributed to nsx-ujo (NSX User World Operator): dynamically configure AVI SE subnet CIDR via NCPSetting CRD (replacing hardcoded 100.65.0.0/28); added virtualnetworks RBAC permissions for DevOps users; enabled FIPS-compliant TLS support; implemented multi-manager fallback mechanism for HttpError and TimeDrift validation.
8.  Led logging system upgrade, migrated from Zap logger to Zerolog for enhanced logging capabilities, implemented unified HTTP request/response logging, and added sophisticated log tracking for resource cleanup operations to facilitate production issue debugging.
9.  Contributed to patent innovation: *VMWI609.WO - Named Port Security Policy: A More Flexible Traffic Isolation Solution*, enabling security policies to reference application-defined port names instead of hardcoded port numbers for more maintainable network configurations.

### 2019.4 - 2021.4

*   **Senior engineer of Tencent cloud network group**

1.  Led the backend development of Tencent Cloud VPC (Virtual Private Cloud) network controller. VPC is an exclusive cloud network space built on Tencent Cloud, providing network services for cloud resources with complete logical isolation between different VPCs, supporting customizable network environments, routing tables, and security policies. The private cloud network serves as the core hub of Tencent Cloud's entire network infrastructure, with the controller responsible for distributing flow tables to hundreds of thousands of physical servers and gateways in real-time. Currently, Tencent Cloud operates 25 geographic regions with 53 available zones, with total server count exceeding 1 million and bandwidth peak reaching 100T. Achieving efficient, elastic, and cost-effective network resource interconnection in such a massive scale presents a highly challenging technical课题. Architected and implemented: MySQL for flow table persistence, ZooKeeper for distributed service discovery and configuration push, custom protocols/Protobuf for low-latency data communication with DPDK/Kernel data plane, supporting cross-VPC connectivity, hybrid cloud networking, and dedicated line access.
2.  Developed the backend for basic network security group (Security Group) control layer. Security groups are stateful virtual firewalls providing fine-grained network access control for cloud servers, load balancers, and databases, serving as the core means of cloud network security isolation. Similar to the private cloud network controller architecture, ZooKeeper serves as middleware for real-time security policy push, with physical servers using Linux kernel NetFilter framework for packet filtering at the data link layer, supporting efficient matching and real-time updates for millions of security rules.

### 2018.8 - 2019.4

*   **Senior engineer of SHAREit**

1.  Led the architecture upgrade of YouTube video large-scale crawling system, transforming single-machine crawling to distributed cluster architecture. This is the core data source for the company's Toolkit product line, requiring massive daily YouTube video metadata processing. Technical solution: AWS EC2 AutoScaling elastic computing cluster for consumer-based competitive crawling and data parsing, MySQL for storing crawling progress and statistics, Redis Bitmap for Bloom Filter deduplication at scale, AWS SQS message queue for traffic削峰 and async processing, DynamoDB for distributed underlying storage, AWS Lambda for ElasticSearch index synchronization and efficient full-text search. After architecture upgrade, throughput increased from less than 10K to 200K videos daily, cumulative data reaching 30 million, DAU reaching 20M in India and 10M in other regions, becoming one of the company's most core data sources.
2.  Built YouTube video server-side CDN address parsing and client crowdsourcing system, processing 20K client-reported CDN address data per second globally. This is a typical big data real-time processing scenario. Technical solution: AWS Aurora distributed database for server-parsed and client-reported CDN links, supporting refined storage and query by country/city dimensions. Redis distributed locks coordinate multi-consumer resource competition, effectively preventing duplicate parsing. Implemented intelligent link lifecycle management: timely expired link cleanup, 302 redirect availability detection. Through series of optimizations, link delivery success rate increased from <30% to 99.99%, video playback first-second success rate improved from 0% to 80%. Further optimized to reduce instances from 100 to 50 while improving performance, significantly reducing infrastructure costs. Awarded Annual Excellent Project for outstanding technical contribution and business value.

### 2015.10 - 2018.8

*   **Senior engineer of Xiaomi TV**

1.  Participated in Mobile Pictorial backend architecture design and operation platform development. Mobile Pictorial is a Xiaomi phone built-in content push application with independent entry point for user activation, an important part of Xiaomi's content ecosystem. Participated in overall backend architecture and operation platform design, deeply integrated with Xiaomi recommendation system and data factory. Tech stack: Master-slave MySQL for underlying database high availability, Django operation platform publishing data to Redis cache layer, Nginx for layer-7 load balancing and reverse proxy, distributing requests to backend Tornado async service processes, core API QPS reaching 5K.
2.  Developed mobile video series subscription and push notification system. Typical subscribe-notify scenario: users subscribe to favorite TV series, receive automatic notifications upon updates. Technical solution: Clients register Topic with episode ID on Xiaomi push system, server periodically polls for new episode releases, triggers PUSH notifications to subscribers upon detection.
3.  Independently designed and developed client log reporting platform. At that time, Xiaomi TV had sold over 10 million units in China with backend service QPS reaching 20K. Due to aggregated CP (Content Provider) resources, video playback and data browsing frequently crashed, causing numerous user complaints. To timely understand and record client error logs, conducted detailed technical research and solution design, ultimately independently completing the full implementation of the reporting platform. Typical big data real-time processing scenario: multi-producer/multi-consumer model for high-concurrency processing, introduced Twitter's open-source Kestrel message queue for traffic buffering, integrated Xiaomi Cloud FDS distributed storage for massive log file storage, integrated OpenFalcon operations monitoring system for real-time queue backlog and request status monitoring. Technical details: Multiple Tornado async processes concurrently receive compressed client upload logs (max 100K/log), log data enters Kestrel message queue, multiple consumer processes read from queue and write to FDS storage, download links generated by FDS along with metadata written to operations system database for customer service and developer troubleshooting. Since Kestrel Python client easily experiences duplicate consumption issues during high concurrency, innovatively introduced distributed lock mechanism to perfectly solve this problem. This project significantly improved playback resource stability and substantially reduced user complaint rate.
4.  Led Xiaomi TV PatchWall internationalization project backend development. PatchWall is Xiaomi TV's core business, the module with strongest user perception, and the company's main revenue driver. For international business expansion, Xiaomi TV collaborated deeply with Google to jointly develop Android TV (ATV) project, successfully expanded into overseas markets including India, Indonesia, and Russia, integrating over 10 local Content Providers (CPs). Referenced mature domestic Xiaomi TV backend architecture, combined with international business special requirements, developed and deployed complete international backend services. Tech stack: Service API using Python Tornado async framework, search engine using Solr for full-text search, cache layer using Redis consistent Hash for data sharding, persistence layer using master-slave MySQL and Redis hybrid storage, operations system built on Django Admin supporting multi-language interface. For different countries' laws and market characteristics, innovatively proposed "one codebase, multiple configurations, independent data" architecture solution. Implemented via i18n_cms and i18n_service two systems: unified code maintenance, differentiated configuration by country, independent data storage, each country deployed independent physical server room. This solution greatly reduced development costs, achieved data standardization, and saved technical costs for CP integration in various countries.
5.  Independently developed TV APK gray-scale upgrade system. This is a critical feature affecting tens of millions of users: Different upgrade strategies for different TV models, user levels, and TV system versions. To effectively control upgrade risks and save bandwidth costs, implemented time-window-based gray-scale release. Technical principle: Calculate time window based on current time percentage, determine if device falls within window based on device ID Hash, only devices that hit the window receive upgrade push. This gray-scale strategy ensures smooth new version promotion while preventing mass complaints from upgrade failures.

### 2014.8 - 2015.10

*   **Engineer of Knownsec**

1.  Monitored hacker attack behaviors on government websites in real-time through crawler technology, captured attack characteristics and patterns, timely provided warning information to public security departments, assisted relevant departments in network security protection and traceability.
2.  Developed Star Map project backend. This project required storing massive website vulnerability information. Since there was no self-built CDN, adopted MongoDB's GridFS distributed file storage solution for storing static resources like images, supporting massive data storage needs.

### 2013.2 - 2014.8

*   **Engineer of Baidu Mobile**

1.  Developed mobile games and application distribution backend. Baidu Mobile Games was one of China's largest mobile game distribution platforms at the time. Backend services mainly used C++ language, built on Baidu's proprietary UB framework for high-concurrency interface services, providing unified API interfaces for clients and middleware, supporting flexible business integration, core API QPS reaching 3K.
2.  Independently developed chess & card hall lottery activity system. Typical high-concurrency marketing scenario: designed user lottery ticket grabbing activity, backend used Nginx+Openresty for request flow filtering and preliminary validation, multiple application servers implemented complex business logic, ticket inventory stored in Redis, used Redis transaction functions and atomic operations (INCR/DECR) for precise inventory increment/decrement, ensuring no overselling under high concurrency.
3.  Designed and implemented MySQL + MongoDB hybrid storage architecture. MySQL stored core business data for transaction consistency, MongoDB handled unstructured interface data, providing flexible scaling capability.

### 2012.10 - 2013.2

*   **Engineer of Hundsun electronic bank**  
    Developed low-counter transaction module of banking financial product sales system, participated in core banking system construction, gained valuable financial industry business experience.

# Contact
*   **Email**  
    [zhengxiexie2@gmail.com](mailto:zhengxiexie2@gmail.com)
*   **WeChat**  
    zhengxiexie
*   **QQ**  
    373618217
*   **Blog**  
    [https://zhengxiexie.github.io/](https://zhengxiexie.github.io/)
