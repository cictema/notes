

## Lambda Function

### Single Argument
```python
# Lambda function that squares a number
square = lambda x: x ** 2

# Use the lambda function
result = square(5)
print(result)
```

### Multiple Argument
```python
# Lambda function that adds two numbers
add = lambda x, y: x + y

# Use the lambda function
result = add(3, 7)
print(result)
```

### With Map
```python
numbers = [1, 2, 3, 4, 5]

# Lambda function to square all numbers in the list
squared = list(map(lambda x: x ** 2, numbers))

print(squared)
```

### With Filter
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Lambda function to filter out even numbers
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))

print(even_numbers)
```