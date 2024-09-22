# kernel
## function
1. 核函数在GPU上并行执行
2. 限定词
3. 返回值是void
## 注意事项
1. 核函数只能访问GPU内存
2. 核函数不能使用变长参数
3. 核函数不能使用静态变量
4. 核函数不能使用函数指针
5. 核函数 具有异步性
## 流程
``` cpp
int main(void)
{
	//host code
	//kernel
	//host code
	return 0;
}
```

# 线程模型
1. 线程模型重要概念
	-  grid 网格
	- block 线程块
2. `<<<grid_size, block_size>>>`
3. 线程块最大大小：1024
    网格最大大小：$2^{31} -1$
5. `grid_size`:gridDim.x
    `block_size`:blockDim.x
6. `blockIdx.x`:$[0,\space gridDim.x-1]$
   `threadIdx.x`:$[0,\space threadIdx.x]$
## 多维线程
``` cpp
int tid = threadIdx.y * blockDim.x + threadIdx.x
int bid = blockIdx.y * gridDim.x + blockIdx.x

int tid = threadIdx.z * blockDim.x * blockDim.y + 
		  threadIdx.y * blockDim.x + threadIdx.x
int bid = blockIdx.z * gridDim.x * gridDim.y + 
		  blockIDX.y * gridDim.x + blockIdx.x
```
## 限制
- `gridDim.x`:$2^{31} - 1$
- `gridDim.y`:$2^{16} - 1$
- `gridDim.z`:$2^{16} - 1$
 
- `blockDim.x`:$1024$
- `blockDim.y`:$1024$
- `blockDim.z`:$64$


`__host__device__ cudaError_t cudaGetDeviceCount(int* count)`
>获取GPU设备数量

`__host__ cuda_Error_t cudaSetDevice(int devive)`
>设置GPU执行时使用的设备代码

## CUDA内存管理函数
`__host__device__ cudaError_t cudaMalloc(void **devPtr, size_t size)`
>`float *fpHost_A;
>`fpHost_A = (float*)malloc(nBytes);`

`__host__ cudaError_t cudaMemcpy(void *dst, const void* src, size_t count, cudaMemcpyKind kind)`
>`kind:`
>	`cudaMemcpyHostToHost:主机->主机` 
>	`cudaMemcpyHostToDevice:主机->设备
>	`cudaMemcpyDeviceToHost:设备->主机` 
>	`cudaMemcpyDeviceToDecide:设备->设备` 
>`cudaMemcpyDeafault 默认寻址, 在统一虚拟寻址设备中可用`
>具有隐式同步


`__host__cudaError_t cudaMemset(void* devPtr, int value, size_t count`)
>设备内存初始化代码：
>`cudaMalloc(fpDevice_A, 0, nBytes);`

`__host__ __device__ cudaError_t cudaFree(void* devPtr)`
>释放内存


# CUDA错误返回
返回值类型：`cudaError_t`
```
cudaSuccess = 0
cudaErrorInvalidValue = 1
cudaErrorMemoryAllocation = 2
cudaErroInitializationError = 3
cudaErrorCudaUnloading = 4
```


``` c
#ifdef ERR_CUH
#define ERR_CUH

#include <stdio.h>

#define CHECK(call)                                   \
do                                                    \
{                                                     \
    const cudaError_t error_code = call;              \
    if (error_code != cudaSuccess)                    \
    {                                                 \
        printf("CUDA Error:\n");                      \
        printf("    File:       %s\n", __FILE__);     \
        printf("    Line:       %d\n", __LINE__);     \
        printf("    Error code: %d\n", error_code);   \
        printf("    Error text: %s\n",                \
            cudaGetErrorString(error_code));          \
        exit(1);                                      \
    }                                                 \
} while (0)

#endif

```
宏定义末尾有\


检查核函数
```
CHECK(cudaGetLastError());
CHECK(cudaDeviceSynchronize());
```


CUDA-MEMCHECK检查内存错误
``` shell
$cuda-memcheck --tool memcheck [options] app_name [options]
$cuda-memcheck --tool racecheck [options] app_name [options]
$cuda-memcheck --tool initcheck [options] app_name [options]
$cuda-memcheck --tool synccheck [options] app_name [options]
```

对kernel进行查错
``` shell
cuda-memcheck [options] app_name [options]
```

>参考: https://zhuanlan.zhihu.com/p/524143632

# 计时

``` c
cudaEvent_t start, stop;
    
CHECK(cudaEventCreate(&start));
CHECK(cudaEventCreate(&stop));
CHECK(cudaEventRecord(start));

CHECK(cudaEventQuery(start));//TCC驱动可省略

//code...  
CHECK(cudaEventRecord(stop));
CHECK(cudaEventSynchronize(stop));

float elapsed_time;

CHECK(cudaEventElapsedTime(&elapsed_time, start, stop));

CHECK(cudaEventDestroy(start));
CHECK(cudaEventDestroy(stop));
```

# CUDA程序的基本框架
```
头文件包含
常量定义（或宏定义）
c++ 自定义函数和核函数的声明（原型）
int main(void)
{
	分配主机与设备内存
	初始化主机中的数据
	将某些数据从主机复制到设备
	调用核函数在设备中进行计算
	将某些数据从设备复制到主机
	释放主机与设备内存
}
c++自定义函数和CUDA核函数的定义（实现）
```

# 一些构造技巧
``` cpp
int grid_size = (N -1) / block_size + 1;
or
int 
int grid_size = (N + block_size -1) / block_size;
```

``` cpp
const int i = blockDim.x * blockIdx.x + threadIdx.x;
```

## 内联与非内联
``` cpp
__noline__
__forceline__
```

# 双精单精宏
``` cpp
#ifdef USE_DP
	typedef double real;
	const real EPSILON = 1.0e - 15
```

# 查看内存使用情况
`nvcc --resource`

# CUDA中不同类型的内存
## 全局内存
全局内存具有较高的延迟和较低的访问速度。
一个网格中的所有线程都可以访问（读或写）传入核函数的设备指针所指向的全局内存中的全部数据。
在处理逻辑上的两维或三维问题时，使用cudaMallocPitch()和cudaMalloc3D()。
## 常量内存
有常量缓存的全局内存。
数量有限，仅有64KB,可见范围与生命周期与全局范围一样，仅可读，不可写。
访问速度比全局内存高。前提是一个线程束中的线程要读取相同的常量内存数据。
使用方法
- 在kernel外，使用__constant__
给核函数传递的参数存放在常量内存中。

## 纹理内存和表面内存
类似与常量内存，有相同的可见范围和生命周期，内存容量更大。

## 共享内存
``` cpp
__shared__ type s_array[]
```
>共享变量的生存周期仅仅在核函数内

### 动态共享内存
``` cpp
<<<grid_size, block_size, sizeof(real) * block_size>>>
```
第三个参数是核函数中每个线程块需要定义的动态共享内存的字节数。

>数组可声明为指针。

使用extern
``` cpp
extern __shared__ type s_array[]
```

## 线程束，协作组函数
http://zh0ngtian.tech/posts/ada27037.html
这个就行，懒得写了，状态好差。


