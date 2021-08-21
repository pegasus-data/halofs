# HaloFS

![image](https://user-images.githubusercontent.com/4479171/130297141-745672a2-602d-4df2-9748-20354d1ee87c.png)

**QA and Performance Testers are welcome to participate. Download the latest binary and run some tests!**

HaloFS is a distributed storage platform that emphasizes primarily on storing massive amounts of data and retrieving the in the most secure and efficient way possible. 

Read the Technical Whitepaper [here](#)

# Problems in using a central main storage server
In a distributed file systems, each file is split into chunks and a primary main server keeps all the mapping of filenames, chunk indexes, propagation and handlers. The problem with this approach is that the central main server can't handle many small files efficiently, and since all read requests need to go through the chunk master, so it might not scale well for many concurrent users.

# Solution
Instead of managing chunks, HaloFS manages data volumes in the main server. Each data volume can hold massive amount of files. Each storage node can have several data volumes so the main node only needs to store the metadata about the volumes so that it can refer to them whenever a request is made.

# Features
- Automatic main server failover - no single point of failure in case a volume is down.
- Replication strategy that can be set.
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

## As a Storage Operator

**Run your main server**
```
./halofs main -defaultReplication=001
```
Take note of the replication types:

- 000	no replication
- 001	one replication in a single rack
- 010	one replication in a different rack on the same data center
- 100	one replication on a different data center
- 200	multiple replication on two other different data center

This will automatically put several volumes connected to your main.

**Say you have different mounted volumes that you want to connect to your main server. You can run the following.**
```
./halofs volume -dir="/tmp/data1" -max=2  -mserver="localhost:9310" -port=8080 & 
./halofs volume -dir="/tmp/data2" -max=2 -mserver="localhost:9310" -port=8081 &
```
## As a Consumer (Developer)
We have golang, java and rust libraries but the main server is always accessible thru HTTP which means you can all it using `curl`

**To save a file:**
Get an DIR assignment first.
```
curl http://localhost:9333/dir/assign
{"count":1,"fid":"3,01637037d6","url":"127.0.0.1:8080","publicUrl":"localhost:8080"}
```
Then use the `fid` to store the file
```
curl -F file=@/home/areyes/myphoto.jpg http://127.0.0.1:8080/3,01637037d6
{"name":"myphoto.jpg","size":43234,"eTag":"1cc0118e"}
```

**To read or load a file:**
```
curl http://localhost:9333/dir/lookup?volumeId=3
{"volumeId":"3","locations":[{"publicUrl":"localhost:8080","url":"localhost:8080"}]}
```
**To clean up or trigger a deletion of the file**
```
curl -X DELETE http://127.0.0.1:8080/3,01637037d6
```

Easy! It gets better! You can use the SDK/Libraries below. 

# SDK and Libraries for Developers
- [Golang](https://github.com/halostac-platform/golang-halofs-lib)

# Solutions Design
Storage is just one main component in an overall solutions design and HaloFS is flexible enough to accommodate virtually any applications that needs to scale. HaloFS was designed to store AND serve billions of files in the most efficient and fastest way possible. This feature combined with the privacy and security makes it the best storage file system to couple with distributed ledger technology.

![image](https://user-images.githubusercontent.com/4479171/130307547-69df3fe8-caea-4db1-90eb-6e654d5b5456.png)

For guidance and consultation on how to design and integrate HaloFS into your solution, contact us at [info@proofsys.io](mailto:info@proofsys.io)

# Integration 
- Distributed Ledger Integration 
- Amazon S3 
- Hadoop
- Red Hat Open Shift
- AES256-GCM Encrypted Storage

# Commerce and Incentive Mechanism
We have extended HaloFS to have it's own economic incentive mechanism by using blockchain technology. A FileSystem Operator who wants to expose their own HaloFS on the global network can have incentives by either participating or being selected to host either a replication or the main host.

**This feature is currently in heavy development as of the moment.**

# Contributing
We welcome any contributors who can evaluate and do performance test in our platform. For more information, email us at info@proofsys.io
