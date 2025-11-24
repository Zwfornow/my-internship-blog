# Linux 命令手册与实用技巧

## 文件与目录操作

### 基础文件操作
```bash
# 查看目录内容（详细信息）
ls -la

# 按时间排序查看
ls -lath

# 按文件大小排序
ls -laS

# 创建多层目录
mkdir -p project/{src,dist,test}

# 复制并保留文件属性
cp -a source destination

# 安全删除（到回收站）
alias rm='trash-put'
```

### 文件查找与搜索
```bash
# 按文件名查找
find /path -name "*.log" -type f

# 按文件大小查找
find . -size +100M -exec ls -lh {} \;

# 查找并删除
find /tmp -name "*.tmp" -mtime +7 -delete

# 查找包含特定内容的文件
grep -r "error" /var/log/

# 忽略二进制文件
grep -r --binary-files=without-match "pattern" .

# 查找空文件和空目录
find . -empty
```

## 文本处理

### grep 高级用法
```bash
# 显示匹配行及前后3行
grep -A 3 -B 3 "error" file.log

# 只显示匹配的文件名
grep -l "pattern" *.js

# 反向匹配（不包含模式的行）
grep -v "debug" file.log

# 正则表达式匹配
grep -E "^[0-9]{3}-[0-9]{2}-[0-9]{4}" file.txt

# 忽略大小写
grep -i "error" file.log

# 统计匹配行数
grep -c "pattern" file.txt
```

### sed 流编辑器
```bash
# 替换文本
sed 's/old/new/g' file.txt

# 原地修改文件
sed -i.bak 's/old/new/g' file.txt

# 删除包含模式的行
sed '/pattern/d' file.txt

# 只打印匹配的行
sed -n '/pattern/p' file.txt

# 在指定行后插入
sed '5a\插入的内容' file.txt

# 多个编辑操作
sed -e 's/foo/bar/g' -e '/baz/d' file.txt
```

### awk 数据处理
```bash
# 打印特定列
awk '{print $1, $3}' file.txt

# 条件过滤
awk '$3 > 100 {print $1, $3}' data.txt

# 使用分隔符
awk -F',' '{print $2}' data.csv

# 计算统计
awk '{sum += $1} END {print sum}' numbers.txt

# 分组统计
awk '{count[$1]++} END {for (item in count) print item, count[item]}' data.txt

# 复杂处理
awk 'BEGIN {FS=","; OFS="\t"} NR>1 {print $1, $2*$3}' sales.csv
```

## 系统监控

### 进程管理
```bash
# 查看进程树
pstree -p

# 按内存使用排序
ps aux --sort=-%mem | head -10

# 按CPU使用排序
ps aux --sort=-%cpu | head -10

# 查找特定进程
pgrep -f "nginx"

# 杀死进程树
pkill -9 -P <parent_pid>

# 实时监控进程
htop
```

### 系统资源监控
```bash
# 磁盘使用情况
df -h

# 目录大小
du -sh /path/to/dir
du -h --max-depth=1 /path

# 内存信息
free -h

# 系统负载
uptime

# 监控文件变化
watch -n 1 'ls -la'

# 网络连接
netstat -tulpn
ss -tulpn
```

### 性能分析工具
```bash
# CPU 使用率监控
mpstat 1 5

# I/O 统计
iostat -x 1

# 网络流量监控
iftop

# 系统调用跟踪
strace -p <pid>

# 内存泄漏检测
valgrind --leak-check=yes ./program
```

## 网络工具

### 网络诊断
```bash
# 查看路由表
route -n
ip route show

# 端口扫描
nc -zv hostname 22
nmap -p 22,80,443 hostname

# 网络速度测试
speedtest-cli

# 持续 ping
ping -c 10 google.com
mtr google.com

# HTTP 请求
curl -I https://example.com
curl -X POST -d 'data' https://api.example.com
```

### SSH 高级用法
```bash
# 密钥认证登录
ssh -i ~/.ssh/id_rsa user@host

# 端口转发
ssh -L 8080:localhost:80 user@host
ssh -R 9000:localhost:3000 user@host

# SOCKS 代理
ssh -D 1080 user@host

# 执行远程命令
ssh user@host 'ls -la /tmp'

# 免密码登录设置
ssh-copy-id user@host
```

## 实用脚本

