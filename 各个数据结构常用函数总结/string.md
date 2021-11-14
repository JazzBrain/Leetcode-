

字符串扩容

```cpp
s.resize(s.size() + count * 2);
```

字符串抹除

```c++
//从位置pos=10处开始删除，直到结尾
s.erase(10);
//删除pos=i 个字符
s.erase(s.begin() + i)
//从位置pos=s.begin() + 1处开始删除3个字符
s.erase(s.begin() + 1, 3);


```

快慢指针去除abc   bdf  kk 字符串的空格

```c++
slow = 0 ; 
fast = 0;
while(fast < size && s[fast] == ' ') {
  fast ++;//取出前面空格
}

while(fast < size) {
  if(fast - 1 > 0 && s[fast] == ' ' && s[fast-1] == ' ') {
    fast++;
    continue;
  }
  else {
    s[slow++] = s[fast++];
  }
}

//最后要判断slow最后一位是不是' '
if(slow > 1 && s[slow - 1] == ' ') s.resize(slow-1);
else s.resize(slow);
```

