
## Linux

- `pwd` 显示当前所在目录
- `cd` 更改当前目录
- `mkdir` 创建目录
- `rmdir` 删除目录
- `cp source dest` 
- `netstat -ntlp` 查看监听端口
- `lscpu` 查看 cpu 信息

## ls

- `ls [OPTION]... [FILE]...` 查看当前目录下的文件
- `-l` 详细显示文件 `drwx------ 3 r·oot root 4096 Jul 31 06:30 snap`
- `-a` 显示隐藏文件
- `-t` 按照时间顺序

## systemctl

- `systemctl start mysql` 启动mysql服务
- `systemctl enable mysql` 设置mysql自启动
- `systemctl status mysqld` 查看mysql状态
- `systemctl restart mysql` 重启mysql服务
- `systemctl restart mysql` 停止mysql服务
- `getconf PAGE_SIZE` 查看页大小

## lm-sensors

- 温度监控

# ufw

- 防火墙
- `ufw delete allow 22/tcp`