### 备份脚本
```bash
#!/bin/bash
# 自动备份脚本

BACKUP_DIR="/backup"
SOURCE_DIR="/home/user/data"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="backup_$DATE.tar.gz"

# 创建备份
tar -czf "$BACKUP_DIR/$BACKUP_FILE" "$SOURCE_DIR"

# 删除7天前的备份
find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +7 -delete

# 记录日志
echo "$(date): Backup completed - $BACKUP_FILE" >> /var/log/backup.log
```

### 日志分析脚本
```bash
#!/bin/bash
# Nginx 日志分析

LOG_FILE="/var/log/nginx/access.log"

echo "=== Top 10 IP Addresses ==="
awk '{print $1}' "$LOG_FILE" | sort | uniq -c | sort -nr | head -10

echo -e "\n=== Top 10 Requested URLs ==="
awk '{print $7}' "$LOG_FILE" | sort | uniq -c | sort -nr | head -10

echo -e "\n=== Response Status Codes ==="
awk '{print $9}' "$LOG_FILE" | sort | uniq -c | sort -nr

echo -e "\n=== Requests per Hour ==="
awk '{print $4}' "$LOG_FILE" | cut -d: -f2 | sort | uniq -c
```

### 系统监控脚本
```bash
#!/bin/bash
# 系统健康检查

echo "=== System Health Check ==="
echo "Uptime: $(uptime)"
echo "Load Average: $(cat /proc/loadavg)"
echo -e "\n=== Memory Usage ==="
free -h
echo -e "\n=== Disk Usage ==="
df -h | grep -v tmpfs
echo -e "\n=== Top Processes by CPU ==="
ps aux --sort=-%cpu | head -5
echo -e "\n=== Top Processes by Memory ==="
ps aux --sort=-%mem | head -5
```

## 开发环境配置

### Shell 配置 (~/.bashrc)
```bash
# 自定义提示符
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# 常用别名
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias grep='grep --color=auto'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'

# 安全别名
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Git 别名
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gl='git log --oneline --graph'

# 快速目录跳转
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# 开发工具
alias py='python3'
alias pip='pip3'
```

### Vim 配置 (~/.vimrc)
```vim
" 基础设置
set number
set relativenumber
set tabstop=4
set shiftwidth=4
set expandtab
set smartindent
set nowrap
set smartcase
set noswapfile
set nobackup
set undodir=~/.vim/undodir
set undofile
set incsearch

" 键位映射
let mapleader = " "
nnoremap <leader>h :wincmd h<CR>
nnoremap <leader>j :wincmd j<CR>
nnoremap <leader>k :wincmd k<CR>
nnoremap <leader>l :wincmd l<CR>
nnoremap <leader>u :UndotreeShow<CR>

" 插件管理 (vim-plug)
call plug#begin('~/.vim/plugged')
Plug 'preservim/nerdtree'
Plug 'vim-airline/vim-airline'
Plug 'tpope/vim-fugitive'
Plug 'mbbill/undotree'
call plug#end()
```

## 故障排查

### 常见问题解决
```bash
# 端口被占用
lsof -i :3000
kill -9 $(lsof -t -i:3000)

# 磁盘空间不足
find /var/log -name "*.log" -size +100M -exec du -h {} \;

# 内存泄漏检测
cat /proc/meminfo
cat /proc/slabinfo

# 系统日志分析
journalctl -f
tail -f /var/log/syslog

# 服务状态检查
systemctl status nginx
systemctl restart nginx
```

## 命令速查表

| 类别 | 常用命令 | 用途 |
|------|----------|------|
| 文件操作 | `ls`, `cp`, `mv`, `rm`, `find` | 文件管理 |
| 文本处理 | `grep`, `sed`, `awk`, `sort`, `uniq` | 文本分析 |
| 系统监控 | `top`, `htop`, `ps`, `free`, `df` | 性能监控 |
| 网络工具 | `curl`, `wget`, `ssh`, `scp`, `netstat` | 网络操作 |
| 进程管理 | `kill`, `pkill`, `pgrep`, `nohup` | 进程控制 |
| 权限管理 | `chmod`, `chown`, `sudo`, `su` | 权限设置 |

> **💡 学习建议**：
> - 每天练习几个新命令
> - 编写脚本自动化重复任务
> - 理解命令的参数和选项
> - 学习正则表达式提升文本处理能力