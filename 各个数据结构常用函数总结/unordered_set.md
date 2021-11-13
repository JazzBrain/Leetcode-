```c++
//用来查看nums2里面的元素在不在nums1中
unordered_set<int> result,nums1,nums2;
 nordered_set<int> nums_set(nums1.begin(), nums1.end());
 for (int num : nums2)
 nums_set.find(num) != nums_set.end()
      result_set.insert(num);
```

unordered_map

```c++
//num 2,3,4,5,3 
auto iter = map.find(3); //如果有重复的，那就是返回最后一个的下标
iter->second //这个返回的是4
```

