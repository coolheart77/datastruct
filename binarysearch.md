二分查找
1. 数据升序排列

左闭右闭区间
[left, right] <BR>
left <= right <br>
v < mid:  left = mid -1<BR>
v > mid:  right = mid + 1
```c++
int search(int* nums, int numsSize, int target){
    // 二分法左闭右闭
    int left = 0;
    int right = numsSize-1;
    while(left <= right)
    {
        int mid = left + (right - left)/2;
        if(*(nums + mid) > target)
        {
            right = mid - 1;
        }
        else if(*(nums + mid) < target)
        {
            left = mid + 1;
        }
        else 
            return mid;
    }
    return -1;

```

左闭右开区间
[left, right) <BR>
left < right <br>
v < mid:  left = mid -1<BR>
v > mid:  right = mid
```c++
int search(int* nums, int numsSize, int target){
	 //二分法左闭右开
    int left = 0;
    int right = numsSize;
    while(left < right)
    {
        int mid = left + (right - left)/2;
        if(*(nums + mid) > target)
        {
            right = mid;
        }
        else if(*(nums + mid) < target)
        {
            left = mid + 1;
        }
        else 
            return mid;
    }
    return -1;
}
```

1. 数据循环有序
```c
//循环严格递增数组的二分搜索 
int Search(int A[],int n, int num)  
{  
    int left = 0 ;  
    int right = n - 1 ;  
    int mid = 0 ;  
    int pos = -1 ;    //返回-1，表示查找失败   
  
    while(left <= right)  
    {  
        mid = (left + right)/2 ;  
  
        if (A[mid] == num)  
        {     
            pos = mid ;   
            break;       
        }  
        if (A[left] <= A[mid])    //前半部分是严格递增的，后半部分是一个更小的循环递增数组   
        {  
            if(A[left] <= num && num  < A[mid] )  
            {  
                right = mid - 1 ;  
            }  
            else  
            {  
                left = mid + 1 ;  
            }   
        }  
        else    //后半部分是严格递增的，前半部分是一个更小的循环递增数组   
        {  
            if ( A[mid] < num && num <= A[right])  
            {  
                left = mid + 1 ;  
            }  
            else  
            {  
                right = mid - 1 ;  
            }  
        }  
    }  
    return pos;  
}  
```

二分查找
二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法。但是，折半查找要求线性表必须采用顺序存储结构，而且表中元素按关键字有序排列。其中适用二分查找的问题的元素排列的「有序性」不是必须的，「有序性」只是「可二分性」或者大家常说的「二段性」的一种。

查找过程
首先，假设表中元素是按升序排列，将表中间位置记录的关键字与查找关键字比较，如果两者相等，则查找成功；否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表。重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。

边界问题
总结为一句话：左闭左+1，右闭右-1，开区间选mid

说到二分查找相信我们都能说出个一二三，但是每次到写代码时，却总是差一点，最多情况往往是边界不知道怎么取，现在就说一下，二分查找存在的边界问题。

二分查找涉及的很多的边界条件，逻辑比较简单，但就是写不好。相信很多同学都和我一样，在条件判断时总是不知道是 while(left < right) 还是 while(left <= right)，到底是right = mid呢，还是要right = mid - 1呢？

大家写二分法经常写乱，主要是因为对区间的定义没有想清楚，区间的定义就是不变量。要在二分查找的过程中，保持不变量，就是在while寻找中每一次边界的处理都要坚持根据区间的定义来操作，这就是循环不变量规则。

写二分法，区间的定义有以下四种，左闭右闭即[left, right]，或者左闭右开即[left, right)，或者左开右闭即(left, right]，或者左开右开即(left, right)，其中左闭右闭即[left, right]比较常用，基本思路不变只是控制了一些变量选择

下面我用这四种区间的定义分别讲解四种不同的二分写法。

以下分析基于理论情况，实际题目中我们比较常用第一种情况和第二种情况

二分法第一种写法，左闭右闭即[left, right]
第一种写法，我们定义 target 是在一个在左闭右闭的区间里，也就是[left, right] （这个很重要非常重要）。

区间的定义这就决定了二分法的代码应该如何写，因为定义target在[left, right]区间：

while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
if (nums[mid] > target) right 要赋值为 mid - 1，因为当前这个nums[mid]一定不是target，那么接下来要查找的左区间结束下标位置就是 mid - 1，因为区间 -1了，所以取不到mid了
if (nums[mid] < target) left 要赋值为 mid +1，因为当前这个nums[mid]一定不是target，那么接下来要查找的右区间结束下标位置就是 mid + 1，因为区间 +1了，所以取不到mid了
if (nums[mid] == target) 找到了我们需要的值，返回下标mid
例如在数组：1,2,3,4,7,9,10中查找元素2


代码如下：（详细注释）

```c
// 版本一
while(left <= right)// 因为left == right的时候，在[left, right]是有效的空间，即相等时可以取到该元素，所以使用 <=
{
    int mid = (left + right)/2;
    //如果left+right过大，导致和溢出，可以用mid = left + (right - left) / 2,防止溢出left+right
    if(nums[mid] > target)
    {
        right = mid - 1;// target 在左区间，所以[left, mid - 1]
    }
    else if(nums[mid] < target)
    {
        left = mid + 1;// target 在右区间，所以[mid + 1, right]
    }
    else if(nums[mid] == target)
    {
        return mid;// 数组中找到目标值，直接返回下标
    }
}
```
二分法第二种写法，左闭右开即[left, right)
如果说定义 target 是在一个在左闭右开的区间里，也就是[left, right) ，那么二分法的边界处理方式则截然不同。

