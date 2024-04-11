# reverse numarray or mountain array
LC33搜索旋转排序数组

```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] == target){
                return mid;
            }
            if(nums[left] <= nums[mid]){ // mid 在旋转数组左段
                //判断target和左段数组的关系
                //如果target在左段数组中间 那么就缩短查找范围为左段，如果不是，那么缩短
                //查找范围为左段的右边
                if(nums[left] <= target && target < nums[mid]){ 
                    right = mid - 1;
                }
                else left = mid + 1;
            }
            else{ // mid 在旋转数组右段
                //如果target在右段
                if(nums[mid] < target && target <= nums[right]){
                    left = mid + 1;
                }
                //如果target不在右段
                else{
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```

***

LC153寻找旋转排序数组中的最小值

```
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left) / 2;
            //如果mid大于右边界，那么mid一定不可能是最小值，但是如果mid小于等于右边界
            //那么mid就有可能是最小值。所以缩短边界的时候right = mid而left = mid + 1
            if(nums[mid] <= nums[right]){
                right = mid;
            }
            else{
                left = mid + 1;
            }
        }
        return nums[left];
    }
}
```

***

LC154寻找旋转排序数组中的最小值2

```
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(nums[mid] > nums[right]){
                left = mid + 1;
            }
            else if(nums[mid] < nums[right]){
                right = mid;
            }
            else{
                right -= 1;
            }
        }
        return nums[left];
    }
}
/**
思路和找最小值类似，不过当mid的值等于right的值的时候，我们缩短右边界一位。
O(N)最坏情况下我们要一直缩短右边界，所以是线性复杂度, O(1)
 */
```

***

LC1095山脉数组查找目标值


```
class Solution {
    /**
    首先先找到峰顶索引，具体思路就是假设low加1一直比high小，然后不断的搜索mid点，如果mid点
    比mid点左边的点要大的话，代表山峰在右边，那么缩小左边界。如果mid点比mid左边的点小的话，
    代表山峰在左边，那么缩小右边界。直到low和high是相邻的两个点，那么找出他们的最大值返回即可。

    如果要找最小峰的话，那么就判断mid点是否比mid点左边小，然后返回low，high最小值即可。
     */
    public int findInMountainArray(int target, MountainArray mountainArr) {
        //先找到峰顶索引
        int low = 0, high = mountainArr.length() - 1;
        while(low + 1 < high){
            int mid = low + (high - low) / 2;
            int midVal = mountainArr.get(mid);
            if(midVal > mountainArr.get(mid - 1)){
                low = mid;
            }
            else{
                high = mid;
            }
        }
        int peak = mountainArr.get(low) > mountainArr.get(high) ? low : high;
        int index = binSearch(mountainArr, 0, peak, target, true);
        if(index == -1) return binSearch(mountainArr, peak + 1, mountainArr.length() - 1, target, false);
        return index;
    }
    /**
    二分搜索，如果是升序的话，当mid小于目标时更新左边界，当mid大于目标的时候更新右边界。
    如果是降序，那么当mid小于目标时更新右边界，当mid大于目标时更新左边界。
     */
    private int binSearch(MountainArray mountainArr, int low, int high, int target, boolean asc){
        while(low <= high){
            int mid = low + (high - low) / 2;
            int midVal = mountainArr.get(mid);
            if(midVal == target){
                return mid;
            }
            if(midVal < target){
                if(asc) low = mid + 1;
                else high = mid - 1;
            }
            else{
                if(asc) high = mid - 1;
                else low = mid + 1;
            }
        }
        return -1;
    }

}
```

O(logN), O(1)
