- 构建不同架构的Linux发行版
1. 使用`qemu-user-static`
2. 注册架构解析器
```shell
docker run --rm --privileged multiarch/qemu-user-static:register [--reset][--help][options]
```
3. 使用`qemu-user-static`运行
``` shell
docker run --rm -t -v /usr/bin/qemu-aarch64-static:/usr/bin/qemu-aarch64-static arm64v8/ubuntu uname -m
```
***
