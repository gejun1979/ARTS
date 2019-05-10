#### 题目：K Nearest Post Offices
Find the k post offices located closest to you, given your location and a list of locations of all post offices available.
Locations are given in 2D coordinates in [X, Y], where X and Y are integers.
Euclidean distance is applied to find the distance between you and a post office.
Assume your location is [m, n] and the location of a post office is [p, q], the Euclidean distance between the office and you is SquareRoot((m - p) * (m - p) + (n - q) * (n - q)).
K is a positive integer much smaller than the given number of post offices. from aonecode.com

e.g.
Input
you: [0, 0]
post_offices: [[-16, 5], [-1, 2], [4, 3], [10, -2], [0, 3], [-5, -9]]
k = 3

Output from aonecode.com
[[-1, 2], [0, 3], [4, 3]] 

### 解答：
```c++
double calculate_distance(pair<int, int> location, pair<int, int> office)
{
	return sqrt((location.first - office.first)*(location.first - office.first)
		+ (location.second - office.second)*(location.second - office.second));
}

vector<pair<int, int>> get_latest_office(int k, pair<int, int> location, vector<pair<int, int>> offices)
{
	vector<pair<int, int>> result;
	if (k <= 0 || offices.empty())
		return result;

	map<double, pair<int, int>> offices_map;
	for (auto element: offices) {
		offices_map[calculate_distance(location, element)] = element;
	}

	auto ite = offices_map.begin();
	for (int i = 0; i<k; ++i) {
		result.push_back((ite++)->second);
	}

	return result;
}
```
