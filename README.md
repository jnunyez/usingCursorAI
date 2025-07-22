# usingCursorAI
As a telco partner architect, my role often requires to complex Openshift-related technical questions, ranging from network protocols to infrastructure, and cloud-native architectures. The ability to delve into the underlying code of systems and applications can often provide an unparalleled level of insight.

Rather than using Cursor AI to write code I will use this tool to demistify complex codebases. In cloud-native telco systems different components interact in complex ways. The goal is to understand whether Cursor AI can help me to quickly architect and troubleshoot solutions more effectively, being able to quickly comprehend relevant code sections. We are gonna try to answer this question with a practical example. How can CursorAI help me to understand the details on how openshift Topology Manager works?

## An Example: How does Topology Manager works? 

First impression: It writes diagrams from code!
[Architecture Overview](imgs/arch.png)

<figcaption class="figure-caption text-center">

**Figure 1** Topology manager Architecture.

</figcaption>

### Architecture Overview
```mermaid
graph TB
A[Pod Admission Request] --> B[Topology Manager]
B --> C{Policy Type?}
C -->|none| D[None Policy<br/>Always allows allocation]
C -->|best-effort| E[Best Effort Policy<br/>Prefers NUMA alignment<br/>but allows allocation anyway]
C -->|restricted| F[Restricted Policy<br/>Only allows preferred<br/>NUMA aligned allocation]
C -->|single-numa-node| G[Single NUMA Node Policy<br/>Only allows allocation on<br/>single NUMA node]
B --> H{Scope Type?}
H -->|container| I[Container Scope<br/>Hints calculated per container]
H -->|pod| J[Pod Scope<br/>Hints calculated for entire pod]
B --> K[HintProviders]
K --> L[CPU Manager]
K --> M[Memory Manager]
K --> N[Device Manager]
L --> O[CPU Topology Hints]
M --> P[Memory Topology Hints]
N --> Q[Device Topology Hints]
O --> R[Hint Merger]
P --> R
Q --> R
R --> S[Best Hint Selection]
S --> T{Admit Pod?}
T -->|Yes| U[Allocate Resources<br/>Store Topology Hints]
T -->|No| V[Reject Pod<br/>TopologyAffinityError]
U --> W[Update Container Runtime]
U --> X[Update Metrics]
```
