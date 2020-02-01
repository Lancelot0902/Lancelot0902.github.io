---
title: Gauss Jordan（2）
date: 2020-2-1 19:30:00 +0800
categories: [Math, Matrix]
tags: [Math, Matrix]
---

Gauss-Jordan按列选取主元消元法的代码实现

```
void createAM(vector<vector<double>>& a)
{
	// 构建增广矩阵
	for (int i = 0; i != a.size(); ++i)
	{
		for (int j = 0; j != a.size(); ++j)
		{
			if (i == j)
				a[i].push_back(1);
			else
				a[i].push_back(0);
		}
	}
}

int search_pivot(vector<vector<double>>& a, int pos)
{
	// 按列选取主元
	double max = 0;
	int maxPos = pos;
	for (int i = pos; i != a.size(); ++i)
	{
		if (abs(a[i][pos]) > max)
		{
			max = abs(a[i][pos]);
			maxPos = i;
		}
	}
	return maxPos;
}

void swapRow(vector<vector<double>>& a, int row1, int row2)
{
	// 交换矩阵的两行
	for (int i = 0; i != a[0].size(); ++i)
	{
		int temp = a[row1][i];
		a[row1][i] = a[row2][i];
		a[row2][i] = temp;
	}
}

void scaleRow(vector<vector<double>>& a, int row, double coef)
{
	// 矩阵的第row行乘以非零系数coef
	for (int i = 0; i != a[0].size(); ++i)
	{
		a[row][i] *= coef;
	}
}

void addTo(vector<vector<double>>& a, int row1, int row2, double coef)
{
	// 将row1的coef倍加到row2上去
	for (int i = 0; i != a[0].size(); ++i)
	{
		a[row2][i] = a[row2][i] + a[row1][i] * coef;
	}
}

void invert(vector<vector<double>>& a)
{
	// 构建增广矩阵
	createAM(a);
	
	for (int i = 0; i != a.size(); ++i)
	{
		// 寻找主元
		int pivot = search_pivot(a, i);

		if (pivot != i)
			swapRow(a, i, pivot); // 将主元移动到主对角线上

		// 主元所在行除以主元
		double coef = 1.0 / a[i][pivot];
		scaleRow(a, i, coef);

		// 将主元所在列的其它元素变为0
		for (int j = 0; j != a.size(); ++j)
		{
			if (j == pivot)
				continue;
			double coef = -a[j][i];
			addTo(a, i, j, coef);
		}
	}
}
```

时间复杂度：2*N<sup>3

如果要考虑优化的话从两个方面入手

* 空间上，要构建增广矩阵，将n阶矩阵扩大成了n * 2n，可以考虑如何用n阶矩阵存储n * 2n的信息

* 时间上，考虑如何减少计算量（还没头绪）
