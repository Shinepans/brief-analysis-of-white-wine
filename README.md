# brief-analysis-of-white-wine

py ana

## 数据文件

> Raw: ./white-wine.txt    Csv: /.white-wine.csv

|fixed acidity|volatile acidity|citric acid|residual sugar|chlorides|free sulfur dioxide|total sulfur dioxide|density|pH|sulphates|alcohol|quality|
|----|----|----|----|----|----|----|----|----|----|----|----|
|7|0.27|0.36|20.7|0.045|45|170|1.001|3|0.45|8.8|6|
|8.1|0.28|0.4|6.9|0.05|30|97|0.9951|3.26|0.44|10.1|6|
|7.2|0.23|0.32|8.5|0.058|47|186|0.9956|3.19|0.4|9.9|6|
|7.2|0.23|0.32|8.5|0.058|47|186|0.9956|3.19|0.4|9.9|6|
|8.1|0.28|0.4|6.9|0.05|30|97|0.9951|3.26|0.44|10.1|6|
|6.2|0.32|0.16|7|0.045|30|136|0.9949|3.18|0.47|9.6|6|
|7|0.27|0.36|20.7|0.045|45|170|1.001|3|0.45|8.8|6|

## 数据含义

|变量名|含义|
|----|---|
|fixed acidity|固定酸度|
|volatile acidity|挥发性酸度|
|citric acid|柠檬酸|
|residual sugar|剩余糖|
|chlorides|氯化物|
|free sulfur dioxide|游离二氧化碳|
|total sulfur dioxide|总二氧化硫|
|density|密度|
|pH|pH值|
|sulphates|酸碱盐|
|alcohol|酒精|
|quality|品质|

## 读取

```
import csv
path = "white_wine.csv"
f = open(path, "r")
content = list(csv.reader(f))
f.close()
print content[:5]
```

```
[['fixed acidity', 'volatile acidity', 'citric acid', 'residual sugar', 'chlorides', 'free sulfur dioxide', 'total sulfur dioxide', 'density', 'pH', 'sulphates', 'alcohol', 'quality'], ['7', '0.27', '0.36', '20.7', '0.045', '45', '170', '1.001', '3', '0.45', '8.8', '6'], ['8.1', '0.28', '0.4', '6.9', '0.05', '30', '97', '0.9951', '3.26', '0.44', '10.1', '6'], ['7.2', '0.23', '0.32', '8.5', '0.058', '47', '186', '0.9956', '3.19', '0.4', '9.9', '6'], ['7.2', '0.23', '0.32', '8.5', '0.058', '47', '186', '0.9956', '3.19', '0.4', '9.9', '6']]
```

## 离散变量 quality 划分等级

```
import csv
f = open("white_wine.csv", "r")
content = list(csv.reader(f))
f.close()
qualities = []
for sample in content[1:]:
    qualities.append(int(sample[-1]))
unity_quality = set(qualities)
print(unity_quality)
```

## 依据 quality 划分子集

```
import csv
f = open("white_wine.csv", "r")
content =list(csv.reader(f))
f.close()
quality_subsets = {}
for sample in content[1:]:
    quality = int(sample[-1])
    if quality not in quality_subsets.keys():
        quality_subsets[quality] = [sample]
    else:
        quality_subsets[quality].append(sample)
```

## 样本量统计

```
import csv
f = open("white_wine.csv", "r")
content =list(csv.reader(f))
f.close()
quality_subsets = {}
for sample in content[1:]:
    quality = int(sample[-1])
    if quality not in quality_subsets.keys():
        quality_subsets[quality] = [sample]
    else:
        quality_subsets[quality].append(sample)
subset_numbers = []
for key, value in quality_subsets.items():
    subset_numbers.append((key, len(value)))
print subset_numbers
```

```
[(3, 14), (4, 115), (5, 1020), (6, 1539), (7, 616), (8, 123), (9, 4)]
```

## fixed acidity 均值

```
import csv
f = open("white_wine.csv", "r")
content =list(csv.reader(f))
f.close()
quality_subsets = {}
for sample in content[1:]:
    quality = int(sample[-1])
    if quality not in quality_subsets.keys():
        quality_subsets[quality] = [sample]
    else:
        quality_subsets[quality].append(sample)
mean_acidities = []
for quality, samples in quality_subsets.items():
    sum_ = 0
    for sample in samples:
        sum_ += float(sample[0])
    mean_acidities.append((quality, sum_/len(samples)))
print mean_acidities
```

```
[(3, 7.535714285714286), (4, 7.052173913043476), (5, 6.907843137254891), (6, 6.812085769980511), (7, 6.755844155844158), (8, 6.708130081300811), (9, 7.5)]
```

## 缺失处理

填入均值

eg.  age部分数据缺确实

```
import numpy as np
age_without_missing = []
for i in range(len(age)):
    if age[i] != '':
        age[i] = float(age[i])
        age_without_missing.append(float(age[i]))
age_mean = round(np.mean(age_without_missing), 3)

for i in range(len(age)):
    if age[i] == '':
        age[i] = age_mean
age_filled = age
```
