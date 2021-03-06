#memoization
메모이제이션(memoization)은 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 
동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술이다. 
동적 계획법의 핵심이 되는 기술이다. 메모아이제이션이라고도 한다.


###아래는 데코레이션으로 표현한 메모제이션

```python
def memoize(func):
    tempMemo = dict()
    def wrapped(n):
        if n in tempMemo:
            return tempMemo[n]
        result = func(n)
        tempMemo[n] = result
        return result
    return wrapped


@memoize
def fib(n):
    if n in (1, 2):
        return 1
    return fib(n - 1) + fib(n - 2)



if __name__ == '__main__':
    import time

    #processing start
    start_time = time.time()
    print(fib(35))
    #processing end
    end_time = time.time()


    #print processing time
    print(end_time - start_time)
    
```
