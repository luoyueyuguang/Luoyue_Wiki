日志
- **journalctl -r**：按时间顺序，倒序显示所有日志内容。
- **journalctl -u _`UNIT`_**: 显示与给定单元文件 UNIT 关联的日志。
- **journalctl -b[=ID] -r**: 按时间倒序，显示自上次引导以来 (或编号为 ID 的引导中) 的所有日志。
- **journalctl -f**: 提供类似 tail -f 的功能 (不断将新日志项输出到屏幕)。
***
coredump
- coredumpctl -r：按时间顺序，倒序显示所有核心转储记录。
- coredumpctl -1 info：显示最近一次核心转储的信息。
- coredumpctl -1 debug：将最后一次核心转储加载到 GDB 中。


