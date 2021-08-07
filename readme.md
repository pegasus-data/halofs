# HaloFS

**This is still in early development. QA and Performance Testers are welcome to contribute.**

HaloFS is a distributed storage platform that emphasizes primarily on storing massive amounts of data and retrieving the in the most secure and efficient way possible. 

Read the Technical Whitepaper [here](#)

# Problem that HaloFS is solving
In a distributed file systems, each file is split into chunks and a primary main server keeps all the mapping of filenames, chunk indexes, propagation and handlers. The problem with this approach is that the central main server can't handle many small files efficiently, and since all read requests need to go through the chunk master, so it might not scale well for many concurrent users.

# Solution
Instead of managing chunks, HaloFS manages data volumes in the main server. Each data volume can hold massive amount of files. Each storage node can have several data volumes so the main node only needs to store the metadata about the volumes so that it can refer to them whenever a request is made.

# Features
- Automatic main server failover - no single point of failure in case a volume is down.
- Replication mechanism that can be set.
- Automatic GZIP compression
- Any server node or contributor can add to the total storage space.
- Support ETag, Accept-Range, Last-Modified, etc.
- Support in-memory/leveldb/readonly mode tuning for memory/performance balance.
- Automatic Disk space reclaim after a deletion of update
- Support rebalancing the writable and readonly volumes.
- Customizable storage disk types to balance performance and cost.
- Unlimited capacity via tiered cloud storage for warm data.
- Erasure coding reduces storage cost and increases availability.

# Installation Guide
- Step 1: Download the binary
- Stpe 2: Run the following command

Go to API guide on how to perform basic tests on your HaloFS instance.

# SDK and Libraries for Developers
- Golang
- Java
- TSJS
- Rust

# Integration 
- Distributed Ledger Integration 
- Amazon S3 
- Hadoop
- WebDAV
- AES256-GCM Encrypted Storage

# Contributing
We welcome any contributors who can evaluate and do performance test in our platform. For more information, email us at info@proofsys.io
