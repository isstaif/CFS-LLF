# CFS-LLF

CPU resource units, such as [Kubernetes’ millicores](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) or virtual CPUs (vCPUs), are essential for sharing physical CPU resources among multiple workloads. These abstractions specify the approximate CPU time a task or group of tasks (e.g., containers) will receive within a reference period of 1000 milliseconds. However, when managing a large number of containers (e.g., [50-100 or more](https://dl.acm.org/doi/abs/10.1145/3592533.3592807)), a notable discrepancy can arise between the allocated and actual CPU time due to the overhead introduced by [Linux group scheduling](https://lwn.net/Articles/240474/). One common approach to address this issue is to reserve additional CPU capacity. However, this often leads to significant resource waste, with unused headroom [sometimes reaching as high as 55%](https://dl.acm.org/doi/10.1145/3542929.3563465).

CFS-Lightest Load First (CFS-LLF) is an extension to the Completely Fair Scheduler (CFS) that mitigates the  CPU contention associated with a large number of colocated containers in a Linux cluster. The design of LLF is inspired by the Shortest Remaining Time First (SRTF) policy, and is different in prioritises tasks with the lightest load given a reference period that corresponds to one millicore (i.e. 1 second). CFS-LLF employs a load credit mechanism to prioritise cgroups based on their recent load, favouring those with lower load credit consumption. This allows to approximate LLF  by scheduling cgroups according to the CPU time already received, assuming it reflects the remaining demand. This approach is particularly effective for serverless workloads, which are typically short-lived and have minimal concurrent invocations.

## Copywrite

CFS-LLF extension by Al Amjad Tawfiq Isstaif

Copyright (C) 2023 Al Amjad Tawfiq Isstaif <alamjad.isstaif@cl.cam.ac.uk>
