
```python
from sklearn.model_selection import train_test_split
```


```python
x_train, x_test, y_train, y_test = train_test_split(x, y, random_state=11, test_size=0.2)
```

- split dataframe into two parts
- train it on one, and test it on another
- use df.describe() to see the mean , std, min, etc
- both will be similar
- that's why we use the split to train and test the model separately