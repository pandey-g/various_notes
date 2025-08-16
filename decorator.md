# Understanding `@lru_cache` in Python

`@lru_cache` is a decorator in Python's `functools` module that provides **memoization** - it caches the results of function calls to avoid repeated computation.

## What It Does

- **LRU** stands for "Least Recently Used"
- It stores function results in a cache
- When the same inputs occur again, it returns the cached result instead of recomputing
- Automatically discards least recently used items when cache exceeds its size limit

## Basic Usage

```python
from functools import lru_cache

@lru_cache(maxsize=None)  # Unlimited cache size
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

## Key Features

1. **Cache Size Control**:
   - `maxsize`: Limits how many recent calls to cache (default is 128)
   - `maxsize=None`: No size limit (cache grows indefinitely)

2. **Performance Benefits**:
   - Dramatically speeds up functions with expensive computations
   - Especially useful for recursive functions

3. **Cache Info**:
   - You can check cache statistics with `fibonacci.cache_info()`
   - Clear cache with `fibonacci.cache_clear()`

## When to Use It

- Pure functions (same input always gives same output)
- Functions with expensive computations
- Recursive functions with overlapping subproblems
- Functions called repeatedly with same arguments

## Limitations

- Only works with hashable (immutable) arguments
- Can increase memory usage
- Not suitable for functions with side effects or that depend on external state

## Example with Parameters

```python
@lru_cache(maxsize=32)
def get_expensive_data(user_id, query_params):
    # Expensive database/API call here
    return result
```

Would you like to see more specific examples or use cases?