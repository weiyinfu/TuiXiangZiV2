# 推箱子的来历
推箱子游戏起源于日本，名为sokoban。推箱子是日本人今林宏行于1981年发明并且编写程序实现的，1982年由Thinking Rabbit公司在日本发行。日文原名《仓库番》，英语音译为 Sokoban，中文目前最通用的叫法是推箱子。  
 
# 游戏规则
用键盘上的上、下、左、右键移动小人，把箱子全部推到指定的位置即可过关。箱子只能推，不能拉，并且小人一次只能推动一个箱子。

# 目录结构说明
* TuiXiangZi4j:Java版的推箱子
* front:网页版推箱子
* sokoban：python版推箱子
* spider:数据收集和清洗

# 推箱子的地图
本程序存储上使用易读易理解的XSB格式，内部使用int表示物体，用一个二维int数组表示推箱子地图。数组中的数字在0到6之间，每个数字代表的含义如下：  
* 0 空白 space
* 1 插槽 slot
* 2 人物 man
* 3 人在插槽处 manSlot
* 4 箱子 box
* 5 箱子在插槽处 boxSlot
* 6 墙 wall  

这种地图表示方式在编码上有一些巧妙之处：因为人和箱子可以位于空白和插槽所在的位置，那么直接通过加减法就能够表示物体的移动。例如，人物在插槽处时，此处编码为“人+插槽=3”，箱子在插槽处时，此处编码为“箱子+插槽=5”。  

## XSB格式
XSB是推箱子地图的专用文件格式，它是一种文本文件，它的优势在于文本可视化比较好，以字符画的形式表示地图。

| 字符  |   含义| 助记 |
| :---:  | :---: | :---: |
|@   | 人 (man) | 在群里at某个人 | 
|+   | 人在目标点 (man on goal) |  |
|$   | 箱子 (box)  | 箱子里面装的是dollar |
|*   | 箱子在目标点 (box on goal) | 正中靶心 |
|#   | 墙 (wall) | 坚不可摧的篱笆 |
|.   | 目标点 (goal) | 虚位以待 |
|-   | XSB格式空格代表“地板” | 空白 |

其中地板的表示比较特殊，因为连续多个空格在网页或即时通讯软件中偶尔显示有问题，也用“-”或“\_”代替空格。(floor, represented by ' ' or '-' or '_')   
PSB格式最后也可以加一些注释，如题目、作者。例如：
```plain
----#####----------
----#---#----------
----#$--#----------
--###--$##---------
--#--$-$-#---------
###-#-##-#---######
#---#-##-#####--..#
#-$--$----------..#
#####-###-#@##--..#
----#-----#########
----#######--------
Title: Classic level 1
Author: Thinking Rabbit
```

## 地图的几种模式
地图的模式主要在于墙的变化：
* 凡是不可到达的地方都是墙，墙外没有空白、插槽等干扰项
* 凡是可到达的地方的四联通分量且不可到达的地方都是墙，这样墙会尽量少
* 凡是可到达的地方的八联通分量都是墙
* 在边界处没有墙，小人不能越界，程序中需要添加越界检查

# 图片
为了游戏效果，推箱子所需要的图片除了包括上述推箱子地图中的图片外，还可以包括以下内容。  
给小人添加动作：上下左右，表示上次移动的方向。 
* up
* right
* down
* left

# 答案格式
推箱子的答案是一个上下左右字符串，解析的时候包括以下步骤：
* 把汉字“上下左右”替换为“udlr”
* 全部转小写
* 去掉非udlr字符 

例如：  
```plain
lldddrRRRRRRRRRRRR
lllllllluuulLulDDDuulldddrRRRRRR
RRRRRllllllluuulluuurDDuullDDDDDuul
```

# AI
推箱子是一个NP难问题，设计AI来推箱子是一个很艰难的问题。  
围棋和推箱子复杂度对比：http://sokoban.ws/blog/?p=2330   
推箱子是PSPACE复杂度的问题：http://sokoban.ws/blog/?p=2254

# 网站
* 国外推箱子网站：
  * [game-sokoban](http://www.game-sokoban.com/)
  * [推箱子在线](https://www.sokobanonline.com/)
* 国内推箱子网站
  * [sokoban.cn](http://sokoban.cn/)：此网站会定期举办的推箱子比赛   
  * [sokoban.ws](http://sokoban.ws/)：推箱子比赛提交答案的网站 
  * [sokoban.org](http://sokoban.org/)：此网站与sokoban.cn是同一个人维护的。  
* 博客
  * 杨超，一个推箱子爱好者，sokoban.cn和sokoban.org就是他创建并维护的，他是一名计算机学者。
      * http://sokoban.ws/blog/  
      * http://sokoban.ws/blog/?page_id=869  
      * http://sokoban.cn/blog/?page_id=189
　* 邹永忠：http://blog.sina.com.cn/zo603
* 学术论文
  * 基于RNN实现推箱子出题与解题：https://thesai.org/Publications/ViewPaper?Volume=8&Issue=3&Code=ijacsa&SerialNo=64
* 程序
  * [HTML推箱子](http://sokoban.cn/sokoplayer/SokoPlayer_HTML5.php)  
  
# 技巧
* 逆推模式：就是拉箱子游戏，拉箱子游戏比推箱子游戏简单。拉箱子是推箱子的逆运算。  
http://sokoban.cn/tutorial/reverse/reverse_mode.php

# TODO
* 快速选关
* 记录最少步数，记录最佳答案
* 输入文本，关卡可视化，可以手动配置映射
* 控制台版推箱子
* 地图制作工具
* 如何进一步消重
* 如何判断一个题目是否有解
* 如何生成难度较大的推箱子关卡
* 问题 最佳解法（最少步数） 完成次数 难度指数
* 闯关 演练（用户输入地图） 出题 
* 原则：题库中的题目一定是正则化的题目，不在乎形状
* 正则化的时候需要考虑答案一起正则化
* TODO:添加地图编辑器
* 重新检查html和java，看能否正常运行