# pandasgeo

[English](README.md) | [简体中文](README.zh-CN.md)

`pandasgeo` 是一个用于地理空间数据处理和分析的Python包，它基于 `geopandas` 、`pandas`、`shapely`、`pyproj`等构建，提供了一系列简化常见GIS操作的工具函数。

## 功能

*   **距离计算**: 高效计算df之间的最近距离。
*   **空间分析**: 创建缓冲区(输入米)、泰森多边形、Delaunay三角网等。
*   **格式转换**: 方便地在 `GeoDataFrame` 与 `Shapefile`, `KML` 等格式之间转换。
*   **坐标聚合**: 提供将坐标点聚合到网格的工具。
*   **几何操作**: 包括合并多边形、计算质心、添加扇区等。

## 安装

1、你可以通过pip从PyPI安装 `pandasgeo`:

```bash
pip install pandasgeo
```

2、或者，直接从GitHub仓库安装最新版本：

```bash
pip install git+https://github.com/Non-existent987/pandasgeo.git
```
3、下载项目后从本地文件导入方便修改
```bash
import sys
import pandas as pd
# 找到你下载的pandasgeo文件路径
sys.path.insert(0, r'C:\Users\Administrator\Desktop\pandasgeo')
# 现在可以导入了
import pandasgeo as pdg
```


## 快速开始

以下是一个如何使用 `pandasgeo` 的简单示例：

### 1、把距离表1每个点最近的点（表2中）找出来并添加id、经纬度和距离。
```python
import pandas as pd
import pandasgeo as pdg

# 创建两个示例DataFrame
df1 = pd.DataFrame({
    'id': [1, 2, 3],
    'lon1': [116.404, 116.405, 116.406],
    'lat1': [39.915, 39.916, 39.917]
})

df2 = pd.DataFrame({
    'id': ['A', 'B', 'C', 'D'],
    'lon2': [116.403, 116.407, 116.404, 116.408],
    'lat2': [39.914, 39.918, 39.916, 39.919]
})

# 计算最近的1个点
result = pdg.min_distance_twotable(df1, df2,lon1='lon1', lat1='lat1', lon2='lon2', lat2='lat2', df2_id='id', n=1)
# 计算最近的2个点
result2 = pdg.min_distance_twotable(df1, df2,lon1='lon1', lat1='lat1', lon2='lon2', lat2='lat2', df2_id='id', n=2)
print("\n结果示例（距离单位：米）:")
print(result)
print(result2)
```
结果展示：
<table>
<tr>
<td style="vertical-align: top; padding-right: 50px;">

**df1表格：**

| id | lon1  | lat1 |
|----|-------|------|
| A  | 114.0 | 30.0 |
| B  | 114.1 | 30.1 |

</td>
<td style="vertical-align: top;">

**df2表格：**

| id | lon2   | lat2  |
|----|--------|-------|
| p1 | 114.01 | 30.01 |
| p2 | 114.05 | 30.05 |
| p3 | 114.12 | 30.12 |

</td>
</tr>
</table>


**最近的1个点：**

| id | lon1  | lat1 | nearest1_id | nearest1_lon2 | nearest1_lat2 | nearest1_distance |
|----|-------|------|-------------|---------------|---------------|-------------------|
| A  | 114.0 | 30.0 | p1          | 114.01        | 30.01         | 1470.515926       |
| B  | 114.1 | 30.1 | p3          | 114.12        | 30.12         | 2939.507557       |

**最近的2个点：**

| id | lon1  | lat1 | nearest1_id | nearest1_lon2 | nearest1_lat2 | nearest1_distance | nearest2_id | nearest2_lon2 | nearest2_lat2 | nearest2_distance | mean_distance |
|----|-------|------|-------------|---------------|---------------|-------------------|-------------|---------------|---------------|-------------------|---------------|
| A  | 114.0 | 30.0 | p1          | 114.01        | 30.01         | 1470.515926       | p2          | 114.05        | 30.05         | 7351.852775       | 4411.184351   |
| B  | 114.1 | 30.1 | p3          | 114.12        | 30.12         | 2939.507557       | p2          | 114.05        | 30.05         | 7350.037700       | 5144.772629   |

### 2、在表中找到每个点的最近的点（自身表），并添加id、经纬度和距离。
```python
import pandas as pd
import pandasgeo as pdg

# 创建两个示例DataFrame
df2 = pd.DataFrame({
    'id': ['A', 'B', 'C', 'D'],
    'lon2': [116.403, 116.407, 116.404, 116.408],
    'lat2': [39.914, 39.918, 39.916, 39.919]
})

# 计算最近的1个点
result = pdg.min_distance_onetable(df2,'lon2','lat2',idname='id',n=1)
# 计算最近的2个点
result2 = pdg.min_distance_onetable(df2,'lon2','lat2',idname='id',n=2)
print("\n结果示例（距离单位：米）:")
print(result)
print(result2)
```
结果展示：  
**df2表格：**

| id | lon2   | lat2  |
|----|--------|-------|
| p1 | 114.01 | 30.01 |
| p2 | 114.05 | 30.05 |
| p3 | 114.12 | 30.12 |

**最近1个点**

| | id | lon2 | lat2 | nearest1_id | nearest1_lon2 | nearest1_lat2 | nearest1_distance |
|---|-------|--------|-------|-------------|---------------|---------------|-------------------|
| 0 | p1 | 114.01 | 30.01 | p2 | 114.05 | 30.05 | 5881.336911 |
| 1 | p2 | 114.05 | 30.05 | p1 | 114.01 | 30.01 | 5881.336911 |
| 2 | p3 | 114.12 | 30.12 | p2 | 114.05 | 30.05 | 10289.545038 |

**最近2个点**

| | id | lon2 | lat2 | nearest1_id | nearest1_lon2 | nearest1_lat2 | nearest1_distance | nearest2_id | nearest2_lon2 | nearest2_lat2 | nearest2_distance | mean_distance |
|---|-------|--------|-------|-------------|---------------|---------------|-------------------|-------------|---------------|---------------|-------------------|---------------|
| 0 | p1 | 114.01 | 30.01 | p2 | 114.05 | 30.05 | 5881.336911 | p3 | 114.12 | 30.12 | 16170.880987 | 11026.108949 |
| 1 | p2 | 114.05 | 30.05 | p1 | 114.01 | 30.01 | 5881.336911 | p3 | 114.12 | 30.12 | 10289.545038 | 8085.440974 |
| 2 | p3 | 114.12 | 30.12 | p2 | 114.05 | 30.05 | 10289.545038 | p1 | 114.01 | 30.01 | 16170.880987 | 13230.213012 |


## 贡献

欢迎各种形式的贡献，包括功能请求、错误报告和代码贡献。

## 许可证

本项目使用MIT许可证。
