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

### 26/8/25：
- lesson 3 of CS267
    - RMM(Recursive Matrix Mutiply)  - 最少搬运数据：O( n^3 / 根号(Mfast))
    - roofline model:upper bound of performance

- pre-proposal of projects:
    - （1）Parallel Ray Tracing 如何更快地渲染一张复杂场景？（rays/pixels 天然并行）
    - （2）Parallel ML Inference 小型 NN/Transformer inference 中哪些操作值得并行？（GEMM/attention/token/batch parallelism）
1 好像可以和CSC4140 computer graphics结合起来！

#### 26/8/27-28：
- lesson 4 of CS267
    - OpenMP:一套共享内存并行编程标准/API
    - (1) parallel: 会遇到的问题 true and false sharing（解决：pad/synchronizing）
``` bash
High level synchronization:
- critical
- barrier
- atomic：make the read/update of a memory location atomic
- ordered  
Low level synchronization
- flush: enforces a consistent view of memory.每个thread各自更新内存view，不相互等待
- locks (both simple and nested)
```
    - (2) parallel loop: 
```bash
- loop worksharing ：`#pragma omp for/#pragma omp parallel for`自动分workload
- schedule:静态/动态（一个个拿workload，处理完接着拿；需要动态schedule的情况：工作量不均匀）
- 
```
    - (3) data sharing: 
```bash
- shared/private/firstprivate
- reduction
- task、single
```


