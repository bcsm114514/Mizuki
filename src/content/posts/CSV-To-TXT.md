---
image: ''
title: 将多个CSV的第一行集合为TXT的Python小工具
published: 2025-08-28
updated: ''
description: ''
category: 数码
tags:
  - CSV-To-TXT
draft: false
lang: ''
---
# CSV-To-TXT

::github{repo="bcsm114514/CSV-To-TXT"}

一个用于 **批量合并 CSV 文件的第一列数据** 到 TXT 文件的小工具。  
只依赖 Python 标准库，简单轻量，开箱即用。

---

## ✨ 功能特性

- 扫描指定目录下所有 `.csv` 文件  
- 自动读取每个文件的 **第一列**  
- 将结果统一合并保存到一个 `.txt` 文件  
- 跳过空行，保证输出整洁  
- 处理进度实时输出  

---

## 📦 环境要求

- Python 3.x  
- 无需额外依赖库（只用标准库 `os`、`glob`、`csv`）  

---

## 🚀 使用方法

1. 克隆或下载本项目代码
2. 运行脚本：

```bash
python CSV-To-TXT.py
```

3.  按提示输入：

要处理的目录路径（存放多个 CSV 文件的文件夹）

输出 TXT 文件路径（生成的 TXT 文件保存位置）

👉 如果不输入输出文件路径，程序会默认生成 merged_first_columns.txt。

## 🖥️ 示例

假设目录 ./data/ 下有 3 个文件：

```plain
data1.csv
data2.csv
data3.csv
```

运行脚本后，会生成一个 TXT 文件：

```plain
merged_first_columns.txt
```

内容类似：

```plain
Alice
Bob
Charlie
David
Eve
...
```

## ✅ 应用场景

- 提取用户 ID、邮箱等第一列数据
- 批量整理数据采集结果
- 将 CSV 转换为 TXT 清单
