#### 题目：
You are on a flight and wanna watch two movies during this flight. 
You are given int[] movie_duration which includes all the movie durations. 
You are also given the duration of the flight which is d in minutes. 
Now, you need to pick two movies and the total duration of the two movies is less than or equal to (d - 30min). 
Find the pair of movies with the longest total duration. If multiple found, return the pair with the longest movie.

e.g. 
Input
movie_duration: [90, 85, 75, 60, 120, 150, 125]
d: 250

Output from aonecode.com
[90, 125]
90min + 125min = 215 is the maximum number within 220 (250min - 30min)

### 解答：
```c++
pair<int, int> flight_movie(vector<int> movies, int d)
{
	if (movies.empty() || d <= 30)
		return pair<int, int>(0, 0);

	pair<int, int> result(-1, -1);

	set<int> movie_set;
	movie_set.insert(movies.begin(), movies.end());

	for (auto element : movies) {
		auto ite = movie_set.upper_bound(d - 30 - element);
		if (ite == movie_set.begin())
			continue;

		ite--;

		if (*ite == element) {
			if (ite == movie_set.begin())
				continue;

			ite--;
		}

		if ((*ite + element) > (result.first + result.second)) {
			result.first = max(*ite, element);
			result.second = min(*ite, element);
		}
		else if ((*ite + element) == (result.first + result.second)) {
			if (result.first < max(*ite, element)) {
				result.first = max(*ite, element);
				result.second = min(*ite, element);
			}
		}
	}

	if ((result.first + result.second) < 0)
		return pair<int, int>(0, 0);

	return result;
}
```
