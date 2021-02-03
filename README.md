```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import mpld3
%matplotlib inline
sns.set_style('darkgrid')

```


```python
df = pd.read_csv('articles_clean.csv', parse_dates=['time'])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>post_id</th>
      <th>time</th>
      <th>likes</th>
      <th>comments</th>
      <th>shares</th>
      <th>user_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>781082525839810</td>
      <td>2021-01-31 20:08:10</td>
      <td>35</td>
      <td>12</td>
      <td>2</td>
      <td>100040038135466</td>
    </tr>
    <tr>
      <th>1</th>
      <td>781450709136325</td>
      <td>2021-02-01 15:17:38</td>
      <td>25</td>
      <td>16</td>
      <td>0</td>
      <td>100040038135466</td>
    </tr>
    <tr>
      <th>2</th>
      <td>782136015734461</td>
      <td>2021-02-02 19:28:30</td>
      <td>24</td>
      <td>5</td>
      <td>3</td>
      <td>100007733416641</td>
    </tr>
    <tr>
      <th>3</th>
      <td>736004423680954</td>
      <td>2020-11-22 17:13:31</td>
      <td>16</td>
      <td>18</td>
      <td>0</td>
      <td>100041413341721</td>
    </tr>
    <tr>
      <th>4</th>
      <td>781566159124780</td>
      <td>2021-02-01 19:49:39</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
      <td>100044610732957</td>
    </tr>
  </tbody>
</table>
</div>




```python
hours = np.sort(df.time.dt.hour.unique())
hours
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23])




```python
hours = [hours[:6], hours[6:12], hours[12:18], hours[18:]]
```


```python
hours
```




    [array([0, 1, 2, 3, 4, 5]),
     array([ 6,  7,  8,  9, 10, 11]),
     array([12, 13, 14, 15, 16, 17]),
     array([18, 19, 20, 21, 22, 23])]




```python
likes_avg = df.groupby(df.time.dt.hour)['likes'].mean()
likes_std = df.groupby(df.time.dt.hour)['likes'].std()
likes_max = df.groupby(df.time.dt.hour)['likes'].max()
likes_min = df.groupby(df.time.dt.hour)['likes'].min()
weight = .20
```


```python
fig, axs = plt.subplots(4, figsize=(10,25))
fig.suptitle('Articles Station Reactions by Hours', fontsize = 25)

# ================
# First 6 hours
axs[0].bar(hours[0], likes_avg[:6], weight, color = 'red', alpha = .7, label = 'Average')
axs[0].bar(hours[0] + weight, likes_std[:6], weight, alpha = .7, label = 'Standard Deviation')
axs[0].bar(hours[0] + weight*2, likes_max[:6], weight, color = 'black', alpha = .7, label = 'Max')
axs[0].bar(hours[0] + weight*3, likes_min[:6], weight, color = 'green', alpha = .7, label = 'Min')

axs[0].set_title('First 6 hours', fontsize = 20)

axs[0].set_xticks(hours[0] + weight*3 / 2)
axs[0].set_xticklabels(hours[0], fontsize=15)

axs[0].legend(fontsize = 12)

# ================
# Second 6 Hours
axs[1].bar(hours[1], likes_avg[6:12], weight, color = 'red', alpha = .7, label = 'Average')
axs[1].bar(hours[1] + weight, likes_std[6:12], weight, alpha = .7, label = 'Standard Deviation')
axs[1].bar(hours[1] + weight*2, likes_max[6:12], weight, color = 'black', alpha = .7, label = 'Max')
axs[1].bar(hours[1] + weight*3, likes_min[6:12], weight, color = 'green', alpha = .7, label = 'Min')

axs[1].set_title('Second 6 hours', fontsize = 20)

axs[1].set_xticks(hours[1] + weight*3 / 2)
axs[1].set_xticklabels(hours[1], fontsize=15)

axs[1].legend(fontsize = 12)

# ================
# Third 6 Hours
axs[2].bar(hours[2], likes_avg[12:18], weight, color = 'red', alpha = .7, label = 'Average')
axs[2].bar(hours[2] + weight, likes_std[12:18], weight, alpha = .7, label = 'Standard Deviation')
axs[2].bar(hours[2] + weight*2, likes_max[12:18], weight, color = 'black', alpha = .7, label = 'Max')
axs[2].bar(hours[2] + weight*3, likes_min[12:18], weight, color = 'green', alpha = .7, label = 'Min')

axs[2].set_title('Third 6 hours', fontsize = 20)

axs[2].set_xticks(hours[2] + weight*3 / 2)
axs[2].set_xticklabels(hours[2], fontsize=15)

axs[2].legend(fontsize = 12)

# ================
# Fourth 6 Hours
axs[3].bar(hours[3], likes_avg[18:], weight, color = 'red', alpha = .7, label = 'Average')
axs[3].bar(hours[3] + weight, likes_std[18:], weight, alpha = .7, label = 'Standard Deviation')
axs[3].bar(hours[3] + weight*2, likes_max[18:], weight, color = 'black', alpha = .7, label = 'Max')
axs[3].bar(hours[3] + weight*3, likes_min[18:], weight, color = 'green', alpha = .7, label = 'Min')

axs[3].set_title('Fourth 6 hours', fontsize = 20)

axs[3].set_xticks(hours[3] + weight*3 / 2)
axs[3].set_xticklabels(hours[3], fontsize=15)

axs[3].legend(fontsize = 12);
```


    
![png](output_6_0.png)
    



```python
df.shape
```




    (1435, 6)




```python
# Export to HTML
html_str = mpld3.fig_to_html(fig)
Html_file= open("index.html","w")
Html_file.write(html_str)
Html_file.close()
```


```python

```


```python

```
