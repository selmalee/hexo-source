---
title: 最优路径算法之小游戏实战
date: 2016-11-13 16:27:55
tags: [dijkstra算法, JavaScript]
categories:
 - 实践
---

## 前言
最近要做一个关于若干点的最短连线的小游戏，才发现自己关于算法方面的知识有多不扎实＝＝ 吃一堑，长一智。借此文简单总结一下。若有大神无意看见此文，请轻拍。
<!-- more -->
## 需求
随机给出若干点，求这些点的最短连线。可以延伸出一个小游戏，给出若干点，让用户在一个面板上绘线将点连起，判断是否最短路径，给出提示。
实质是有权无向完全图中，没有特定源点的最短路径算法。

## Dijkstra算法和Floyd算法
搜索资料，Dijkstra算法和Floyd算法是比较接近需求的算法。

### stra算法
Dijkstra算法是典型的单源最短路径算法，用于计算一个节点到其他所有节点的最短路径。主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。
算法思路：
1. 初始时，S只包含源点，即S＝{v}，v的距离为0。U包含除v外的其他顶点，即U={其余顶点}，若v与U中顶点k有边，则<k,v>正常有权值，若k不是v的出边邻接点，则<k,v>权值为∞。
2. 从U中选取一个距离v最小的顶点k，把k，加入S中。
3. 以k为新考虑的中间点，修改U中各顶点的距离；若从源点v到顶点u的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，修改后的距离值的顶点k的距离加上边上的权。
4. 重复步骤2和3直到所有顶点都包含在S中。

![Dijkstra算法](/static/2016/11/shortest-1.gif)

### Floyd算法
Floyd-Warshall算法（Floyd-Warshall algorithm）是解决任意两点间的最短路径的一种算法，可以正确处理有向图或负权的最短路径问题。
算法思路：
1. 从任意一条单边路径开始。所有两点之间的距离是边的权，如果两点之间没有边相连，则权为无穷大。
2. 对于每一对顶点u和v，看看是否存在一个顶点k使得从u到k再到v比己知的路径更短。如果是更新它。

关于Dijkstra算法和Floyd算法详细见[最短路径—Dijkstra算法和Floyd算法](http://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html)

## 具体实现
借鉴Dijkstra算法和Floyd算法思路，结合需求，开始实现。
### 实现思路
1. 随机生成若干个点，每个点有x坐标、y坐标、是否被访问过visited三个属性。
2. 用勾股定理算出任意两点的直线距离，生成二维矩阵
3. 将源点记为uNext，从所有连线选择一条与uNext的最短连线，将线上的此点(重新记为uNext)放入U中，将最短连线放入到一个数组中。
4. 重复步骤3，直到所有点都被放进U，计算数组中的路径和，返回最短路径。
5. 遍历点，求出各点作为源点时的最短路径，比较，得出最终的最短路径。

### 主要代码
声明类Graph及其属性和方法。实现随机生成点。做一些限制，使各点不会过于聚集。
``` js
function Graph(v,w) {
  this.vertices = v;
  this.points = []; // V 总的点集
  this.u = []; // U 点集
  this.width = w;
  this.pythagorean = pythagorean; // 勾股定理
  this.getPoints = getPoints;
  this.matrix = [];
  this.getMatrix = getMatrix;
  this.path = [];
  this.pathSum = 0;
  this.getPath = getPath;
  this.getTempPath = getTempPath;
}
function Point(x, y) {
  this.x = x;
  this.y = y;
  this.visited = false;
}
function getPoints() {
  this.points = [];
  for (var i = 0; i < this.vertices; i++) {
    this.points[i] = new Point(Math.ceil(15+Math.random()*(this.width-30)),Math.ceil(15+Math.random()*(this.width-30)));
  }
  for (var j = 0; j < this.vertices; j++) {
    for(var k = j+1; k < this.vertices; k++) {
      if(Math.abs(this.points[j].x - this.points[k].x) < 30 && Math.abs(this.points[j].y - this.points[k].y) < 30) {
          this.getPoints();
      }
    }
  }
}
```
用勾股定理算出任意两点的直线距离，生成二维矩阵。
``` js
// a^2 + b^2 = c^2
function pythagorean(a, b) {
  return Math.pow(this.points[a].x-this.points[b].x,2) + Math.pow(this.points[a].y-this.points[b].y,2);
}
function getMatrix() {
  var min = 999999;
  var pointI, pointJ;
  for (var i = 0; i < this.vertices; i++) {
    this.matrix[i] = [];
    for (var j = 0; j < this.vertices; j++) {
      if (i === j) {
        this.matrix[i][j] = 0;
      } else {
        this.matrix[i][j] = this.pythagorean(i,j);
      }
    }
  }
  return this.matrix;
}
```
将源点记为uNext，从所有连线选择一条与uNext的最短连线，将线上的此点(重新记为uNext)放入U中，将最短连线放入到一个数组中。重复此步，直到所有点都被放进U，计算数组中的路径和，返回最短路径。
``` js
function getTempPath(uNext) {
  this.path = [];
  this.u = [];
  var pathTempSum = 0;
  for (var k = 0; k < this.vertices; k++) {
    this.points[k].visited = false;
  }
  this.u.push(uNext);
  this.points[uNext].visited = true;

  while (this.u.length !== this.vertices) {
    var min = 999999;
    var tempNext;
    for (var i = 0; i < this.vertices; i++){
      if(this.points[i].visited === false) {
        if(this.matrix[uNext][i] < min) {
          min = this.matrix[uNext][i];
          tempNext = i;
        }
      }
    }
    uNext = tempNext;
    this.u.push(uNext);
    this.path.push(min);
    this.points[uNext].visited = true;
  }
  for (var j = 0; j < this.path.length; j++) {
    pathTempSum += this.path[j];
  }
  return pathTempSum;
}
```
遍历点，求出各点作为源点时的最短路径，比较，得出最终的最短路径。
``` js
function getPath() {
  var min = 9999999;
  var temp = 0;
  var shortest;
  for(var i = 0; i < this.vertices; i++) {
    temp = this.getTempPath(i);
    if(temp < min) {
      min = temp;
      shortest = i;
    }
  }
  this.pathSum = min;
  this.getTempPath(shortest);
}
```
## 结合canvas做出小游戏
主要思路：
1. new一个Graph对象。利用canvas画出随机生成的点。
2. 监听touchstart、touchmove、touchend事件。实现随着触摸绘线。touchmove事件中判断是否经过图中的点并用数组记录。touchend事件触发后经过的点样式发生变化，点与点间用直线连起来。
3. 点击开始检测，计算经过的路径，并比较经过的路径和最短的路径，分别给出提示。

demo地址：http://119.29.142.213/shortest
![demo](/static/2016/11/shortest-2.png)

## 6 不足之处
 - UI方面待实现：经过点时有呼吸灯效果；画布边缘不能绘线等；错误后显示重新开始按钮
 - 声明了较多数组和用了较多循环。我再想想= =b

## 7 参考
- [最短路径—Dijkstra算法和Floyd算法](http://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html)
- [Floyd最短路算法及javascript实现](http://www.108js.com/article/article5/50041.html?id=898)
