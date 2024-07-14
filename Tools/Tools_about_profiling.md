# perf
红帽系
- `sudo dnf install perf`hong
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
- `hyperfine --warmup/-w`使用暖身,暖cache
- `hyperfine --prepare/-p`使用冷cache
- `hyperfine -P/--parameter-scan`














 