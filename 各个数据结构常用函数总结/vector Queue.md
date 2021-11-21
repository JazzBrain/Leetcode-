## vector

### erase

```c++
vector<int> a

vector<int>::iterator it;
it = a.begin() + i;
a.erase(it) //执行完，it之后的元素全部前移，it指向下一个元素
 
```

### Sort

```c++
 static bool cmp(const pair<int,int>& a,const pair<int,int> &b){
        return a.second > b.second;
 }
 vector<pair<int,int>> b;
 sort(b.begin(),b.end(),cmp);
```



## Queue

单调队列的设计

```c++
deque<int>q; //双端队列

class MyQueue { //单调队列（从大到小）
public:
    deque<int> que; 
    // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
    // 同时pop之前判断队列当前是否为空。
    void pop(int value) {
        if (!que.empty() && value == que.front()) {
            que.pop_front();
        }
    }
    // 这样就保持了队列里的数值是单调从大到小的了。
    void push(int value) {
        while (!que.empty() && value > que.back()) {
            que.pop_back();
        }
        que.push_back(value);
    }
  
    int front() {
        return que.front();
    }
    
};
```

