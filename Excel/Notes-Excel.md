[TOC]
# 一、5种数据分析模型
## SWOT模型
```
SWOT模型，即“态势分析”，根据内在因素（优势和劣势）和外在因素（机会和威胁）定位自身并制定相应的战略。
```
| OT\SW | Strengths | Weakness |
| :-: | :-: | :-: |
| **Opportunity** | SO战略<br>发挥优势，把握机遇 | WO战略<br>克服劣势，把握机遇 |
| **Threats** | ST战略<br>发挥优势，规避威胁 | WT战略<br>克服劣势，规避威胁 |
## PEST模型
```
PEST模型，根据宏观环境（政治、经济、社会、技术）定位自身并指定响应的战略。
1. 政治因素
    - 政府态度
    - 政策法规
2. 经济因素
    - 宏观经济
    - 微观经济
3. 社会因素
    - 文化（文化背景、消费观念）
    - 人口（人口结构、教育程度）
    - 经济（消费需求、收入水平）
4. 技术
    - 生产技术
    - 专利保护
```
## 5W2H模型
```
5W2H模型，即“七问分析”，通过目的导向的因素拆解（何事、何故、何人、何时、何地、怎么做、做到什么程度）定位自身并制定相应的战略。
```
## 4P营销模型
```
4P营销模型，根据营销组合理论（Product产品、Price价格、Place渠道、Promotion宣传）定位自身并指定响应的战略。
```
## 逻辑树模型
```
逻辑树模型，通过因素分解、层层深入解析自身并制定相应的战略。
```
# 二、5种数据分析思路
1. 对比分析
2. 分组分析
3. 杜邦分析（拆解因素、层层深入，研究结构关系）
4. 漏斗分析（理顺流程，环节转变分析）
5. 象限分析（横纵维度、全局分析，拓展：面积维度）
# 三、常用数据分析工具
## 方差分析
```
1.概念
方差分析，即“F检验”，通常用于两个或以上“均数”差别的显著性检验，可以分析一个或多个因素在不同水平下对总体的影响。
2.操作
【数据分析】->【方差分析：单因素方差分析】
注意：若勾选“标志”，则输入区域涵盖标题。
3.意义
在结果中，SUMMARY表的“方差”值越大，则反映“均数”之间的差别越大；如若需要，应该对均数的生成产生怀疑。
方差分析表的“P值”越大，表示组与组之间的差别越大（P值大于0.05，则每组之间差异不大，每组均数的生成可信）。
```
## 相关系数
```
1.概念
相关系统，通常用于反映多个因素之间两两影响的程度。
2.操作
【数据分析】->【相关系数】
注意：若勾选“标志”，则输入区域涵盖标题。
3.意义
在结果中，行列交叉的单元格中的数值（取值范围为-1~+1）表示这两个因素的关联程度。正数越大或负数越小，则表示关联程度越大。
注意：相关系数只能分析纯数字，因此如有必须需要将其他类型的因素值转换成数字类型。
注意：正、负相关的差异
```
## 协方差
```
1.概念
协方差，也通常用于反映多个因素之间两两影响的程度。与“相关系数”的区别在于结果值的取值范围不受-1~+1的限制。
2.操作
【数据分析】->【协方差】
注意：若勾选“标志”，则输入区域涵盖标题。
```
## 指数平滑
```
1.概念
指数平滑，通过“加权平均”的方式预测“未来数据”。
2.操作
步骤：假设α值的范围 》利用指数平滑工具确定最佳α值 》观察最佳α值下预测值的趋势线并确定是否继续指数平滑 》计算未来数据
注意：若勾选“标志”，则输入区域涵盖标题。
注意：α即阻尼系数。
    2.1 根据已有数据的数值规律假设α值：
        数值波动不大                 α ∈ (0.05, 0.20)
        数值有波动但整体趋势平稳       α ∈ (0.10, 0.40)
        数值波动很大                 α ∈ (0.50, 0.80)
        数值有波动但整体趋势上升或下降  α ∈ (0.60, 1.00)
    2.2 试算并确定最佳α值
        观察不同α值计算后，实际值与预测值趋势线的重合程度，选择重合度最高的α值。
    2.3 观察最佳α值下预测值趋势线的波动趋势
        若趋势线为“不规律”，则无需再次指数平滑；
        若趋势线为“直线”，则需要二次指数平滑；
        若趋势线为“二次曲线”，则需要三次指数平滑；
    2.4 计算未来数据
        # 一次平滑
        未来数据 = α*最近的实际值 + (1-α)*最近的一次预测值
        # 二次平滑
        未来数据 = α*一次未来数据 + (1-α)*最近的二次预测值
        # 三次平滑
        未来数据 = α*二次未来数据 + (1-α)*最近的三次预测值
3.意义
预测未来产量、未来销量、未来利润等等。
```
## 移动平均
```
1.概念
移动平均，通过指定区间内的平均值来展现发展趋势并预测“未来数据”。
2.操作
步骤：假设区间间隔 》利用移动平均工具确定最佳间隔 》计算未来数据
    2.1 假设区间间隔
        理论上，间隔 >= 2即可，通常选择2，3，4或者更多。
    2.2 试算并确定最佳间隔
        观察不同间隔下，实际值与预测值趋势线的重合程度，选择重合度最高的间隔。
    2.3 计算未来数据
        若最佳间隔为n，则：未来数据 = 最近n个移动平均值之和 / n
```
## 描述统计
```
1.概念
描述统计，一次性列出多项统计指标。
2.操作
【数据分析】->【描述统计】
注意：勾选“汇总统计”。
注意：若勾选“标志”，则输入区域涵盖标题。
```
## 排位与百分比排位
```
1.概念
排位与百分比排位，展示每项数据的排名和每项数据超越了百分之多少的总体数据。
2.操作
【数据分析】->【排位与百分比排位】
3.意义
在结果中，“点”表示每项数据在原数据中的位置，“排位”表示数据从大到小排序，“百分比”表示对应项的数据超越了多少的总体数据。
```
## 回归
```
1.概念
回归，对数据使用“最小二乘法”的直线拟合来执行线性回归分析，用于分析一个因素是如何受到一个或多个其他因素影响的。
2.操作
【数据分析】->【回归】
注意：“Y值输入区域”对应“一个因素”的数据区域，“X值输入区域”对应“一个或多个其他因素”的数据区域。
注意：注意：若勾选“标志”，则输入区域涵盖标题。
3.意义
结果的“回归统计”表中
    Multiple R值，表示“一个因素”与“一个或多个其他因素”之间的关联程度（范围为-1~+1，绝对值越接近1，则表示相关性越大）。
    R Square值，表示“一个因素”受“一个或多个其他因素”影响变化的剧烈程度（值越大，则受影响后变化的程度越大）。仅受一个其他因素影响时，应重点关注该值。
    Adjusted R Square值，表示“一个或多个其他因素”对“一个因素”变化的百分比程度。受到多个其他因素影响时，应重点关注该值。
    标准误差值，表示拟合程度的大小。该值越小，则拟合的越好。
    观测值，表示每个因素的样本个数。
结果的“方差分析”表中
    Significance F值，表示整个回归模型的有效度（该值小于0.05，则表示有效性高）。
结果的“回归参数”表中
    Coefficients值，表示回归系数。
    标准误差值，该值越小，则回归参数的精确度越高。
    P-value值，表示“一个因素”与“每个其他因素”之间的关联程度。
```
# 四、常用函数
```
IF函数
    FORMAT	=IF(test_expr, expr1[, expr2])
    INTRO	若test_expr为True,则当前单元格的值为expr1,否则为expr2；
    E.G.	=IF(C2>90, "优秀", IF(C2>60, "及格", IF(C2>=0, "不及格")))
    TIPS	expr1或expr2可以是直接值,也可以是能返回值的表达式；
            expr2可以省略,等价于将expr2设为"FALSE"；
```
```
SUMIF函数
    =SUMIF(test_range, test_expr, sum_range)
    当前单元格的值为"就test_range范围内满足test_expr条件"的单元格的求和结果；
    =SUMIF(C2:C9, ">80", B2:B9)
    test_range为test_expr提供判断范围,形如Xm:Xn；
    sum_range为求和操作提供最原始的范围(即尚未通过test_expr进行过滤)；
```
```
SUMIFS函数
    =SUMIFS(sum_range, test_range1, test_expr1, test_range2, test_expr2, ...)
    当前单元格的值为"就test_range1范围内满足test_expr1条件"且"就test_range2范围内满足test_expr2条件"且"......"的单元格的求和结果；
    =SUMIFS(B2:B9, C2:C9, ">80", D2:D9, "男")
```
```
TREND函数
    =TREND()
```
