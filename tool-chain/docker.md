# Docker

## 为什么使用Docker？

- 简化了重复搭建开发环境的工作
- 交付系统流畅
- 伸缩性好

常用命令
- `docker pull <image>`
- `docker search <image>`
- `docker start <容器名>`
- `docker stop <容器名>`

## docker run

选项说明
- `-d` 后台运行容器
- `-e` 设置环境变量
- `--expose / -p` 宿主端口 `:` 容器端口
- `--name` 指定容器名称
- `--link` 链接不同容器
- `-v` 宿主目录 `:` 容器目录 挂载磁盘卷

## 常用命令

`docker exec -it 容器 bash` 在容器中执行bash命令