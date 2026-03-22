![Image](https://upload.wikimedia.org/wikipedia/labs/5/53/Etcd_logo.png)

![Image](https://etcd.io/img/client-architecture-balancer-figure-06.png)

![Image](https://etcd.io/docs/v3.5/learning/img/data-model-figure-01.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AtsGsmJ4IQW99CV0Ub96P8g.png)

## etcd

**etcd** is an open-source, distributed key-value store used to manage configuration data, service discovery, and coordination across distributed systems. Designed for reliability and consistency, it underpins critical cloud-native infrastructure such as Kubernetes.

### Key facts

* **Initial release:** 2013
* **Developed by:** CoreOS (later acquired by Red Hat)
* **Language:** Go
* **License:** Apache License 2.0
* **Core protocol:** Raft

### Architecture and operation

etcd maintains a consistent view of data across a cluster of nodes using the Raft consensus algorithm. Clients interact with the cluster through a simple HTTP/JSON or gRPC API to read or modify key-value pairs. It guarantees linearizable reads and writes, ensuring that all nodes see the same data even in the presence of failures.

### Role in cloud and container orchestration

In Kubernetes, etcd stores all cluster state data—including configuration, secrets, and workload definitions. Its reliability directly affects cluster health, making it a core dependency of Kubernetes and other distributed systems such as Cloud Foundry and OpenStack.

### Features and guarantees

etcd supports key TTLs (time-to-live), watch functionality for real-time updates, and authentication with role-based access control. It emphasizes durability through snapshotting and write-ahead logging, while offering fault tolerance through leader election and quorum-based writes.

### Ecosystem and evolution

Backed by the Cloud Native Computing Foundation (CNCF), etcd is widely adopted across cloud infrastructure projects. Continuous development focuses on scalability, security hardening, and performance optimization to support increasingly large and complex clusters.
s