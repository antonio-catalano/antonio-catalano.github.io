# [Home](https://antonio-catalano.github.io/#) | [About](https://antonio-catalano.github.io/about.html) | [Archive](https://antonio-catalano.github.io/Archive.html) | [Contact](https://antonio-catalano.github.io/form-contact.html)

### In this article I'm going to summarize some of the most utilized functions and operations in _NumPy_ and _Pandas_.

#### These is an incomplete list (maybe I will add something else in the future), but in my experience these are the most important scripts to have in mind when you make data analysis with Python.
#### The aim is to do an efficient cheat sheet of NumPy and Pandas, assuming the knowledge of the basics that I left out.

*In this article __arr__ is a numpy array, __df__ is a DataFrame, __s__ is a Series.*

# _NumPy_

- Sort in place `arr.sort()`<br>
- Sort out of place `np.sort(arr)`
<br>
- Find the sorted unique values `np.unique(arr)`
<br>
- If _arr1_ is a (3 x 2) matrix and _arr2_ is a (2 x 4) matrix
`arr1 @ arr2` is the (3 x 4) matrix product.
<br>
- Inverse of a matrix
`np.linalg.inv(arr)`
<br>
- Determinant of a matrix
`np.linalg.det(arr)`
<br>
- Draw a random (4 x 3) matrix from a standard normal distribution
`np.random.randn(4,3)`
<br>
- Draw 100 random numbers chosen from the list [1,5,10] with different probabilities
`np.random.choice([1,5,10], p = [0.2, 0.3, 0.5], size = 100)`
<br>

- Fix the generator seed of random numbers `np.random.seed(2981)`
<br>
- Find the position of the maximum value `np.argmax(arr)` or `arr.argmax()`
<br>
- You have a 2 x 2 numpy _array_ and you want all values in a single array
`np.ravel(array)` or `array.ravel()`
Both operate out of place.
<br>
- Draw random sample from a multivariate normal distribution
`np.random.multivariate_normal(`__mean__`, `__cov__`)`
where __mean__ is an array of length _N_ and __cov__ is the covariance matrix (_N_ x _N_)
<br>
- Create a new array from an old one where the negative values are replaced with __0__
and the positive values with __10__
`new_array = np.where(old_array < 0, 0, 10)`
<br>
- Convert a numpy array of _str_ to an array of _float_.
`arr.astype(float)`
It operates out of place.
<br>
- 70th percentile of a numpy array.
`np.percentile(arr, 70)`
<br>
- Vertical stack: _arr1_ is a (3 x 2) numpy array and arr2 is a (2 x 2).
`np.r_[arr1,arr2]` is a (5 x 2) numpy array .
<br>
- Horizontal stack: _arr1_ is a (3 x 2) numpy array and arr2 is a (3 x 6).
`np.c_[arr1,arr2]` is a (3 x 8) numpy array.
<br>
- Reshape out of place an array of 12 elements in a (4 x 3) matrix
`arr.reshape((4,3))`
<br>
- Find the maximum value for each index among 4 arrays of the same shape.
`np.maximum(arr1, arr2, arr3, arr4)`

# _Pandas_

- Display only 2 floating points for each Series or DataFrame
`pd.options.display.float_format = '{:.2f}'.format`
<br>
- Set a column as index
`df.set_index('name_col', inplace = True)`
Go back to the original df.
`df.reset_index(inplace = True)`
<br>
- Delete a column of a DataFrame
`del df['col_name']`
<br>
- Delete _row2_ and _row5_ of a DataFrame
`df.drop(['row2', 'row5'], inplace = True)`
<br>
- Subset of df where both df['col1'] and df['col2'] verify the condition
`df[(df['col1'] < 0) & (df.col2 > 10)]`
Note that you __can't__ replace `&` with `and` and brackets are necessary.
<br>
- You have a Series but you want to give a name to the column and highlight it
`s.rename('Column_Name').to_frame()`
<br>
- Count the number of positive and not null numbers in each column of a DataFrame
`(df > 0).sum(axis = 0)`
<br>
- Find the index label where column _A_ has the minimum value
`df['A'].idxmin()`
<br>
- Find the position where column _A_ has the minimum value
`df['A'].values.argmin()`
<br>
- Plot a one-dimensional random walk in one line
`pd.Series(np.random.choice([-1,1], size = 10_000).cumsum()).plot()`
<br>
- _R_ is a numpy array containing _n_ returns: calculate the final capital (cumulative return)
`(R + 1).prod()`
<br>
- Create a new DataFrame from an old one where we retain the positive values but we replace
the negative values with __-1__
`df_new = df_old.where(df_old > 0, -1)`
<br>
- Instantiate a time series DataFrame of 252 daily gaussian data ending at 1st January 2020
`df = pd.DataFrame({'Normal Data' : np.random.randn(252)}, index = pd.date_range(end = '1/1/2020', periods = 252))`
<br>
- Sort in decreasing order a DataFrame by a given columns
`df.sort_values(by = 'col_name', ascending = False)`
<br>
- Sort in place a DataFrame by index
`df.sort_index(inplace = True)`
<br>
- Drop rows with at least 1 null value
`df.dropna(axis = 0)`
<br>
- Drop rows where only the _C_ column has a null value
`df.dropna(subset = ['C'])`
<br>
- Replace _null_ values with the median value of the corresponding column.
`df.fillna(df.median(), axis = 0)`
<br>
- Rename a column
`df.rename(columns = {'old_name' : 'new_name'}, inplace = True)`
<br>
- Concatenate two series in order to have a DataFrame with two columns.
`pd.concat([arr1, arr2], axis = 1)`
<br>
- Each element of a series is an element of a given list: true or false?.
`s.isin(given_list)`
<br>
- Number of null values for each column
`df.isnull().sum(axis = 0)`
<br>
- Create dummy variables for a categorical column and then discard one (in order to avoid the perfect collinearity)
`pd.get_dummies(df['categorical_col']).iloc[:,:-1]`
<br>
- Returns
`df['Adj Close'].pct_change()`
<br>
- Log returns
`np.log(df['Adj Close'] / df['Adj Close'].shift(1))`
<br>
- 100 days Rolling correlation
`df['AAPL_rets'].rolling(100).corr(df['SPY_rets'])`
<br>
- We have a daily price dataframe: find the first, high, low and last price of each month
`df['Adj Close'].resample('M').ohlc()`
<br>
- Convert an index of _str_ in _DatetimeIndex_
` df.index = pd.to_datetime(df.index)`
<br>
- Divide a series in quartiles and give them a label
`pd.qcut(s, 4, labels = 'Q1 Q2 Q3 Q4'.split())`
<br>
- Bootstrap a DataFrame with 20 extractions
`df.sample(20, replace = True)`
<br>
- Turn each data of the dataframe in 4 floating points
`df.applymap(lambda x: '%.4f' % x)`
<br>
- Group by column _C_ and apply a function _f(df, k, q)_ to each column.
`df.groupby('C').apply(f, k, q)`
<br>
- Group by column _C_ and compute the _sum_ for column _A_ and the _mean_ and _std_ for column _B_
`df.groupby('C').agg({'A' : 'sum', 'B' : ['mean', 'std']})`
<br>
- Create a pivot table where both index and columns are created with a _groupby_ operation <br>of the columns of the original DataFrame, then we apply the _mean function_ to the _'Returns'_ column.
`df.pivot_table(values = ['Returns'], index = ['Utilities','Financial','Energy'], columns = ['Day of the week'], aggfunc = 'mean')`
<br>
