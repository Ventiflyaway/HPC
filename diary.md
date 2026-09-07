## week 1
### 26/8/10: 
- had lesson 1 of CS267, reviewed laws learnt in CSC3050 

### 26/8/13：
- lesson 2 of CS267, 
    - reviewed mem hirerarchy
    - sigle processor 并行：ILP/pipelining、SIMD、FMA（虽然严格上不算并行，只是fused inst）
    - 目标是优化q = CI = f/m，经典例子是tiling优化MM（根据tiling for具体东西决定大小b）
    - 其他优化的手段：pad防止conflict miss/unrolling/把值存到local变量减少访存/把地址指针变动修改成base array访问减少地址计算依赖链

### 26/8/14：
- HW1 environment setup
- 用Ubuntu+WSL环境，cmake version 3.28.3

## week 2:
### 26/8/25：
- lesson 3 of CS267
    - RMM(Recursive Matrix Mutiply)  - 最少搬运数据：O( n^3 / 根号(Mfast))
    - roofline model:upper bound of performance

- pre-proposal of projects:
    - （1）Parallel Ray Tracing 如何更快地渲染一张复杂场景？（rays/pixels 天然并行）
    - （2）Parallel ML Inference 小型 NN/Transformer inference 中哪些操作值得并行？（GEMM/attention/token/batch parallelism）
1 好像可以和CSC4140 computer graphics结合起来！

#### 26/8/27-28：
- lesson 4 of CS267: 大致了解了OpenMP的用法，后续作业实战真正练手看掌握程度！
    - OpenMP:一套共享内存并行编程标准/API
    - (1) parallel: 会遇到的问题 true and false sharing（解决：pad/synchronizing）
``` bash
High level synchronization: critical/ barrier/ atomic/ ordered  
Low level synchronization: flush/ locks (both simple and nested)
```
    - (2) parallel loop: 
```bash
- loop worksharing ：`#pragma omp for/#pragma omp parallel for`自动分workload
- schedule:静态/动态（一个个拿workload，处理完接着拿；需要动态schedule的情况：工作量不均匀）
```
    - (3) data sharing: 
```bash
- shared/private/firstprivate
- reduction
- task、single
```
## week 3:
#### 26/9/1:
- lesson 5 of CS267: simulation的4种分类
    - discrete event
    - particle systems
    - lumped variables depending on continuous parameters: ODE（Star Wars: The Force Unleashed）,spice curcuit
    - continunous variables depending on continuous parameters: PDE, heat（Terminator 3: Rise of the Machines）
如何把现实中的连续物理问题（尤其是 PDE）离散成 mesh/grid 上的计算

#### 26/9/3:
- lesson 6 of CS267：从n-body到3D到general，讲解通过 tiling/blocking 最大化数据复用
（其实lesson 5、6的公式看的不甚明白，需要复习）

#### 26/9/6：
- 收尾HW1：在研究avl的时候，对比了rvv可能的写法：
    - RVV scalable vector 最漂亮的地方之一：tail 可以通过改变 VL 自然处理，不一定需要现在的cleanup（ 8 → 4 → scalar ）
- 需要一个完整的writeup

#### 26/9/7:
- lesson 7 of CS267: GPU & CUDA!终于！
```
1. GPUs gain efficiency from simpler cores and more parallelism
  - Very wide SIMD (SIMT) for parallel arithmetic and latency-hiding
2. Heterogeneous（异构的） programming with manual offload:CPU to run OS, etc. GPU for compute 
  - 一个系统里使用不同类型的处理器，让它们各自做擅长的工作
3. GPU 想获得高性能，需要大量并行任务，而且主要是 data parallelism
  - Not as strict as CPU-SIMD，因为=>
  - 1. divergent addresses：不同 thread 可以访问不同地址
  - 2. instructions：不同 thread 也可以走不同分支
4. 同一个 Block 里的 threads 可以共享高速的 shared memory，并且可以用 barrier 同步；同一个 kernel 的不同 Blocks 之间，通常只能通过较慢的 device/global memory 共享数据，并通过 atomic operations 来协调。
```