[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: CAI, Mengjia
### Student Id: 21072941
### Email: mcaiah@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > For my performance measurements, I used the Phoronix Test Suite.
    > 1. CPU performance(Average Compression Rate): This benchmark measures the efficiency of the CPU in compressing data. I selected this instead of the "Average Decompression Rate" because compression tasks are often more CPU-intensive than decompression, making it a better indicator of CPU performance under heavy load.
    > 2. Memory Performance(Copy Benchmark:Integer Rate): This measures how quickly the system can copy data in memory. This test is useful for assessing the performance of the memory subsystem, which directly affects tasks that require heavy memory operations.
    > The values obtained from the measurements represent the compression rate for the CPU benchmark and the integer rate (in MB/s) for the memory benchmark. These values indicate how efficiently the CPU and memory perform under specific load conditions.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |3599 MIPS|10820.10 MB/s|
    | `t2.medium`  |10072 MIPS|19866.53 MB/s|
    | `c5d.large` |7783 MIPS|13806.94 MB/s|

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

    > Answer:
    > 1. CPU Performance: The `t2.medium` instance has the highest CPU performance (10072 MIPS), followed by `c5d.large` (7783 MIPS), and `t2.micro` with the lowest CPU performance (3599 MIPS). Even though `t2.medium` has more vCPUs, it shows better performance than `c5d.large` despite the latter having a higher instance size. This could be due to different CPU architectures or optimizations between instance families.
    > 2. Memory Performance: The `t2.medium` instance also leads in memory performance (19866.53 MB/s), followed by `c5d.large` (13806.94 MB/s) and `t2.micro` (10820.10 MB/s). The memory performance roughly increases as the number of vCPUs and memory resources increase, though `t2.medium` exceeds `c5d.large` in memory performance despite having fewer resources.
    > The performance of EC2 instances does generally increase with the increase in vCPUs and memory resources, especially for memory performance. However, the CPU performance doesn't always increase proportionally, as seen with the `t2.medium` instance outperforming `c5d.large` in CPU performance. This indicates that factors like instance architecture and specific optimizations also play a role in the performance characteristics of the instances.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |27400|0.034|
    | `m5.large` - `m5.large`   |28900|0.025|
    | `c5n.large` - `c5n.large` |34100|0.015|
    | `t3.medium` - `c5n.large` |2320|0.724|
    | `m5.large` - `c5n.large`  |2640|0.648|
    | `m5.large` - `t3.medium`  |4580|0.174|

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    > Answer:
    > 1. Same Instance Type: When instances of the same type are connected (e.g., `t3.medium - t3.medium`, `m5.large - m5.large`, `c5n.large - c5n.large`), the network performance is optimal, showing high TCP bandwidth and low RTT values. The c5n.large instances consistently perform best in both metrics.
    > 2. Different Instance Types: When instances of different types are connected (e.g., `t3.medium - c5n.large`, `m5.large - c5n.large`, `m5.large - t3.medium`), the network performance decreases significantly. This is reflected by reduced TCP bandwidth and increased RTT values. This suggests that network performance can be impacted when connecting instances of different types, especially when they are not from the same optimized family.
    > 3. This indicates that network performance is generally better within the same instance type and optimized for instance family.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |32.4|59.628|
    | N. Virginia - N. Virginia |3570|0.319|
    | Oregon - Oregon           |4750|0.293|
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    > 1. When instances are deployed in different regions (e.g., `N. Virginia - Oregon`), network performance suffers significantly, as reflected by the low TCP bandwidth and high RTT values. This is typical due to the physical distance between regions and the potential routing and inter-region data transfer limitations.
    > 2. In contrast, when instances are deployed within the same region, such as `N. Virginia - N. Virginia` or `Oregon - Oregon`, both TCP bandwidth and RTT perform much better, reflecting optimized networking within the same region. The `Oregon - Oregon` connection performs slightly better than `N. Virginia - N. Virginia`, likely due to internal regional optimizations or differences in infrastructure.
    > 3. This shows that instances deployed within the same region offer the best network performance, while inter-region communication is generally slower with higher latency.
