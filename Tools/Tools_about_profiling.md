# perf
- `sudo dnf install perf`红帽系
- `perf record`
	生成perf.data
	- `-F:指定采样频率`
	- `-a：指定所有cpu`
	- `-g:栈踪迹`
- `perf script`
	使用这个命令来加载
- `perf trace`
	跟踪系统调用
	- `-s:输出系统调用汇总`
	- `-e:过滤器指定事件`

# profile
`-F:指定频率`
# offcputime

# hyperfine
[hyperfine](https://github.com/sharkdp/hyperfine)
一个rust写的有趣的benchmark工具
- `hyperfine <command>...`用来运行命令
- `hyperfine -runs [times] <command>`
- `hyperfine <command1> <command2>`
- `hyperfine --warmup/-w　[times] <command>`使用暖身,暖cache,多跑几次
- `hyperfine --prepare/-p <command>`使用冷cache,在测试前跑某些命令
- `hyperfine -P/--parameter-scan　<VAR> <MIN> <MAX> <command>`从min到max代入到var里跑,例如`hyperfine -P a 1 5 'echo $a'`
- `hyperfine --parameter-scan <VAR> <MIN> <MAX> <command> --parameter-step-size/-D <DELTA>`给定步距` hyperfine --parameter-scan delay 0.3 0.7 -D 0.2 'sleep {delay}'`
- `hyperfine --parameter-list <VAR> <VALUES> <command>`给出不同参数来跑.
- `hyperfine --shell/-S <shell>`选用shell
- `hyperfine -N <command>`不用shell
- `hyperfine`还支持导出数据
# lmbench
在`ostep`里看到的,可以测很多东西
*reference:[https://www.cnblogs.com/ylxtiankong/p/18159724]*
gcc-14.1生成时出现了一些error,`socklen_t`define就好,rpc有关的需要下载`tirpc`
``` shell
make #编译
make results　#做测试,生成结果
make see　#查看结果
```


# vtune
[my tutorial of vtune](Tools/vtune_tutorial)
