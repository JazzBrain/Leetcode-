# STL

### 1. capacity，size，resize，reverse

```
capacity 只代表容器容量，不可以用下表访问，因为里面没有任何对象
size 指的是此时容器中实际的元素个数
resize 都改变，默认初始化
reverse 只改变capacity，不会自动创建对象，需要自己push_back进去
```

### 2. 容器类型

```
关联容器:set,map 红黑树
序列容器:vector,list,deque
容器适配器:queue,stack,priority_queue
```

### 3. erase的失效

```
序列容器 自动指向下一个
关联容器 不影响
```

