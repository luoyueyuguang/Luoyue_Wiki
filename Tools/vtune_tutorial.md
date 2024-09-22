## 需要收集某些信息时起用要求
```
echo 1 > /proc/sys/kernel/perf_event_paranoid
echo 0 > /proc/sys/kernel/kptr_restrict
echo 0 > /proc/sys/kernel/yama/ptrace_scope
```
## 生成报告
``` bash
vtune -collect [variety] -r [result_dir]
```

``` bash
vtune -report hotspots -r result_directory
```