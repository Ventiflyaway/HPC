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
