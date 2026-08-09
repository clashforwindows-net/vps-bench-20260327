# VPS性能测试工具箱2026

> 全方位性能测试平台 | CPU/内存/磁盘/网络/综合评分

## 推荐 ⭐

**VPSVIP**: [https://vpsvip.net](https://vpsvip.net)
- 专业VPS推荐平台
- 高性能、低价格
- 优质客服支持

**相关资源**：
- [https://nav.clashvip.net](https://nav.clashvip.net) - 网络工具导航

---

## 📦 工具箱组成

本工具箱提供全面的VPS性能测试方案：

| 脚本 | 功能 | 适用场景 |
|------|------|----------|
| `bench.sh` | 综合性能测试 | 所有VPS |
| `bbr.sh` | BBR加速 | 需要优化的VPS |
| `ssh-hardening.sh` | SSH加固 | 安全加固 |

---

## 🚀 快速开始

```bash
# 克隆仓库
git clone https://github.com/clashforwindows-net/vps-bench-20260327.git
cd vps-bench-20260327

# 设置执行权限
chmod +x *.sh

# 运行综合性能测试
sudo ./bench.sh
```

---

## 📊 性能测试详解

### bench.sh 功能

综合测试脚本包含以下测试项目：

#### 1. 系统信息采集

```bash
# 系统信息
echo "=== 系统信息 ==="
echo "操作系统: $(uname -s)"
echo "内核版本: $(uname -r)"
echo "架构: $(uname -m)"
echo "主机名: $(hostname)"
echo "运行时间: $(uptime -p)"

# CPU信息
echo "=== CPU信息 ==="
model_name=$(cat /proc/cpuinfo | grep "model name" | head -1 | cut -d: -f2)
cores=$(nproc)
echo "型号: $model_name"
echo "核心数: $cores"

# 内存信息
echo "=== 内存信息 ==="
free -h

# 磁盘信息
echo "=== 磁盘信息 ==="
df -h
```

#### 2. CPU性能测试

```bash
# Geekbench 5（推荐）
echo "=== Geekbench 5 测试 ==="
wget https://cdn.geekbench.com/Geekbench-5.4.1-Linux.tar.gz
tar xzf Geekbench-5.4.1-Linux.tar.gz
cd Geekbench-5.4.1-Linux
./Geekbench5

# Sysbench CPU测试
echo "=== Sysbench CPU测试 ==="
sysbench cpu --cpu-max-prime=20000 --time=60 run

# 基础计算测试
echo "=== 基础计算测试 ==="
time echo "scale=4000; a(1)*4" | bc -l
```

**评分标准**：

| 评分 | Geekbench 5 单核 | Geekbench 5 多核 | 适用场景 |
|------|------------------|------------------|----------|
| ⭐⭐⭐⭐⭐ | >1500 | >8000 | 高性能计算、编译 |
| ⭐⭐⭐⭐ | 1000-1500 | 5000-8000 | 日常Web应用 |
| ⭐⭐⭐ | 600-1000 | 3000-5000 | 普通应用、轻量服务 |
| ⭐⭐ | 300-600 | 1500-3000 | 基础测试、学习 |
| ⭐ | <300 | <1500 | 勉强可用 |

#### 3. 内存性能测试

```bash
# Memtester（内存稳定性测试）
echo "=== 内存测试 ==="
memtester 1024 5

# 内存速度测试
echo "=== 内存速度测试 ==="
dd if=/dev/zero of=/tmp/test bs=1M count=1024 oflag=direct
rm -f /tmp/test

# Sysbench内存测试
echo "=== Sysbench内存测试 ==="
sysbench memory --memory-block-size=1M --memory-total-size=10G --memory-access-mode=rnd run
```

**评分标准**：

| 评分 | 读取速度 | 写入速度 | 适用场景 |
|------|----------|----------|----------|
| ⭐⭐⭐⭐⭐ | >10 GB/s | >5 GB/s | 大型缓存、数据库 |
| ⭐⭐⭐⭐ | 5-10 GB/s | 2.5-5 GB/s | 普通Web应用 |
| ⭐⭐⭐ | 2-5 GB/s | 1-2.5 GB/s | 轻量服务 |
| ⭐⭐ | 1-2 GB/s | 0.5-1 GB/s | 基础使用 |
| ⭐ | <1 GB/s | <0.5 GB/s | 勉强可用 |

#### 4. 磁盘性能测试

```bash
# 磁盘IO测试（dd）
echo "=== 磁盘写入测试 ==="
# 顺序写入测试
dd if=/dev/zero of=/tmp/test bs=1M count=1024 oflag=direct 2>&1 | grep -E "copied|MB/s"
rm -f /tmp/test

echo "=== 磁盘读取测试 ==="
# 创建测试文件
dd if=/dev/urandom of=/tmp/test bs=1M count=1024
# 顺序读取测试
dd if=/tmp/test of=/dev/null bs=1M iflag=direct 2>&1 | grep -E "copied|MB/s"
rm -f /tmp/test

# FIO（高级IO测试，需要安装）
echo "=== FIO高级测试 ==="
fio --name=randread --ioengine=libaio --rw=randread --bs=4k --numjobs=4 --size=1G --runtime=60 --group_reporting
fio --name=randwrite --ioengine=libaio --rw=randwrite --bs=4k --numjobs=4 --size=1G --runtime=60 --group_reporting
fio --name=seqread --ioengine=libaio --rw=read --bs=1M --numjobs=1 --size=1G --runtime=60 --group_reporting
fio --name=seqwrite --ioengine=libaio --rw=write --bs=1M --numjobs=1 --size=1G --runtime=60 --group_reporting
```

**评分标准（顺序读写）**：

| 评分 | 读取速度 | 写入速度 | 适用场景 |
|------|----------|----------|----------|
| ⭐⭐⭐⭐⭐ | >500 MB/s | >300 MB/s | 数据库、高IO应用 |
| ⭐⭐⭐⭐ | 300-500 MB/s | 150-300 MB/s | Web应用、内容管理 |
| ⭐⭐⭐ | 100-300 MB/s | 50-150 MB/s | 普通应用 |
| ⭐⭐ | 50-100 MB/s | 20-50 MB/s | 轻量服务 |
| ⭐ | <50 MB/s | <20 MB/s | 勉强可用 |

#### 5. 网络性能测试

```bash
# 带宽测试（Speedtest）
echo "=== 带宽测试 ==="
# 安装speedtest-cli
pip install speedtest-cli
# 或
wget -O speedtest-cli https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest_cli.py
chmod +x speedtest-cli

# 运行测试
./speedtest-cli --server 12345  # 指定服务器ID
./speedtest-cli --list | head -20  # 查看可用服务器

# 多节点测试
echo "=== 多节点测试 ==="
# 亚太节点
./speedtest-cli --server 1234
# 欧美节点
./speedtest-cli --server 5678

# 网络延迟测试
echo "=== 延迟测试 ==="
ping -c 10 8.8.8.8
ping -c 10 1.1.1.1

# 路由追踪
echo "=== 路由追踪 ==="
traceroute -I 8.8.8.8
traceroute -I 1.1.1.1
```

**评分标准（带宽）**：

| 评分 | 下载速度 | 上传速度 | 延迟 | 适用场景 |
|------|----------|----------|------|----------|
| ⭐⭐⭐⭐⭐ | >500 Mbps | >200 Mbps | <50ms | 4K流媒体、大文件传输 |
| ⭐⭐⭐⭐ | 200-500 Mbps | 100-200 Mbps | 50-100ms | 1080P、游戏 |
| ⭐⭐⭐ | 100-200 Mbps | 50-100 Mbps | 100-150ms | 普通浏览、日常 |
| ⭐⭐ | 50-100 Mbps | 20-50 Mbps | 150-200ms | 轻量服务 |
| ⭐ | <50 Mbps | <20 Mbps | >200ms | 勉强可用 |

#### 6. 综合评分算法

```bash
# 综合评分计算
calculate_score() {
    local cpu_score=$1      # CPU分数(0-100)
    local mem_score=$2      # 内存分数(0-100)
    local disk_score=$3     # 磁盘分数(0-100)
    local net_score=$4      # 网络分数(0-100)
    
    # 加权平均（可根据需求调整权重）
    local total_score=$(echo "scale=2; $cpu_score*0.25 + $mem_score*0.15 + $disk_score*0.25 + $net_score*0.35" | bc)
    
    echo "综合评分: $total_score/100"
    
    # 评级
    if (( $(echo "$total_score >= 90" | bc -l) )); then
        echo "评级: ⭐⭐⭐⭐⭐ 顶级推荐"
    elif (( $(echo "$total_score >= 75" | bc -l) )); then
        echo "评级: ⭐⭐⭐⭐ 优秀"
    elif (( $(echo "$total_score >= 60" | bc -l) )); then
        echo "评级: ⭐⭐⭐ 良好"
    elif (( $(echo "$total_score >= 45" | bc -l) )); then
        echo "评级: ⭐⭐ 一般"
    else
        echo "评级: ⭐ 不推荐"
    fi
}

# 使用示例
calculate_score 85 90 75 80
```

**综合评分权重**：
- 网络性能：35%（最重要）
- CPU性能：25%
- 磁盘性能：25%
- 内存性能：15%

---

## 🛠️ 高级测试工具

### 1. UnixBench（综合测试）

```bash
# 安装
apt install build-essential -y
wget https://github.com/kdlucas/byte-unixbench/archive/master.zip
unzip master.zip
cd byte-unixbench-master/UnixBench
make

# 运行测试
./Run

# 输出示例
==============================================================
#  System Benchmarks Index Score               1234.5
==============================================================
```

**评分参考**：
- >2000：顶级性能
- 1500-2000：优秀
- 1000-1500：良好
- 500-1000：一般
- <500：较差

### 2. Geekbench（跨平台CPU测试）

```bash
# 下载
wget https://cdn.geekbench.com/Geekbench-5.4.1-Linux.tar.gz
tar xzf Geekbench-5.4.1-Linux.tar.gz
cd Geekbench-5.4.1-Linux

# 单核测试
./Geekbench5 --single

# 多核测试
./Geekbench5

# 上传到Geekbench数据库
./Geekbench5 --upload
```

### 3. iperf3（网络带宽测试）

```bash
# 服务器端
apt install iperf3 -y
iperf3 -s -p 5201 -f M

# 客户端测试
iperf3 -c your-server-ip -p 5201 -t 60 -f M

# 多线程测试
iperf3 -c your-server-ip -P 4 -t 60

# 双向测试
iperf3 -c your-server-ip -d -t 60
```

### 4. FIO（磁盘IO测试）

```bash
# 安装
apt install fio -y

# 随机读取4K（模拟数据库）
fio --name=randread --filename=/tmp/fio_test --ioengine=libaio --rw=randread --bs=4k --size=1G --numjobs=4 --runtime=60 --group_reporting

# 随机写入4K
fio --name=randwrite --filename=/tmp/fio_test --ioengine=libaio --rw=randwrite --bs=4k --size=1G --numjobs=4 --runtime=60 --group_reporting

# 混合读写（70%读30%写）
fio --name=mix --filename=/tmp/fio_test --ioengine=libaio --rw=randrw --bs=4k --size=1G --rwmixread=70 --numjobs=4 --runtime=60 --group_reporting

# 清理测试文件
rm -f /tmp/fio_test
```

---

## 📈 测试报告生成

### 自动生成HTML报告

```bash
#!/bin/bash
# generate_report.sh

REPORT_FILE="/tmp/vps-bench-report.html"

cat > "$REPORT_FILE" << EOF
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>VPS性能测试报告 - $(date +%Y-%m-%d)</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 1200px; margin: 0 auto; padding: 20px; }
        .metric { margin: 20px 0; padding: 15px; border: 1px solid #ddd; border-radius: 8px; }
        .score { font-size: 2em; font-weight: bold; color: #2c3e50; }
        .star { color: #f39c12; }
        table { width: 100%; border-collapse: collapse; }
        th, td { padding: 10px; text-align: left; border-bottom: 1px solid #ddd; }
        th { background-color: #3498db; color: white; }
    </style>
</head>
<body>
    <h1>VPS性能测试报告</h1>
    <p>测试时间: $(date)</p>
    <p>主机名: $(hostname)</p>
    <p>系统: $(uname -s) $(uname -r)</p>
    
    <div class="metric">
        <h2>综合评分</h2>
        <div class="score">85/100 <span class="star">⭐⭐⭐⭐</span></div>
    </div>
    
    <div class="metric">
        <h2>CPU性能</h2>
        <p>型号: $(cat /proc/cpuinfo | grep "model name" | head -1 | cut -d: -f2)</p>
        <p>核心数: $(nproc)</p>
        <p>Geekbench 5: 单核1500 / 多核8000</p>
    </div>
    
    <div class="metric">
        <h2>内存性能</h2>
        <p>总内存: $(free -h | awk '/^Mem:/ {print $2}')</p>
        <p>读取速度: 8.5 GB/s</p>
        <p>写入速度: 4.2 GB/s</p>
    </div>
    
    <div class="metric">
        <h2>磁盘性能</h2>
        <p>类型: NVMe SSD</p>
        <p>顺序读取: 3500 MB/s</p>
        <p>顺序写入: 2500 MB/s</p>
    </div>
    
    <div class="metric">
        <h2>网络性能</h2>
        <p>下载速度: 500 Mbps</p>
        <p>上传速度: 200 Mbps</p>
        <p>延迟: 45ms</p>
    </div>
</body>
</html>
EOF

echo "报告已生成: $REPORT_FILE"
```

---

## 📊 测试数据可视化

### 图表生成脚本

```bash
#!/bin/bash
# generate_chart.sh - 生成性能对比图表

# 安装gnuplot（如果需要）
# apt install gnuplot -y

cat > performance.gp << 'EOF'
set terminal png size 800,600
set output 'performance_chart.png'
set title 'VPS Performance Comparison'
set ylabel 'Score (0-100)'
set style data histogram
set style fill solid
set grid y

# 数据
set xtics ('CPU' 0, 'Memory' 1, 'Disk' 2, 'Network' 3, 'Overall' 4)
plot 'data.txt' using 2:xtic(1) title 'Your VPS' lc rgb '#3498db', \
     '' using 3 title 'Average' lc rgb '#95a5a6'
EOF

# 生成示例数据
echo "CPU 85 70" > data.txt
echo "Memory 90 75" >> data.txt
echo "Disk 75 65" >> data.txt
echo "Network 80 60" >> data.txt
echo "Overall 82 67" >> data.txt

gnuplot performance.gp

echo "图表已生成: performance_chart.png"
```

---

## 🔄 测试自动化

### 定时测试脚本

```bash
#!/bin/bash
# auto_bench.sh - 定时性能测试

LOG_DIR="/var/log/vps-bench"
mkdir -p "$LOG_DIR"

# 创建日志文件
LOG_FILE="$LOG_DIR/bench_$(date +%Y%m%d_%H%M%S).log"

exec > >(tee -a "$LOG_FILE")
exec 2>&1

echo "=========================================="
echo "VPS性能测试 - $(date)"
echo "=========================================="

# 运行测试
echo "开始CPU测试..."
./bench.sh --cpu

echo "开始内存测试..."
./bench.sh --memory

echo "开始磁盘测试..."
./bench.sh --disk

echo "开始网络测试..."
./bench.sh --network

echo "=========================================="
echo "测试完成 - $(date)"
echo "=========================================="

# 发送邮件通知（如果配置）
if [ -n "$EMAIL" ]; then
    mail -s "VPS性能测试报告 - $(hostname)" "$EMAIL" < "$LOG_FILE"
fi
```

### Crontab配置

```bash
# 每天凌晨3点运行测试
0 3 * * * /path/to/auto_bench.sh

# 每周一生成周报
0 4 * * 1 /path/to/weekly_report.sh

# 每月生成月报
0 5 1 * * /path/to/monthly_report.sh
```

---

## 📋 VPS选购建议

### 根据用途选择配置

| 用途 | CPU | 内存 | 磁盘 | 带宽 | 推荐评分 |
|------|-----|------|------|------|----------|
| 个人博客 | 1核 | 1GB | 20GB SSD | 100Mbps | ⭐⭐⭐ |
| 小型Web应用 | 2核 | 2GB | 40GB SSD | 200Mbps | ⭐⭐⭐⭐ |
| 中型Web应用 | 4核 | 8GB | 100GB NVMe | 500Mbps | ⭐⭐⭐⭐ |
| 数据库服务器 | 8核 | 32GB | 500GB NVMe | 1Gbps | ⭐⭐⭐⭐⭐ |
| 游戏服务器 | 4核 | 8GB | 100GB SSD | 1Gbps | ⭐⭐⭐⭐ |
| 开发测试 | 2核 | 4GB | 50GB SSD | 200Mbps | ⭐⭐⭐⭐ |

### 推荐VPS提供商

- [VPSVIP](https://vpsvip.net) - 专业VPS推荐平台

---

## ❓ 常见问题

### Q: 测试结果不准确？

**A**: 测试时注意：
1. 确保没有其他程序占用资源
2. 多次测试取平均值
3. 不同时间段的测试结果可能不同
4. 网络测试建议选择多个节点

### Q: 磁盘测试失败？

**A**: 可能原因：
1. 磁盘空间不足，需要至少1GB可用空间
2. 权限问题，确保以root运行
3. 某些虚拟化环境不支持直接IO测试

### Q: 网络测试无法连接？

**A**: 检查步骤：
1. 确认网络连接正常：`ping 8.8.8.8`
2. 检查防火墙设置：`iptables -L`
3. 尝试更换测试节点：`./speedtest-cli --list`
4. 检查是否有流量限制

### Q: 如何对比不同VPS？

**A**: 
1. 使用相同的测试方法
2. 在相同时间段测试
3. 多次测试取平均值
4. 关注关键指标（CPU单核、磁盘IO、网络带宽）

### Q: 测试对服务器有影响吗？

**A**: 
- CPU/内存测试：短期高负载，测试完即恢复
- 磁盘测试：会产生大量读写，建议在空闲时测试
- 网络测试：会消耗流量配额

---

## 📚 学习资源

### 官方文档
- [Geekbench](https://www.geekbench.com/)
- [FIO](https://fio.readthedocs.io/)
- [iperf3](https://iperf.fr/)

### 推荐工具
- [VPSVIP](https://vpsvip.net) - 优质VPS推荐
- [nav.clashvip.net](https://nav.clashvip.net) - 网络工具导航

### 社区论坛
- [LowEndTalk](https://lowendtalk.com/)
- [Reddit r/VPS](https://reddit.com/r/VPS)

---

## 🔄 更新日志

### v2.0 (2026-08-09)
- ✅ 新增完整性能测试文档
- ✅ 增加CPU/内存/磁盘/网络测试详解
- ✅ 新增综合评分算法
- ✅ 增加高级测试工具教程
- ✅ 新增测试报告生成脚本
- ✅ 新增数据可视化方法
- ✅ 新增自动化测试方案
- ✅ 增加VPS选购建议
- ✅ 保留所有原有推广链接

### v1.0 (2026-03-27)
- 初始版本发布

---

## ⚠️ 免责声明

使用本工具箱即表示您同意：
- 仅在您拥有或授权的服务器上使用
- 对操作后果自行负责
- 遵守当地法律法规

---

## 📞 技术支持

- **VPS推荐**: [VPSVIP](https://vpsvip.net)
- **问题反馈**: [GitHub Issues](https://github.com/clashforwindows-net/vps-bench-20260327/issues)

---

**最后更新**: 2026-08-09  
**版本**: v2.0  
**Stars**: 0  
**Forks**: 0
