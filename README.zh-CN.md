```markdown
# pandasgeo

[![en](https://img.shields.io/badge/lang-en-red.svg)](README.md)
[![zh-CN](https://img.shields.io/badge/lang-zh--CN-green.svg)](README.zh-CN.md)
```
`pandasgeo` 是一个用于地理空间数据处理和分析的Python包，它基于 `geopandas` 和 `pandas` 构建，提供了一系列简化常见GIS操作的工具函数。

## 功能

*   **距离计算**: 高效计算点对之间的最近距离。
*   **空间分析**: 创建缓冲区、泰森多边形、Delaunay三角网等。
*   **格式转换**: 方便地在 `GeoDataFrame` 与 `Shapefile`, `KML` 等格式之间转换。
*   **坐标聚合**: 提供将坐标点聚合到网格的工具。
*   **几何操作**: 包括合并多边形、计算质心、添加扇区等。

## 安装

你可以通过pip从PyPI安装 `pandasgeo` (一旦发布):

```bash
pip install pandasgeo
```

或者，直接从GitHub仓库安装最新版本：

```bash
pip install git+https://github.com/yourusername/pandasgeo.git
```

## 快速开始

以下是一个如何使用 `pandasgeo` 计算两个点集之间最近距离的简单示例：

```python
import pandas as pd
import pandasgeo as pdg

# 创建两个示例DataFrame
data1 = {'id': ['A', 'B'], 'lon1': [114.0, 114.1], 'lat1': [30.0, 30.1]}
df1 = pd.DataFrame(data1)

data2 = {'id': ['p1', 'p2', 'p3'], 'lon2': [114.01, 114.05, 114.12], 'lat2': [30.01, 30.05, 30.12]}
df2 = pd.DataFrame(data2)

# 计算df1中每个点到df2中最近的那个点
result = pdg.min_distance_twotable(df1, df2, n=1)

print(result)
```

## 贡献

欢迎各种形式的贡献，包括功能请求、错误报告和代码贡献。

## 许可证

本项目使用MIT许可证。
