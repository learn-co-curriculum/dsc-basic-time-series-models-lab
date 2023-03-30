# Basic Time Series Models - Lab

## Introduction

Now that you have some basic understanding of the white noise and random walk models, its time for you to implement them! 

## Objectives

In this lab you will: 

- Generate and analyze a white noise model 
- Generate and analyze a random walk model 
- Implement differencing in a random walk model 


## A White Noise model

To get a good sense of how a model works, it is always a good idea to generate a process. Let's consider the following example:
- Every day in August, September, and October of 2018, Nina takes the subway to work. Let's ignore weekends for now and assume that Nina works every day 
- We know that on average, it takes her 25 minutes, and the standard deviation is 4 minutes  
- Create and visualize a time series that reflects this information 

Let's import `pandas`, `numpy`, and `matplotlib.pyplot` using their standard alias. 


```python
# Do not change this seed
np.random.seed(12)
```


```python
# __SOLUTION__
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Do not change this seed
np.random.seed(12)
```

Create the dates. You can do this using the `date_range()` function Pandas. More info [here](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.date_range.html).


```python
# Create dates
dates = None
```


```python
# __SOLUTION__
dates = pd.date_range("2018-08-01", "2018-10-31", freq="B", inclusive="left")
len(dates)
```




    65



Generate the values for the white noise process representing Nina's commute in August and September.


```python
# Generate values for white noise
commute = None
```


```python
# __SOLUTION__
commute = np.random.normal(25, 4, len(dates))
```

Create a time series with the dates and the commute times.


```python
# Create a time series
commute_series = None
```


```python
# __SOLUTION__
commute_series = pd.Series(commute, index=dates)
```

Visualize the time series and set appropriate axis labels.


```python
# Visualize the time series
```


```python
# __SOLUTION__
ax = commute_series.plot(figsize=(14, 6))
ax.set_ylabel("Commute time (min)", fontsize=16)
ax.set_xlabel("Date", fontsize=16)
plt.show()
```


    
![png](index_files/index_18_0.png)
    


Print Nina's shortest and longest commute.


```python
# Shortest commute
```


```python
# Longest commute
```


```python
# __SOLUTION__
min(commute)
```




    12.41033391382408




```python
# __SOLUTION__
max(commute)
```




    36.487277579955666



Look at the distribution of commute times.


```python
# Distribution of commute times
```


```python
# __SOLUTION__
commute_series.hist(grid=False);
```


    
![png](index_files/index_26_0.png)
    


Compute the mean and standard deviation of `commute_series`. The fact that the mean and standard error are constant over time is crucial!


```python
# Mean of commute_series
```


```python
# Standard deviation of commute_series
```


```python
# __SOLUTION__
np.mean(commute_series)
```




    24.629331839058004




```python
# __SOLUTION__
np.std(commute_series)
```




    4.2480368492867395



Now, let's look at the mean and standard deviation for August and October.  


```python
# Mean and standard deviation for August and October
```


```python
# __SOLUTION__
aug_series = commute_series["08-2018"]
oct_series = commute_series["10-2018"]
print("August: ", np.mean(aug_series), np.std(aug_series), "\n")
print("October: ", np.mean(oct_series), np.std(oct_series))
```

    August:  25.35433780425335 4.206438796880027 
    
    October:  25.677914014895 4.2736723525385925


Because you've generated this data, you know that the mean and standard deviation will be the same over time. However, comparing mean and standard deviation over time is useful practice for real data examples to check if a process is white noise!

## A Random Walk model 

Recall that a random walk model has: 

- No specified mean or variance 
- A strong dependence over time 

Mathematically, this can be written as:

$$Y_t = Y_{t-1} + \epsilon_t$$

Because today's value depends on yesterday's, you need a starting value when you start off your time series. In practice, this is what the first few time series values look like:
$$ Y_0 = \text{some specified starting value}$$
$$Y_1= Y_{0}+ \epsilon_1 $$
$$Y_2= Y_{1}+ \epsilon_2 = Y_{0} + \epsilon_1 + \epsilon_2  $$
$$Y_3= Y_{2}+ \epsilon_3 = Y_{0} + \epsilon_1 + \epsilon_2 + \epsilon_3 $$
$$\ldots $$

