# Path Planning used A*
## 算法流程


## 运行结果
>这里是以Diagonal heuristic作为例子演示,可以发现能够到达终点,并且探索的节点数目较少.


##  对比不同的启发式函数对A*效率的影响
### 不同类型的启发式函数
>首先在这里可以分析Euclidean以及Diagonal Heuristic是满足admissible的,而因为飞机是可以斜向上飞行的,所以Manhattan是不满足admissible的,因此使用Manhattan可以保证较快的搜索到可行路径,但是不能满足optimality;使用Euclidean以及Diagonal Heuristic可以保证实现optimality,当然作为trade-off,搜索速度会下降一点.

>加入Tie Breaker的作用主要是打破f相等的平衡,这里采取$h=h*(1+p)$,这样子h大一点的点代表可能距离目标点比较远,因此这样子调整后可以优先扩展距离目标点近的节点,可以加快扩展速度,同时因为只是稍微加大了一点,不会不满足admissible的条件,还可以实现更快的扩展.

>实现方法则为在getHeu函数以及AstarGraphSearch函数中添加额外的参数flag,用于确定使用哪个启发式函数来计算当前点到终点的距离.其中Tie Breaker是在Euclidean的基础上改进,因此应和Euclidean进行对比,最终对比结果如下:
* Case1

|  |Euclidean|Manhattan|Tie_Breaker|Diagonal|
|:-:|:-:|:-:|:-:|:-:|
|时间 ms|1.065659|0.781013|0.891030|0.693911|
|距离 m|0.895391|0.918823|0.895391|0.895391|
|visited nodes|144|21|139|51|

* Case2

|  |Euclidean|Manhattan|Tie_Breaker|Diagonal|
|:-:|:-:|:-:|:-:|:-:|
|时间 ms|3.413633|1.632898|2.943285|1.815696|
|距离 m|1.360653|1.384084|1.3606353|1.3606353|
|visited nodes|266|27|252|48|

**注:Tie Breaker是在Euclidean的基础上进行修改,要与Euclidean进行对比**

可以发现Manhattan因为不是admissible的,牺牲了一定的最优性换取了速度;而Diagonal作为最tight的heuristic,实现最优性以及较快的速度;Tie Breaker这里可以实现一定速度的提升,相信如果选择更加合适的Tie Breaker可以换取更好的性能.

## A*与JPS比较
这里按照A*的算法逻辑简单实现了JPS,对比效果如下:

* Case1

|  |A*|JPS|
|:-:|:-:|:-:|
|时间 ms|1.916744|0.707338|
|距离 m|1.174544|1.147257|
|visited nodes|99|24|

* Case2

|  |A*|JPS|
|:-:|:-:|:-:|
|时间 ms|2.994652|1.572211|
|距离 m|1.276963|1.249676|
|visited nodes|102|36|

可以发现JPS无论在扩展速度还是最优性方面都是优于A*的,主要得益于这个地图的架构是栅格的以及障碍物比较多,适合使用跳点;当地图不是栅格地图时,JPS无法使用.

## 小结
虽然说实现了A*与JPS,但是JPS跳点扩展代码不是自己编写的,这部分才是真正的难度;另外地图导入/可视化等也都需要进一步学习!