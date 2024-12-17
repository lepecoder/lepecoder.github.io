---
title: openGL实现图形学扫描线种子填充算法
date: 2018-06-11T19:49:00
tags: ['图形学']
categories: 图形学
---


### 先上效果图



![](http://p1f1jwe7c.bkt.clouddn.com/18-6-11/28919649.jpg)


白色的起始种子点



### 代码



```c++


#include <GL/glut.h>

#include <cmath>

#include <set>

#include <vector>

#include <unistd.h> //sleep函数

#include <iostream>

#include <algorithm> //find函数，查找vector中元素

#include <stack>





using namespace std;



//规格化为0.05的倍数

inline GLdouble normal(GLdouble x)

{

	return (round(x * 20) / 20);

}



typedef struct Point {

	GLdouble x, y;



	Point(GLdouble a = 0, GLdouble b = 0)

	{

		x = a, y = b;

	}



	// set会对插入的元素自动排序，需要重载运算符.定义排序规则

	//

	//

	// 重载运算符的要求

	// 1. 若A<B为真,则B<A为假

	// 2. 若A<B,B<C --> A<C

	// 3. A<A永远为假

	// set中判断元素是否相等

	// if(!(A<B||B<A)) --> A=B

	bool operator<(const Point &a) const

	{

		return ((x - a.x) < -0.01 || ((x - a.x) < 0.01 && (y - a.y) < -0.01));

	}



	bool operator==(const Point &a) const

	{

		return (abs(x - a.x) < 0.01 && abs(y - a.y) < 0.01);

	}

} point;



void drawGrid();



void initGraphBorder();



void drawGraphices();



void myDisplay();



void DDA(point A, point B);



void initGraphBorder();





point first_seed;

unsigned long m;

static int n = 40;

static GLfloat pointSize = 12.5;

set<point> graphBorder; // 图形边界数组<set>可以快速查找

//set虽然可以快速查找，但却会打乱顺序，不方便实现填充动画

vector<point> graphFill;   // 要填充的数组

vector<point> graphVertex;  //存储图形定点数组，按顺序

stack<point> seed;  //存储种子



GLdouble p = float(2.0 / n); // 每个格子的大小



//画网格坐标

void drawGrid()

{

	glColor3d(1, 1, 1); //网格用白色表示

	glLineWidth(1);

	glBegin(GL_LINES);



	for (int i = 0; i <= n; i++) {

		//画竖线

		glVertex2d(-1 + 1.0 / n + i * 2.0 / n, -1);

		glVertex2d(-1 + 1.0 / n + i * 2.0 / n, 1);

		//画横线

		glVertex2d(-1, -1 + 1.0 / n + i * 2.0 / n);

		glVertex2d(1, -1 + 1.0 / n + i * 2.0 / n);

	}



	glEnd();

}





//DDA算法

void DDA(point A, point B)

{

	cout << "连接(" << A.x << ',' << A.y << ") 到 (" << B.x << ',' << B.y << ")\n";



	if (A.x > B.x) {

		swap(A, B);

	}



	A.x = normal(A.x);

	A.y = normal(A.y);

	B.x = normal(B.x);

	B.y = normal(B.y);

	double delta_x = B.x - A.x;

	double delta_y = B.y - A.y;

	double k = delta_y / delta_x;

	double x = A.x, y = A.y;



	if (k > -1 && k < 1) {

		//x是最大位移

		//cout << k << endl;

		while (true) {

			if ((x - B.x) > 0.01)break;



			graphBorder.insert(point(normal(x), normal(y)));

			x += p;

			y += (p * k);

		}

	}

	else if (k >= 1) {

		//Y是最大位移

		while (true) {

			if ((y - B.y) > 0.01) break;



			graphBorder.insert(point(normal(x), normal(y)));

			y += p;

			x += (p / k);

		}

	}

	else {

		while (true) {

			if ((B.y - y) > 0.01) break;



			graphBorder.insert(point(normal(x), normal(y)));

			y -= p;

			x -= (p / k);

		}

	}

}





//初始化图形边界数组

void initGraphBorder()

{

	//    graphVertex存储图形的顶点

	for (auto it = graphVertex.begin(); it != graphVertex.end(); it++) {

		if (it == graphVertex.end() - 1) {

			//最后一个点连接第一个点

			DDA(*it, *graphVertex.begin());

		}

		else {

			//连接it1和it2xr

			DDA(*it, *(it + 1));

		}

	}

}



/*********************************

 * 1. 初始化：堆栈置空,将种子seed(x,y)入栈

 * 2. 出栈: 若栈空则结束，否则将栈顶元素出栈，以y作为当前扫描线

 * 3. 填充并确定新种子点所在区域：从当前种子点出发，沿y扫描线向左右方向填充

 *    直到遇到边界像素。标记当前区段的左右端点坐标为Xl, Xr.

 * 4. 确定新种子点：检查[Xl-1,Xr]和[Xl+1,Xr]区域，若存在非边界，未填充的

 *    像素，则把每一区间的最右像素作为种子点压栈。返回第2步

**********************************/



// 沿扫描线的区域填充

// 从种子堆栈<stack>seed中取出种子点

// 把要填充的点加入<vector>graphFill

void floodFillSet()

{

	while (!seed.empty()) {

		point s = seed.top();   //当前种子点

		seed.pop();

		cout << "当前种子点  " << s.x << "  " << s.y << endl;

		//填充种子点及左右连续区域

		point t = s, xl, xr;



		//填充左侧连续区域,并找到区域最左边界

		while (true) {

			//到达边界退出

			if (t.x < -1)break;

			graphFill.push_back(t);

			t.x -= p;

			//查找是否已填充

			auto it1 = find(graphFill.begin(), graphFill.end(), t);

			//查找是否是边界

			auto it2 = graphBorder.find(t);



			if (it1 != graphFill.end() || it2 != graphBorder.end()) { //已填充或已达边界

				xl = t;

				xl.x += p;

				break;

			}

		}



		t = s;



		//填充右侧区域

		while (true) {

			if (t.x > 1)break;



			graphFill.push_back(t);

			t.x += p;

			//查找是否已填充

			auto it1 = find(graphFill.begin(), graphFill.end(), t);

			//查找是否是边界

			auto it2 = graphBorder.find(t);



			if (it1 != graphFill.end() || it2 != graphBorder.end()) { //已填充或已达边界

				xr = t;

				xr.x -= p;

				break;

			}

		}



		//在下一行找种子点

		for (int d : { 1, -1 }) {//先上再下

			bool status = false; //标记一段空白区域的开始

			t = xl;

			t.y += (p * d);   //移到下一行或上一行



			while ((t.x - xr.x) < 0.07) {

				auto it1 = find(graphFill.begin(), graphFill.end(), t);

				auto it2 = graphBorder.find(t);



				//找空白区域开始点

				//如果当前点是空白区域,且空白区域未开始 则s=true

				if (!status && it1 == graphFill.end() && it2 == graphBorder.end()) {

					status = true;

				}



				//区域开始且遇到边界或填充区域

				if (status && (it1 != graphFill.end() || it2 != graphBorder.end())) { //到达边界或已填充颜色，且左侧有空白区域

					status = false;

					//添加左侧一点为种子点

					seed.push(point(t.x - p, t.y));

				}



				// 到达最右区间xr，即便不是边界,只要s=true也应标注最右种子点

				if (status && abs(t.x - xr.x) < 0.01) {   //注意浮点数比较大小

					status = false;

					seed.push(t);

				}



				t.x += p;

			}

		}

	}

}





//画待填充的图形边界

void drawGraphices()

{

	glColor3f(0.5, 0.5, 0.2);

	glBegin(GL_POINTS);



	//C++11的新特性，可以遍历vector map list or {1,2,3}

	for (auto it : graphBorder) {

		glVertex2d(it.x, it.y);

	}



	glEnd();

}



//画填充像素

void drawFloodFill(unsigned long m)

{

	glColor3f(0.4, 0.8, 0.1);

	glBegin(GL_POINTS);



	for (unsigned long i = 0; i < m && i < graphFill.size(); i++) {

		glVertex2d(graphFill[i].x, graphFill[i].y);

		//myDisplay();

	}



	glEnd();

}



//绘图

void myDisplay()

{

	glClearColor(0.1f, 0.3f, 0.33f, 0.0f);

	glClear(GL_COLOR_BUFFER_BIT);

	//画图形边界

	drawGraphices();

	//画m个填充像素

	drawFloodFill(m);

	glColor3d(1, 0, 0);

	glPointSize(pointSize);

	//加个白点,起始种子点

	glColor3d(1, 1, 1);

	glBegin(GL_POINTS);

	glVertex2d(first_seed.x, first_seed.y);

	glEnd();

	//画网格

	drawGrid();

	glFlush();

}





int main(int argc, char *argv[])

{

	//存入要填充的图形顶点 8

	//(-0.7,0.6) (0.7,0.6) (0.7,0) (0.3,0) (0.3,-0.5) (-0.3,-0.5)

	// (-0.3,0) (-0.7,0)

	cout << "请输入顶点坐标,(-1,1) 输入>1 结束 \n";

	double x, y;



	while (cin >> x) {

		if (x > 1)

			break;

		else

			cin >> y;



		graphVertex.emplace_back(point(x, y));

	}



	cout << "输入初始种子点\n";

	cin >> x >> y;

	first_seed.x = x;

	first_seed.y = y;

	glutInit(&argc, argv);

	glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);

	glutInitWindowSize(500, 500);

	glutInitWindowPosition(100, 100);

	//初始化边界集

	initGraphBorder();

	//扫描线种子填充

	seed.push(first_seed);

	floodFillSet();

	glutCreateWindow("扫描线种子填充");

	glutDisplayFunc(&myDisplay);



	for (m = 0; m < graphFill.size(); m++) {

		usleep(10000);

		myDisplay();

	}



	glutMainLoop();

	return 0;

}



```



### 输入样例



```

-0.7 0.7

-0.35 0.7

-0.35 0

0.35 0

0.35 0.7

0.7 0.7

0.7 -0.7

-0.7 -0.7

2 

0 -0.2





```
    