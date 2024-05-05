## device类
``` cpp
queue q;
device my_device = q.get_device();
std::cout << "Device" << my_device.get_info<info::device::name>() << std::endl;
```

## selector类
``` cpp
default_selector selector;
//host_selector selector
//cpu_selector selector
//gpu_selector selector
//也可以自定义selector
queue q(selector);
std::cout << "Device:" << q.get_device().get_info<info::device::name>() << std::endl;
```

## queue类
queue类不声明selector的话，device默认是由sycl runtime根据环境变量指定。
``` cpp
queue q;
q.submit([&](handler& h){
	//COMMAND GROUP CODE
})
```

## examples
``` cpp
h.parallel_for(nd_range<1> (range<1> (1024), range<1>(64), [=](nd_item<1> item){
	auto idx = item.get_global_id();
	auto local_id = item.get_local_id();
	// CODE THAT RUNS ON DEVICE
} 
```

## USM和buffer


## 同步
### Host Accessors
``` cpp
#include <CL/sycl.hpp>
using namespace sycl;
constexpr int N = 16;

int main()
{
	std::vector<double> v(N, 10);
	queue q;

	buffer buf(v);
	//利用buffer转移所有权
	q.submit([&](handler& h){
		accessor a(buf, h)
		h.parallel_for(N, [=](auto i){
			a[i] -= 2;
		});
	});

	host_accessor b(buf, read_only);
	//
	for(int i = 0; i < N; i++)
	{
		std::cout << b[i] << "\n";
	}
	return 0;
}
```

### Buffer析构
``` cpp
#include <CL/sycl.hpp>
using namespace sycL;
constexpr int N = 16;

void dpcpp_code(std::vector<double> &v, queue &q)
{
buffer buf(v);
q.submit([&](handler &h){
	accessor a(buf, h);
	h.parallel_for(N, [=](auto i){
		a[i] -= 2;
		});
	});
}

int main()
{
	std::vector<double> v(N, 10);
	queue q;
	dpcpp_code(v, q);
	//dpcpp_code的作用域消失后， 触发buffer的析构函数， 可以访问v[i]。
	for(int i = 0; i < N; i++)
	{
		std::cout << v[i] << "\n";
	}
	return 0;
}
```