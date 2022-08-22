# heap
```python
import heapq 

def heapsort(iterable, is_min=True): 
    if not is_min:
        iterable = [-x for x in iterable]
        
    h = []
    result = [] 
    
    for v in iterable:
        heapq.heappush(h, v)
        
    for i in range(len(h)):
        v = heapq.heappop(h) if is_min else -heapq.heappop(h)
        result.append(v)
        
    return result
    
>>> lst = [3,6,8,4,5,1,9,0]
>>> heapsort(lst)
[0, 1, 3, 4, 5, 6, 8, 9]
>>> heapsort(lst, is_min=False)
[9, 8, 6, 5, 4, 3, 1, 0]
```

