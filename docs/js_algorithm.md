# js常见算法

## 最大子数组和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

> 输入: [-2,1,-3,4,-1,2,1,-5,4],
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

暴力破解法——时间效率O(N^3)，超时

```javascript
function maxSubArray(nums) {
    var max = -Infinity;
    for (var i = 0; i < nums.length; i++) {// 子数组左端点
        for (var j = i; j < nums.length; j++) {// 子数组右端点
            var sum = 0;
            for (var k = i; k <= j; k++) {//暴力计算
                sum += nums[k];
            }
            if (sum > max) {
                max = sum;
            }
        }
    }
    return max;
}
console.log(maxSubArray([-2,1,-3,4,-1,2,1,-5,4]))
```

改进暴力法——时间效率O(N^2)

其实我们可以发现，每次我们都是重复计算了一部分子序列，即当我计算前两个时，第三次我还是会计算前两个在加第三个，这样就造成了O(N^3)，现在我们根据前一次的进行计算，那么将会减少一层循环。

```javascript
function maxSubArray(nums) {
    var max = -Infinity;
    for (var i = 0; i < nums.length; i++) {// 子数组左端点
        var sum = 0;
        for (var j = i; j < nums.length; j++) {// 子数组右端点
             sum += nums[j];
            if (sum > max) {
                max = sum;
            }
        }
    }
    return max
}
console.log(maxSubArray([-2,1,-3,4,-1,2,1,-5,4]))
```

动态规则——时间效率O(N)

```javascript
function maxSubArray(nums) {
    var sum=nums[0];
    var n=nums[0];
    for(var i=1;i<nums.length;i++) {
        if(n >0) {
          n+=nums[i];
        }else{
          n = nums[i]
        }
        if(sum<n){
           sum=n;
        }
    }
    return sum;
}
console.log(maxSubArray([-2,1,-3,4,-1,2,1,-5,4]))
```

