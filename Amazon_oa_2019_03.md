### 题目：Subtree: Maximum Average Node
Given a binary tree, find the subtree with maximum average. Return the root of the subtree.
Example Given a binary tree:
####       2
####     /   \ 
####  -2       14 
####  / \      / \ 
#### -1  1     5  -1
return the node 14.

### 解答：
```c++
#include "stdafx.h"
#include <stdlib.h>
#include <string.h>

#include <map>
#include <set>
#include <list>
#include <stack>
#include <queue>
#include <vector>
#include <utility>
#include <iostream>
#include <cmath>

using namespace std;

struct tree_node
{
	tree_node()
	{
		value = 0;
		left = NULL;
		right = NULL;
	}

	tree_node(int para, tree_node * lp, tree_node * rp)
	{
		value = para;
		left = lp;
		right = rp;
	}

	int value;
	tree_node * left;
	tree_node * right;
};

pair<tree_node*, double> max_res;

void helper(tree_node * root, int & sum, int & count)
{
	if (root == NULL)
		return;
	
	int l_sum = 0;
	int l_count = 0;
	helper(root->left, l_sum, l_count);

	int r_sum = 0;
	int r_count = 0;
	helper(root->right, r_sum, r_count);

	sum = root->value + l_sum + r_sum;
	count = 1 + l_count + r_count;
	double avg = (double)sum / count;
	if (avg > max_res.second) {
		max_res.first = root;
		max_res.second = avg;
	}
}

tree_node * get_largest_subtree(tree_node * root)
{
	if (root == NULL)
		return NULL;

	int sum = 0;
	int count = 0;
	helper(root, sum, count);

	return max_res.first;
}

int main()
{
	tree_node * n_3_1 = new tree_node(-1, NULL, NULL);
	tree_node * n_3_2 = new tree_node(1, NULL, NULL);
	tree_node * n_3_3 = new tree_node(5, NULL, NULL);
	tree_node * n_3_4 = new tree_node(-1, NULL, NULL);
	tree_node * n_2_1 = new tree_node(-2, n_3_1, n_3_2);
	tree_node * n_2_2 = new tree_node(14, n_3_3, n_3_4);
	tree_node * n_1_1 = new tree_node(2, n_2_1, n_2_2);

	auto result = get_largest_subtree(n_1_1);

	return 0;
}
```
