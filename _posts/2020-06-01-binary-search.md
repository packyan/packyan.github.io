# 二分搜索



一、寻找一个数（基本的二分搜索）**

这个场景是最简单的，肯能也是大家最熟悉的，即搜索一个数，如果存在，返回其索引，否则返回 -1。

```python
int binarySearch(int[] nums, int target) {
   int left = 0; 
   int right = nums.length - 1; // 注意

while(left <= right) { // 注意
     int mid = (right + left) / 2;
     if(nums[mid] == target)
       return mid; 
     else if (nums[mid] < target)
       left = mid + 1; // 注意
     else if (nums[mid] > target)
       right = mid - 1; // 注意
     }
   return -1;
 }

 

 


```



**二、寻找左侧边界的二分搜索**

直接看代码，其中的标记是需要注意的细节：

```python
int left_bound(int[] nums, int target) {
   if (nums.length == 0) return -1;
   int left = 0;
   int right = nums.length; // 注意

while (left < right) { // 注意
     int mid = (left + right) / 2;
     if (nums[mid] == target) {
       right = mid;
     } else if (nums[mid] < target) {
       left = mid + 1;
     } else if (nums[mid] > target) {
       right = mid; // 注意
     }
   }
   return left;
 }
```





 

**三、寻找右侧边界的二分查找**

寻找右侧边界和寻找左侧边界的代码差不多，只有两处不同，已标注：

```python
int right_bound(int[] nums, int target) {
   if (nums.length == 0) return -1;
   int left = 0, right = nums.length;

while (left < right) {
     int mid = (left + right) / 2;
     if (nums[mid] == target) {
       left = mid + 1; // 注意
     } else if (nums[mid] < target) {
       left = mid + 1;
     } else if (nums[mid] > target) {
       right = mid;
     }
   }
   return left - 1; // 注意
 }
```