while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
if (nums[mid] > target) right 更新为 mid，因为当前nums[mid]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为mid，即：下一个查询区间不会去比较nums[mid]，因为是开区间，所以取不到
if (nums[mid] < target) left 更新为 mid + 1，因为当前nums[mid]不等于target，去右区间继续寻找，而寻找区间是左闭右开区间，所以left更新为mid + 1，即：下一个查询区间不会去比较nums[mid]，因为区间+1了，所以取不到mid了
if (nums[mid] == target) 找到了我们需要的值，返回下标mid
在数组：1,2,3,4,7,9,10中查找元素2，如图所示：（注意和方法一的区别）


代码如下：（详细注释）

```c
// 版本二
while(left < right)// 因为left == right的时候，在[left, right)是无效的空间，即相等时取不到该元素，所以使用 <
{
    int mid = (left + right)/2;
    //如果left+right过大，导致和溢出，可以用mid = left + (right - left) / 2,防止溢出left+right
    if(nums[mid] > target)
    {
        right = mid;// target 在左区间，所以[left, mid)
    }
    else if(nums[mid] < target)
    {
        left = mid + 1;// target 在右区间，所以[mid + 1, right)
    }
    else if(nums[mid] == target)
    {
        return mid;// 数组中找到目标值，直接返回下标
    }
}
```
二分法第三种写法，左开右闭即(left, right]
如果说定义 target 是在一个在左开右闭的区间里，也就是(left, right] ，那么二分法的边界处理方式则截然不同。

while (left < right)，这里使用 < ,因为left == right在区间(left, right]是没有意义的
if (nums[mid] > target) right 更新为 mid - 1，因为当前nums[mid]不等于target，去左区间继续寻找，而寻找区间是左开右闭区间，所以right更新为mid - 1，即：下一个查询区间不会去比较nums[mid]，因为区间-1了，所以取不到mid了
if (nums[mid] < target) left 更新为 mid，因为当前nums[mid]不等于target，去右区间继续寻找，而寻找区间是左开右闭区间，所以left更新为mid，即：下一个查询区间不会去比较nums[mid]，因为是开区间，所以取不到
if (nums[mid] == target) 找到了我们需要的值，返回下标mid
在数组：1,2,3,4,7,9,10中查找元素7，如图所示：（注意和方法二的区别）


```c
// 版本三
while(left < right)// 因为left == right的时候，在(left, right]是无效的空间，即相等时取不到该元素，所以使用 <
{
    int mid = (left + right)/2;
    //如果left+right过大，导致和溢出，可以用mid = left + (right - left) / 2,防止溢出left+right
    if(nums[mid] > target)
    {
        right = mid - 1;// target 在左区间，所以(left, mid]
    }
    else if(nums[mid] < target)
    {
        left = mid;// target 在右区间，所以(mid + 1, right]
    }
    else if(nums[mid] == target)
    {
        return mid;// 数组中找到目标值，直接返回下标
    }
}
```
二分法第四种写法，左开右开即(left, right)
如果说定义 target 是在一个在左开右开的区间里，也就是(left, right) ，那么二分法的边界处理方式则截然不同。

while (left < right)，这里使用 < ,因为left == right在区间(left, right)是没有意义的
if (nums[mid] > target) right 更新为 mid ，因为当前nums[mid]不等于target，去左区间继续寻找，而寻找区间是左开右闭区间，所以right更新为mid ，即：下一个查询区间不会去比较nums[mid]，因为是开区间，所以取不到
if (nums[mid] < target) left 更新为 mid，因为当前nums[mid]不等于target，去右区间继续寻找，而寻找区间是左开右闭区间，所以left更新为mid，即：下一个查询区间不会去比较nums[mid]，因为是开区间，所以取不到
if (nums[mid] == target) 找到了我们需要的值，返回下标mid
在数组：1,2,3,4,7,9,10中查找元素7，如图所示：（注意和方法三的区别）


```c
// 版本四
while(left < right)// 因为left == right的时候，在(left, right)是无效的空间，即相等时取不到该元素，所以使用 <
{
    int mid = (left + right)/2;
    //如果left+right过大，导致和溢出，可以用mid = left + (right - left) / 2,防止溢出left+right
    if(nums[mid] > target)
    {
        right = mid;// target 在左区间，所以(left, mid)
    }
    else if(nums[mid] < target)
    {
        left = mid;// target 在右区间，所以(mid, right)
    }
    else if(nums[mid] == target)
    {
        return mid;// 数组中找到目标值，直接返回下标
    }
}
```
总结
二分法是非常重要的基础算法，为什么会对于二分法都是一看就会，一写就废？

其实主要就是对区间的定义没有理解清楚，在循环中没有始终坚持根据查找区间的定义来做边界处理。

区间的定义就是不变量，那么在循环中坚持根据查找区间的定义来做边界处理，就是循环不变量规则。

```c++
class Solution {
    // 代码1:左边界
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] < target) {
                l = mid + 1;
            } else {
                // 当找到目标值，我们选择继续缩小左区间
                r = mid;
            }
        }
        return l;
    }
    // 代码2:右边界
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (nums[mid] < target) {
                r = mid;
            } else {
                // 当找到目标值，我们选择继续缩小右区间
                l = mid + 1;
            }
        }
        return l - 1;
    }
}
```
ps：上述采用左闭右开的写法
• 上述的写法中返回l和r无区别，因为循环的终止条件是 l == r;
• 为什么右边界要减1呢？因为我们对 left 的更新必须是 l = mid + 1，就是说 while 循环结束时，nums[l] 一定不等于 target 了，而 nums[l-1] 可能是 target。
• 对于查找的元素不存在的情况可以单独判断。添加下面代码判断即可：

```c++
// 左边界
if (l >= nums.length || nums[l] != target) {
  return -1;
}
// 右边界
if (l == 0 || nums[l - 1] != target) {
  return -1;
}
```
