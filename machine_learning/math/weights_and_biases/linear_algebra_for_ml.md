## Linear Algebra is Important
- Linear algebra is mathematics are arrays.
```
X: Array[3]  
```
$$
X \epsilon \mathbb{R}^3
$$
- Many objects are represented by arrays in ML:
	- Data (almost always):
		- inputs and outputs
	- Models (very often):
		- equivalent to programs
	- Internal computations of models (often):
		- local models

## Core Operations
- Core operation is matrix multiplication.
- Some examples:
	- {Dot, Scalar, Inner} product
	- Correlation/covariance
	- Linear regression
	- Logistic regression
	- Principal components analysis
	- Discrete fourier transform
	- PageRank
	- Hidden layers of neural nets
	- Convolutions
	- Newton/L-BFGS

Note: GPUs are machines designed for LA.

## Matrix Multiplication is surprisingly simple
```
X = Array [3,k]
Y = Array [k,2]

result = X.Y = Array [3,2]
```
## Linear Algebra is not like Algebra
Intuitive ways of understanding LA:
- [3Blue1Brown Series](https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)
	- LA is the study of linear transformation of spaces
	- Geometric and visual reasoning
- [Graphical Linear Algebra](https://graphicallinearalgebra.net/)
	- LA is what happens when "adding meets copying"
	- Diagrammatic reasoning

## Linear Algebra is more like Programming
- Matrices = Functions
- Shapes = Types
- Multiplication = Composition

For example,
- In programming, we combine functions with matching types through function decomposition
- In linear algebra, we combine matrices with matching shapes through matrix multiplication

#### Programming Example
```python
def too_long_to_tweet(str):
	return over_140(str)

def over_140(str):
	return str.length > 140
```

Here, too_long_to_tweet is the composition of two functions
```python
len: String  -> Int
		over_140 : Int -> bool
too_long_to_tweet: 
	String -> Bool
```

| String                  | Int  | Bool  |
| ----------------------- | ---- | ----- |
| a                       | 1    | True  |
| bb                      | 2    | True  |
| "When in the course..." | 8007 | False |
|                         |      |       |
### In Linear Algebra, We Do The Same Thing:
#### Matrix-Vector multiplication is `function application`

```python
M = Array [3,k]
v = Array [k,1]

result = M.v = Array [3,1]
```

Here, it's more like
```python
M(v) : Array [k,1] -> Array [3,1]
```

That is, `M` can be thought of as a function that takes in a arrays of length `[k,1]`, and returns arrays of length `[k,3]`.

#### Matrix-Matrix Multiplication is `function composition`

```python
Z = Y.X
```
$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 0 & 0 \\
\end{bmatrix}
=\begin{bmatrix}
1 & 0 \\
0 &  0\\
\end{bmatrix} .
\begin{bmatrix}
1 & 0 &  0\\
 0&  1& 0 \\
\end{bmatrix} 
$$


![[matrix_mult_1.png]]

![[matrix_mult_2.png]]


## Why?
### Why do we use Arrays to represent Functions?
- And what kinds of functions can we represent this way?
- Why is linear algebra so important for optimization and hence ML?

#### How do we represent functions as data?
Our goal is `programming by optimization`, so we need to represent `functions` as `data`, so we can adjust them quantitatively.

- Usual way: 
	- `Strings` (sum.py)
- Other way:
	- `Dictionaries` (look-up table)

Neither `Strings` or `Dictionaries` are good for optimization.
We need a more `change-friendly` format for our programs.

If we restrict ourselves to fewer functions, we can represent data as 
- `a more limited type`


#### Instead, we focus on easy to optimize functions:
- As `trees`
	- At each step, apply a decision rule.
	- Easy to grow
		- Split branches, add rules
	- Collection of trees can be combined
	- Example: decision trees, gradient-boosted trees, random forests
- As `arrays`:
	- Using matrix multiplication as a function application
	- Easy to change slightly
		- Small change to entries -> small change to function
	- Linear Algebra can be made lightning fast
	- In Calculus, thats's huge
	- Example: linear/logistic regression, SVD/PCA, building blocks of NNs

#### Arrays Apply Linear Functions
$$
f(a+ b) = f(a) + f(b)
$$

$$
f(\lambda a) = \lambda f(a)
$$
That is, 
- if we look at the function applied to the sum of two inputs, we look at the the sum of the outputs of the function applied to the two individual inputs
- if we scale an input, we scale the output

#### Linear Functions Play Nicely with Weighted Sums
$$
f(\Sigma_{i} (\lambda_{i} . a_{i}) ) = \Sigma_{i} \lambda_{i} . f(a_{i})
$$
We can:
- pull the scalar out
- pull the `+` out
- That is, anytime we do the weighted sums of things, LA can go either inside or outside.

That is huge, because, 
- weighted functions show up all over
- makes linear functions easy to reason about

#### Interaction With 0

0 is always sent to 0
$$
f(0) = f(\lambda. 0) = \lambda. f(0) = 0
$$

If inputs collide, more has to be sent to 0.
$$
f(a) - f(b) = 0
$$
$$
f(a-b) = 0
$$Everything sent to 0 is called the kernel.

The kernel is made up of kernel sums 
$$
f(a) = 0
$$
$$
f(b) =0
$$
$$
f(a+b) =  f(a) + f(b) = 0 
$$

This collection of things sent to 0 is made up of weighted sums.

Same is true of things not the in the kernel, if you combine two things and they are not 0, they'll stay outside of the kernel.

#### Rank
These weighted sums define the `rank` of the function.
- We can make new non-kernel elements by making weighted sums of known non-kernel elements.

Rank answers the question:
- how many non-kernel elements do I need to know in order to make every element that's not in the kernel?
- 0 is not that interesting, so what inputs do I need to make something that's non-zero, that is, interesting

#### SVD is Matrix Refactoring

| Refactoring                              | LA                                |
| ---------------------------------------- | --------------------------------- |
| Separation of concerns                   | Eigenvectors (eigendecomposition) |
| Removing dead code                       | Low-rank approximation            |
| Breaking up into functions (decomposing) | Singular-value decomposition      |




