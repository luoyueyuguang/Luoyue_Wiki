# 安装rustup
- 官方安装
`curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
- arch linux
`yay -S rustup`
# vscode插件
`rust-analyzer`
# Cargo

- 创建项目
`cargo new project_name`

- 运行项目
```
cd project_name
cargo run
```
run会将编译和运行都做了
- 编译和运行项目
```
cargo build
././target/debug/project_name
```
- 指定版本
```
cargo run --release
cargo build --release
./target/release/project_name
cargo run --debug
cargo build --debug
././target/debug/project_name
```
默认为debug
- 检查能否编译通过
`cargo check`检查能否编译通过，用来节省编译时间
- `Cargo.toml`与`Cargo.lock`
	1. `Cargo.toml` 是 `cargo` 特有的 **项目数据描述文件**
	2. `Cargo.lock` 文件是 `cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**
