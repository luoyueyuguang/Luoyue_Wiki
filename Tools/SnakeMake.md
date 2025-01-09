## reference
[slides](https://slides.com/johanneskoester/snakemake-tutorial)
[snakemake](https://snakemake.github.io/)
## 什么是SnakeMake
Snakemake 遵循[GNU Make](https://www.gnu.org/software/make)范式：工作流根据规则定义，这些规则定义如何从输入文件创建输出文件。规则之间的依赖关系是自动确定的，从而创建可自动并行化的作业 DAG（有向无环图）。nakemake 与其他基于文本的工作流系统的区别在于：Snakemake 连接到 Python 解释器，提供了一种定义语言，它是[Python](https://www.python.org/)的扩展，具有定义规则和工作流特定属性的语法。
## 一些简介(slides里截的)
![[Pasted image 20250107222330.png]]
- 规则定义
![[Pasted image 20250107222524.png]]
![[Pasted image 20250107222544.png]]
![[Pasted image 20250107222607.png]]
![[Pasted image 20250107222651.png]]
![[Pasted image 20250107222706.png]]
![[Pasted image 20250107222737.png]]![[Pasted image 20250107222828.png]]
![[Pasted image 20250107223101.png]]
![[Pasted image 20250107223117.png]]![[Pasted image 20250107223155.png]]
![[Pasted image 20250107223721.png]]
![[Pasted image 20250107224054.png]]
![[Pasted image 20250107224420.png]]
![[Pasted image 20250108154220.png]]![[Pasted image 20250108154241.png]]
![[Pasted image 20250108154338.png]]
![[Pasted image 20250108154409.png]]
![[Pasted image 20250108154617.png]]![[Pasted image 20250108154755.png]]
![[Pasted image 20250108155147.png]]
![[Pasted image 20250108155226.png]]
![[Pasted image 20250108155947.png]]
![[Pasted image 20250108160042.png]]
![[Pasted image 20250108160210.png]]
![[Pasted image 20250108160257.png]]
![[Pasted image 20250108160357.png]]
![[Pasted image 20250108160529.png]]![[Pasted image 20250108160547.png]]

## 命令行参数
- cache存放在`$XDG_CACHE_HOME`中
- `--cores`指定核数
- `-s`指定snakefile
- `-n`dry-run
- `--set-threads rule=<num>`设置rule的线程数
- `--set-resources myrule:partition="foo"`设置res数
- `--profile myprofile`myprofile文件夹里是配置文件，里面可以放yaml文件