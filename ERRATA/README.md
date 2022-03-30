# *Machine Learning with PyTorch and Scikit-Learn* Book

##  Errata



### Chapter 2


**Page 23**

- It seems that the predicted label ŷ and the true label y got flipped.

**Page 24**

> It is important to note that the convergence of the perceptron is only guaranteed if the two classes are linearly separable, which means that the two classes cannot be perfectly separated by a linear decision boundary. 

- Linearly separable means that the two classes ***can*** be perfectly separated (it mistakenly said "cannot"). [[#10](https://github.com/rasbt/machine-learning-book/issues/10)]

**Page 37**

For context: 

> The mean squared error loss is shown with an added 1/2 term which, in the text, is called out as a convenience term used to make deriving the gradient easier. It seems like use of this term is completely dropped / ignored in all later uses of this loss function.

I remember that I was thinking about whether I should use the convenience term or not. I thought I cleaned this up, but it now looks like a weird hybrid where I introduce the convenience term but then don't use it.


### Chapter 6

**Page 185**

The text says

> In this case, the sweet spot appears to be between 0.01 and 0.1 of the C value.

but "between 0.1 and 1.0" would have been more accurate. [[#27](https://github.com/rasbt/machine-learning-book/issues/27)]


**Page 188**

The following import is missing:

    >>> import scipy.stats



### Chapter 9

**Page 287**

Here, the mean absolute deviation was computed

```python
>>> def mean_absolute_deviation(data):
... return np.mean(np.abs(data - np.mean(data))) 
>>> mean_absolute_deviation(y)
58269.561754979375
```

But it makes more sense to compute the median absolute deviation

```python
>>> def median_absolute_deviation(data):
... return np.median(np.abs(data - np.median(data))) 
>>> median_absolute_deviation(y)
37000.00
```

### Chapter 11

**Page 348:**

The code comments for the `NeuralNetMLP`'s are outdated [[#23](https://github.com/rasbt/machine-learning-book/issues/23)]. Originally, I implemented the following computation

```python
z_h = np.dot(x, self.weight_h.T) + self.bias_h
```

via the equivalent 

```python
z_h = np.dot(self.weight_h, x.T).T + self.bias_h
```

(Note that in both cases `z_h` is exactly the same.)

    
The code comments reflect the second code line. For the first line, the code comment has to change and should be

```python
# input dim: [n_examples, n_features] dot [n_hidden, n_features].T
# output dim: [n_examples, n_hidden]
z_h = np.dot(x, self.weight_h.T) + self.bias_h
```

Similarly, the code comments for `z_out` should be 

```python
# input dim: [n_examples, n_hidden] dot [n_classes, n_hidden].T
# output dim: [n_examples, n_classes]
z_out = np.dot(a_h, self.weight_out.T) + self.bias_out
```

### Chapter 12

**Page 380**

> We can also simply utilize the torch.utils.data.TensorDataset class, if the second dataset is a labeled dataset in the form of tensors. So, instead of using our self-defined Dataset class, JointDataset, we can create a joint dataset as follows:
>
> **>>>** joint_dataset = JointDataset(t_x, t_y)

- Here, we mistakenly used `JointDataset` again. It should have been

```python
from torch.utils.data import TensorDataset
joint_dataset = TensorDataset(t_x, t_y)
```

**Page 397**

In the line 

```python
accuracy_hist[epoch] += is_correct.mean()
```

it should be `is_correct.sum()` instead of `is_correct.mean()`. The resulting figures etc. are all correct, though.


**Page 400**

The word "multilabel" should be "multiclass". [[#35](https://github.com/rasbt/machine-learning-book/issues/35)]


### Chapter 13

**Page 422**

In the lines

```python
... loss_hist_train[epoch] /= n_train
... accuracy_hist_train[epoch] /= n_train/batch_size
```

The first line misses the `/batch_size`.



### Chapter 14

**Page 472**

In the figure, the `y_pred` value for the `BCELoss` is 0.8, but it should be 0.69, because of sigmoid(0.8) = 0.69. You can find an updated figure [here](../ch14/figures/14_11.png).

Also, in the lines 

```python
... f'{cce_logits_loss_fn(logits, target):.4f}') CCE (w Probas): 0.5996
>>> print(f'CCE (w Logits): '
... f'{cce_loss_fn(torch.log(probas), target):.4f}') CCE (w Logits): 0.5996
```

the phrases `w Probas` and `w Logits` should be flipped.  [[#34](https://github.com/rasbt/machine-learning-book/issues/34)]




### Chapter 15

**Page 508**

In the following line

    ht = torch.matmul(xt, torch.transpose(w_xh, 0, 1)) + b_hh

the bias should be  `b_xh` instead of `b_hh`. However, the resulting output is correct.

**Page 532**

`LogSoftmax(dim=1)` is not used when defining the model -- this is correct, because `nn.CrossEntropyLoss` is designed for logits, not log-probabilities. However, the output contains a false reference to `LogSoftmax(dim=1)` (this is a left-over from editing, and it can be ignored).  [[#37](https://github.com/rasbt/machine-learning-book/issues/37)]

