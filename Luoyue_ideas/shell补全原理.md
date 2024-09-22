## shell命令行补全的原理
在如`bash`或`zsh`这样的shell中,命令行补全功能是由一些库实现的,`bash`是`readline`,`zsh`是`zle`.

`readline`里的补全依靠是`rl_complete`这个函数,`rl_complete`调用`rl_complete_internal`,`rl_complete_internal又要调用其他函数。



## GPT log
- Q:找到shell源代码中，负责命令行补齐的源代码

