## 工具
- SYCLomatic：将CUDA代码迁移到SYCL的开源工具。
- Intel DPC++ Compatibility
  Intel产品，可能有自己的功能
## 简单项目的迁移
``` shell
cd PRJ_ROOT
dpct/c2s --in-root=./ --out-root=/path/to/output cuda/sample.cu--extra-arg="-l./include" --keep-original-code*.cu
```
``` mermaid
graph TB
a[PRJ_ROOT] --- b[include]
a --- c[cuda]
a --- d[. . .]
```
### 基本选项

|Basic Options|function|
|-------------|--------|
|--in-root| 源代码文件路径|
|--out-root|生成文件的路径|
|-p|指定编译数据库文件|
|--process-all|迁移所有文件从--in-root到--out-root,不用单独指定文件。|
|--extra-arg|指定编译选项|
|--keep-original-code|在迁移后的SYCL文件中，原来的CUDA代码以注释形式存在|
|--cuda-include-path|CUDA头文件路径|
### USM选项
|usm选项|
|---|---|
|--usm-level|=Restricted(default):使用USM|
||=note:Use helper functions and SYCL buffers|


## Make/Cmake项目的迁移
``` mermaid
graph TB
a[准备:Intercept-Build]-->b[迁移:dpct/c2s]-->c[审核:验证与手动编辑]
```

1. 创建编译数据库文件
   *intercept -build make*
2. 迁移代码:
   *dpct/c2s-p=<directory of compilation database \*.json file>*
3. 验证代码的正确性并修改未迁移的部分
### 产生Makefile的选项
- --gen-build-script
	- 生成迁移文件的makefile
- --build-script-file=<file\>
	- 指定生成的Makefile的名字
## CUDA library的迁移
一些CUDA的库也可以被迁移
具体可查看[CUDA API Migration Support Status (intel.com)](https://www.intel.com/content/www/us/en/docs/dpcpp-compatibility-tool/developer-guide-reference/2023-2/cuda-api-migration-support-status.html)

## 诊断信息
迁移工具会生成诊断信息。
## 一般最佳方法
- 逐步迁移
	- 在一次运行中迁移大量CUDA源文件列表时， 如果看到dpct产生了多个错误，那么需要逐个迁移。
- 首先从clean项目 - "make clean"开始， 然后运行"intercept-build make"
- 如果生成编译数据库时不能生成某些目标，请运行intercept-build make -k 以继续。