geneExonCov
===========

# 1.软件介绍

本软件名为geneExonCov，目的是获取样本在基因和外显子的质控情况。

# 2.安装说明

* 下载：  git clone git@10.0.1.7:Software/geneExonCov.git


* 无需安装，直接调用代码源文件即可， 依赖`samtools` 软件 和 python `intervaltree`第三方模块


# 3.使用说明
###3.1.参数介绍

^ 参数  ^ 介绍说明                                                        ^
| -i  | 输入的gene.list文件，区间要求bed格式， 如果有表头，表头需以`#`开头  |
| -b  | 输入的bam文件                                                    |
| -c  | 输入的芯片区间文件， 区间要求bed格式， 如果有表头，表头需以`#`开头      |
| -o  | 输出目录，不存在会自动创建                                            |
| -t  | 使用的cpu核数，默认为1个cpu                                           |



### 3.2.输入文件

* gene.list文件
^#Gene^NM^Exon^Chr^Start^Stop^
|ERBB1|NM\_001005862.1|EX1|17|37844392|37844531|
|ERBB2|NM\_001005862.1|EX2|17|37844948|37845053|

* 样本比对BAM文件

* 芯片区间文件
^ #Chr   ^ Start ^End ^
|1|1212|1234| 
|2|5678|6789| 


### 3.3.输出文件

* 基因外显子覆盖度文件，tsv格式，geneExonCover.tsv
^#Gene^Exon^CaptExon^meanDep^maxDep^minDep^10Xcover^20Xcover^50Xcover^100Xcover^200Xcover^500Xcover^1000Xcover^
|BRCA1|EX1	|100%|	5748|	7863	|2727|	100.00%	|100.00%|	100.00%	|100.00%|	100.00%	|100.00%|	100.00%	|
|BRCA1|EX2	|100%|	6456|	5455	|5420|	100.00%	|100.00%|	100.00%	|100.00%|	100.00%	|100.00%|	100.00%	|

* bed芯片区域覆盖度文件，tsv格式，bedCov.tsv
^ #Chr  ^ Start  ^ Stop  ^  meanDep  ^ 1.0meanCover  ^ 0.5meanCover  ^  0.2meanCover ^
| 1     | 123    | 234   | 1000      | 40.00%        | 80.00%        | 100.00%       |
| 2     | 213    | 123   | 600       | 20.00%        | 50.00%        | 80.00%        |


### 3.4. 应用实例
<code>
python geneExonCov.py -t 10 -b sample.bam -i gene.list -c chip.bed -o outdir
</code>

### 3.5.资源消耗
内存消耗可控

支持多进程，推荐10进程

### 3.6. 结果说明
* geneExonCover.tsv
^ 结果列        ^ 结果说明                              ^
|Gene|基因名，一个基因只能对应到一个转录本上。|
|Exon|外显子区域|
|CaptExon|探针在该基因外显子中的捕获区域占该基因外显子整个EXON的百分比|
|meanDep|该基因外显子的捕获区域内平均深度|
|maxDep|该基因外显子的捕获区域内最高深度|
|minDep|该基因外显子的捕获区域内最低深度|
|10Xcover|该基因外显子的捕获区域内10X深度以上的覆盖度|
|20Xcover|该基因外显子的捕获区域内20X深度以上的覆盖度|
|50Xcover|该基因外显子的捕获区域内50X深度以上的覆盖度|
|100Xcover|该基因外显子的捕获区域内100X深度以上的覆盖度|
|200Xcover|该基因外显子的捕获区域内200X深度以上的覆盖度|
|500Xcover|该基因外显子的捕获区域内500X深度以上的覆盖度|
|1000Xcover|该基因外显子的捕获区域内1000X深度以上的覆盖度|

* bedCov.tsv
^ 结果列        ^ 结果说明                              ^
|Chr|染色体位置|
|Start|染色体坐标起始位置|
|Stop|染色体坐标终止位置|
|MeanDep|该区域平均深度|
|1.0meanCover|该区域在芯片平均深度1倍以上的区域覆盖情况|
|0.5meanCover|该区域在芯片平均深度0.5倍以上的区域覆盖情况|
|0.2meanCover|该区域在芯片平均深度0.2倍以上的区域覆盖情况|

# 4 性能测试

因为每个多进程的实际cpu消耗不高，所以总cpu消耗大约在1个左右，多进程主要用于读写

### 4.1 不同数据量下的时间消耗

按照 3.5 中的推荐cpu=10 下分析

^数据量^时间(Min)^
|1G|2.44|
|3G|3.95|
|5G|5.28|
|8G|6.52|
|10G|5.26|

* 运行时间可能受当时IO影响

### 4.2 不同数据量下的内存消耗  

按照 3.5 中的推荐cpu=10 下分析

^数据量^内存(MB)^
|1G|162.69|
|3G|214.27|
|5G|226.69|
|8G|218.02|
|10G|230.27|

### 4.3 不同cpu下的时间消耗

在5G样本下进行分析

^cpu数^时间(Min)^
|1|69.30|
|3|24.37|
|5|15.00|
|10|7.80|
|20|3.08|


### 4.4 异常处理

无其他输出，运行结束后会打印程序运行时间到标准输出


# 5 精度测试

暂无

# 6 文献和来源
暂无