Keeping this in mind, let's create a random walk model: 

Starting from a value of 1000 USD of a share value upon a company's first IPO (initial public offering) on January 1 2010 until end of November of the same year, generate a random walk model with a white noise error term, which has a standard deviation of 10.


```python
# Keep the random seed
np.random.seed(11)

# Create a series with the specified dates
dates = None

# White noise error term
error = None


# Define random walk
def random_walk(start, error):
    pass


shares_value = random_walk(1000, error)

shares_series = pd.Series(shares_value, index=dates)
```


```python
# __SOLUTION__
# Keep the random seed
np.random.seed(11)

# Create a series with the specified dates
dates = pd.date_range("2010-01-01", "2010-11-30", freq="B", inclusive="left")

# White noise error term
error = np.random.normal(0, 10, len(dates))


# Define random walk
def random_walk(start, error):
    Y_0 = start
    cum_error = np.cumsum(error)
    Y = cum_error + Y_0
    return Y


shares_value = random_walk(1000, error)

shares_series = pd.Series(shares_value, index=dates)
```

Visualize the time series with correct axis labels. 


```python
# Your code here
```


```python
# __SOLUTION__
ax = shares_series.plot(figsize=(14, 6))
ax.set_ylabel("Stock value", fontsize=16)
ax.set_xlabel("Date", fontsize=16)
plt.show()
```


    
![png](index_files/index_41_0.png)
    


You can see how this very much looks like the exchange rate series you looked at in the lesson!

## Random Walk with a Drift

Repeat the above, but include a drift parameter $c$ of 8 now!


```python
# Keep the random seed
np.random.seed(11)
```


```python
# __SOLUTION__
# Keep the random seed
np.random.seed(11)

# Create a series with the specified dates
dates = pd.date_range("2010-01-01", "2010-11-30", freq="B", inclusive="left")

# White noise error term
error = np.random.normal(0, 10, len(dates))


# Define random walk
def random_walk(start, error):
    Y_0 = start
    cum_error = np.cumsum(error + 8)
    Y = cum_error + Y_0
    return Y


shares_value_drift = random_walk(1000, error)

shares_series_drift = pd.Series(shares_value_drift, index=dates)
```


```python
ax = shares_series_drift.plot(figsize=(14, 6))
ax.set_ylabel("Stock value", fontsize=16)
ax.set_xlabel("Date", fontsize=16)
plt.show()
```


```python
# __SOLUTION__
ax = shares_series_drift.plot(figsize=(14, 6))
ax.set_ylabel("Stock value", fontsize=16)
ax.set_xlabel("Date", fontsize=16)
plt.show()
```


    
![png](index_files/index_47_0.png)
    


Note that there is a very strong drift here!

## Differencing in a Random Walk model

One important property of the random walk model is that a differenced random walk returns a white noise. This is a result of the mathematical formula:

$$Y_t = Y_{t-1} + \epsilon_t$$
which is equivalent to
$$Y_t - Y_{t-1} = \epsilon_t$$

and we know that $\epsilon_t$ is a mean-zero white noise process! 

Plot the differenced time series (time period of 1) for the shares time series (no drift).


```python
# Your code here
```


```python
# __SOLUTION__
shares_diff = shares_series.diff(periods=1)

fig = plt.figure(figsize=(18, 6))
plt.plot(shares_diff)
plt.title("Differenced shares series")
plt.show(block=False)
```


    
![png](index_files/index_53_0.png)
    


This does look a lot like a white noise series!

Plot the differenced time series for the shares time series (with a drift).


```python
# Your code here
```


```python
# __SOLUTION__
shares_drift_diff = shares_series_drift.diff(periods=1)

fig = plt.figure(figsize=(18, 6))
plt.plot(shares_drift_diff)
plt.title("Differenced shares (with drift) series")
plt.show(block=False)
```


    
![png](index_files/index_57_0.png)
    


This is also a white noise series, but what can you tell about the mean? 

The mean is equal to the drift $c$, so 8 for this example!

## Summary

Great, you now know how white noise and random walk models work!
