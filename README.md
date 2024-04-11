# -
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
