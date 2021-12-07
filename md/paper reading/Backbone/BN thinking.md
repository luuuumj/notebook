1. BN 训练过程先 forward，再更新mean，std两个参数，这两个参数只在inference有用
2. 训练过程中BN在batch内可以认为是线性的，但在batch之间是非线性的
3. 为什么BN两个逆操作不能合并？
   1. 第一个-mean /std 是data dependent 的，不同的batch并不能学会  f(x) = (x-mean)/std * alpha + beta  = a * x + b这个操作 
   2. 为了保持输出 是稳定的 alpha beta 分布
   3. 而之所以是逆操作，是为了在inference过程中能够直接吸掉
4. why bn work？
   1. **internal covariate shift** (ICS)
   2. mitigates the interdependency between layers during training
   3. **optimization landscape smoothness**



reference

https://towardsdatascience.com/batch-normalization-in-3-levels-of-understanding-14c2da90a338#b93c

https://blog.csdn.net/sinat_33741547/article/details/87158830