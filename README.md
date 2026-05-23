# 实验3：Jupyter Notebook 基础实践

## 实验目的
- 进一步熟悉 Python 语法  
- 掌握 Jupyter Notebook 开发的基本流程  
- 熟悉 Pandas、Matplotlib 等常用数据分析库的用法  

## 实验环境
- 操作系统：macOS (M2 芯片)  
- 环境管理：Anaconda (通过 Homebrew 安装)  
- 主要工具：Jupyter Notebook, Python 3  

## 一、Notebook 基本概念
### 快捷键与模式
- **编辑模式**（绿色边框）：`Enter` 进入，可编辑代码或文本。  
- **命令模式**（蓝色边框）：`Esc` 进入，支持快捷键：  
  - `A` / `B`：上方/下方插入单元格  
  - `D`（两次）：删除单元格  
  - `M` / `Y`：切换为 Markdown / 代码单元格  
  - `Shift + Enter`：运行当前单元格并跳转到下一个  

### Kernel 概念
Kernel 是 Notebook 的“计算引擎”，负责执行代码并维护变量状态。可通过 `Kernel -> Restart` 重置环境。

![Notebook 界面](images/5d5beb9b0cefeb8adab1b75b6266f13e.png)

## 二、Python 语法练习：选择排序算法
实现 `selection_sort` 函数，对列表进行升序排序。

```python
numbers = [64, 25, 12, 22, 11]

def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

def test():
    sorted_numbers = selection_sort(numbers.copy())
    print("排序前:", numbers)
    print("排序后:", sorted_numbers)

test()
```
运行结果：
```
排序前: [64, 25, 12, 22, 11]
排序后: [11, 12, 22, 25, 64]
```

![选择排序代码与结果](images/32f10edeee6c5929a346dabfca480e39.png)

## 三、数据分析：财富500强数据集（Fortune 500）

### 3.1 数据加载与初步观察
```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline

df = pd.read_csv('fortune500.csv')
df.head()
df.tail()
```
数据共有 25500 条记录，列名为原始名称（Year, Rank, Company, Revenue, Profit）。为方便处理，重命名列：

```python
df.columns = ['year', 'rank', 'company', 'revenue', 'profit']
```
![数据加载](images/a2a77e1720cc55b22981b24b8c7b5bc3.png)
![数据预览 head](images/a66f20ad5ac922a72a1933cbee5a7779.png)
![数据预览 tail](images/bea7d839bfd4201f39f16b2340a30468.png)
![列重命名](images/b658f12653ada3dbb2430dbffccb7cd6.png)
![数据条数](images/f103c4293f8356b19b4b061e933a14eb.png)

### 3.2 数据清洗：处理“利润”列中的异常值
利润列中存在 `'N.A.'` 等非数值字符，需要将其删除。

```python
non_numeric_profits = df.profit.str.contains('[^0-9.-]')
df.loc[non_numeric_profits].head()          # 查看异常行示例
len(df.profit[non_numeric_profits])         # 异常行数：369

df = df.loc[~non_numeric_profits]           # 保留正常数据
df.profit = df.profit.apply(pd.to_numeric)  # 转为数值类型
len(df)                                     # 清洗后剩余 25131 行
df.dtypes                                   # profit 类型变为 float64
```

![异常值展示](images/3c39e02e4bbf2c8460cd196a9fa4f4d3.png)
![异常值统计](images/a40e5ae25996dc6012a5062d12b2dc78.png)
![清洗后数据类型](images/4e0d6482a79e954c90160f484e92c256.png)

### 3.3 数据分组与聚合
按年份分组，计算每年的平均利润和平均收入。

```python
group_by_year = df.loc[:, ['year', 'revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index          # 年份
y_profit = avgs.profit
y_revenue = avgs.revenue
```
![分组聚合](images/acf213e6f74f00ec1548f92e73ab13f5.png)

## 四、数据可视化：利润与收入双线图
使用 Matplotlib 在同一张图上绘制平均利润和平均收入的变化曲线。

```python
plt.figure(figsize=(12, 5))
plt.plot(x, y_profit, label='Average Profit')
plt.plot(x, y_revenue, label='Average Revenue')
plt.title('Mean Fortune 500 Company Profits and Revenues (1955–2005)')
plt.xlabel('Year')
plt.ylabel('Amount (in millions USD)')
plt.legend()
plt.grid(True)
plt.show()
```

![双线图](images/1e9838a1998a02fb947c2a2d162463b9.png)

## 五、完整实验记录截图
Jupyter Notebook 中包含了全部的代码单元格、运行输出及 Markdown 说明。

![Notebook 全貌](images/de3d52cf802bae32ad2fc547c19b6b11.png)

## 六、GitHub 上传
将以下文件上传至 GitHub 仓库：
- `实验3_Jupyter_Notebook实践.ipynb`  
- `fortune500.csv`（数据集）  
- `images/` 文件夹（所有截图）

仓库链接：[https://github.com/unnn-hokeii/jupyter-notebook-experiment3](https://github.com/unnn-hokeii/jupyter-notebook-experiment3)

![GitHub 仓库页面](images/89a357e87f86ff1511f6d154900d840d.png)

## 实验总结
本次实验完成了：
1. Jupyter Notebook 的安装与基础操作（快捷键、Cell 模式、Kernel）。  
2. Python 选择排序算法的编写与测试。  
3. Fortune 500 数据集的加载、清洗（删除利润列异常值）、分组聚合。  
4. 使用 Matplotlib 绘制利润和收入随时间变化的双线图。  
5. 将完整实验记录及截图上传至 GitHub，形成可复现的实验报告。

通过本次实践，熟悉了数据科学的基本工作流，为后续更复杂的数据分析项目打下了基础。
