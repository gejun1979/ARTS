### 题目：Treasure Island
You have a map that marks the location of a treasure island. Some of the map area has jagged rocks and dangerous reefs. Other areas are safe to sail in. 
There are other explorers trying to find the treasure. So you must figure out a shortest route to the treasure island.
Assume the map area is a two dimensional grid, represented by a matrix of characters.
You must start from the top-left corner of the map and can move one block up, down, left or right at a time.
The treasure island is marked as ‘X’ in a block of the matrix. ‘X’ will not be at the top-left corner.
Any block with dangerous rocks or reefs will be marked as ‘D’. You must not enter dangerous blocks. You cannot leave the map area.
Other areas ‘O’ are safe to sail in. The top-left corner is always safe.
Output the minimum number of steps to get to the treasure.
from aonecode.com

e.g.
Input
[
[‘O’, ‘O’, ‘O’, ‘O’],
[‘D’, ‘O’, ‘D’, ‘O’],
[‘O’, ‘O’, ‘O’, ‘O’],
[‘X’, ‘D’, ‘D’, ‘O’],
]

Output from aonecode.com
Route is (0, 0), (0, 1), (1, 1), (2, 1), (2, 0), (3, 0) The minimum route takes 5 steps. 

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

bool is_in_map(int x, int y, vector<vector<char>> & treasure_map)
{
	if (x < treasure_map.size() && y < treasure_map[0].size()) {
		if ( treasure_map[x][y] != 'D' &&  treasure_map[x][y] != 'S') {
			return true;
		}
	}

	return false;
}

int get_shortest_route(vector<vector<char>> treasure_map)
{
	if (treasure_map.empty())
		return 0;

	int count = 0;
	queue<pair<int, int>> cur_queue;
	queue<pair<int, int>> next_queue;

	treasure_map[0][0] = 'S';
	cur_queue.push(pair<int, int>(0,0));
	while (cur_queue.empty() == false) {
		count++;

		while (cur_queue.empty() == false) {
			auto point = cur_queue.front();
			cur_queue.pop();

			if (is_in_map(point.first + 1, point.second, treasure_map)) {
				if (treasure_map[point.first + 1][point.second] == 'X')
					return count;

				treasure_map[point.first + 1][point.second] = 'S';
				next_queue.push(pair<int, int>(point.first + 1, point.second));
			}

			if (is_in_map(point.first - 1, point.second, treasure_map)) {
				if (treasure_map[point.first - 1][point.second] == 'X')
					return count;

				treasure_map[point.first - 1][point.second] = 'S';
				next_queue.push(pair<int, int>(point.first - 1, point.second));
			}

			if (is_in_map(point.first, point.second + 1, treasure_map)) {
				if (treasure_map[point.first][point.second + 1] == 'X')
					return count;

				treasure_map[point.first][point.second + 1] = 'S';
				next_queue.push(pair<int, int>(point.first, point.second + 1));
			}

			if (is_in_map(point.first, point.second - 1, treasure_map)) {
				if (treasure_map[point.first][point.second - 1] == 'X')
					return count;

				treasure_map[point.first][point.second - 1] = 'S';
				next_queue.push(pair<int, int>(point.first, point.second - 1));
			}
		}

		cur_queue = next_queue;
		next_queue = queue<pair<int, int>>();
	}

	return 0;
}

int main()
{
	vector<vector<char>> treasure_map =
	{
		{'O', 'O', 'O', 'O'},
		{'D', 'O', 'D', 'O'},
		{'O', 'O', 'O', 'D'},
		{'O', 'D', 'D', 'X'},
	};

	auto result = get_shortest_route(treasure_map);

	return 0;
}
```
