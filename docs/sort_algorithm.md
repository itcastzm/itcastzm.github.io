# js实现十种排序算法

比较排序和非比较排序
常见的排序算法都是比较排序，非比较排序包括计数排序、桶排序和基数排序，非比较排序对数据有要求，因为数据本身包含了定位特征，所有才能不通过比较来确定元素的位置。比较排序的时间复杂度通常为O(n2)或者O(nlogn)，比较排序的时间复杂度下界就是O(nlogn)，而非比较排序的时间复杂度可以达到O(n)，但是都需要额外的空间开销。
比较排序时间复杂度为O(nlogn)的证明：
`a1,a2,a3……an序列的所有排序有n!种，所以满足要求的排序a1',a2',a3'……an'（其中a1'<=a2'<=a3'……<=an'）的概率为1/n!。基于输入元素的比较排序，每一次比较的返回不是0就是1，这恰好可以作为决策树的一个决策将一个事件分成两个分支。比如冒泡排序时通过比较a1和a2两个数的大小可以把序列分成a1,a2……an与a2,a1……an（气泡a2上升一个身位）两种不同的结果，因此比较排序也可以构造决策树。根节点代表原始序列a1,a2,a3……an，所有叶子节点都是这个序列的重排（共有n!个，其中有一个就是我们排序的结果a1',a2',a3'……an'）。如果每次比较的结果都是等概率的话（恰好划分为概率空间相等的两个事件），那么二叉树就是高度平衡的，深度至少是log(n!)。`
又因为   
1. n! < nn ,两边取对数就得到log(n!)<nlog(n)，所以log(n!) = O(nlogn).
2. n!=n(n-1)(n-2)(n-3)…1 > (n/2)^(n/2) 两边取对数得到 log(n!) > (n/2)log(n/2) = Ω(nlogn)，所以 log(n!) = Ω(nlogn)。
`因此log(n!)的增长速度与 nlogn 相同,即 log(n!)=Θ(nlogn)，这就是通用排序算法的最低时间复杂度O(nlogn)的依据。`

排序的稳定性和复杂度
不稳定：
- 选择排序（selection sort）— O(n2)
- 快速排序（quicksort）— O(nlogn) 平均时间, O(n2) 最坏情况; 对于大的、乱序串列一般认为是最快的已知排序
- 堆排序 （heapsort）— O(nlogn)
- 希尔排序 （shell sort）— O(nlogn)
- 基数排序（radix sort）— O(n·k); 需要 O(n) 额外存储空间 （K为特征个数）

稳定：
- 冒泡排序（bubble sort） — O(n2)
- 插入排序（insertion sort）— O(n2)
- 归并排序 （merge sort）— O(n log n); 需要 O(n) 额外存储空间
- 二叉树排序（Binary tree sort） — O(nlogn); 需要 O(n) 额外存储空间
- 计数排序  (counting sort) — O(n+k); 需要 O(n+k) 额外存储空间，k为序列中Max-Min+1
- 桶排序 （bucket sort）— O(n); 需要 O(k) 额外存储空间

## 冒泡排序（Bubble Sort）
冒泡排序须知：
作为最简单的排序算法之一，冒泡排序给我的感觉就像Abandon在单词书里出现的感觉一样，每次都在第一页第一位，所以最熟悉。。。冒泡排序还有一种优化算法，就是立一个flag，当在一趟序列遍历中元素没有发生交换，则证明该序列已经有序。但这种改进对于提升性能来说并没有什么太大作用。。。

什么时候最快（Best Cases）：
当输入的数据已经是正序时（都已经是正序了，我还要你冒泡排序有何用啊。。。。）

什么时候最慢（Worst Cases）：
当输入的数据是反序时（写一个for循环反序输出数据不就行了，干嘛要用你冒泡排序呢，我是闲的吗。。。）
冒泡排序动图演示： 
[![冒泡演示图](sort_algorithm/maopao.gif)](docs/sort_algorithm/maopao.gif)
冒泡排序JavaScript代码实现：
```javascript
function bubbleSort(arr) {
    var len = arr.length;
    for (var i = 0; i < len; i++) {
        for (var j = 0; j < len - 1 - i; j++) {
            if (arr[j] > arr[j+1]) {        //相邻元素两两对比
                var temp = arr[j+1];        //元素交换
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
```

## 选择排序（Selection Sort）
选择排序须知：
在时间复杂度上表现最稳定的排序算法之一，因为无论什么数据进去都是O(n²)的时间复杂度。。。所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。

选择排序动图演示：
[![选择排序](sort_algorithm/xuanze.gif)](docs/sort_algorithm/xuanze.gif)
Selection Sort 动图演示 算法可视化来源：http://visualgo.net/
选择排序JavaScript代码实现：
```javascript
function selectionSort(arr) {
    var len = arr.length;
    var minIndex, temp;
    for (var i = 0; i < len - 1; i++) {
        minIndex = i;
        for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {     //寻找最小的数
                minIndex = j;                 //将最小数的索引保存
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
}
```
## 插入排序（Insertion Sort）
插入排序须知：
插入排序的代码实现虽然没有冒泡排序和选择排序那么简单粗暴，但它的原理应该是最容易理解的了，因为只要打过扑克牌的人都应该能够秒懂。当然，如果你说你打扑克牌摸牌的时候从来不按牌的大小整理牌，那估计这辈子你对插入排序的算法都不会产生任何兴趣了。。。
插入排序和冒泡排序一样，也有一种优化算法，叫做拆半插入。对于这种算法，得了懒癌的我就套用教科书上的一句经典的话吧：感兴趣的同学可以在课后自行研究。。。
插入排序动图演示：
[![插入排序](sort_algorithm/charu.gif)](docs/sort_algorithm/charu.gif)
Insertion Sort 动图演示 算法可视化来源：http://visualgo.net/
插入排序JavaScript代码实现：
```javascript
function insertionSort(arr) {
    var len = arr.length;
    var preIndex, current;
    for (var i = 1; i < len; i++) {
        preIndex = i - 1;
        current = arr[i];
        while(preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex+1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex+1] = current;
    }
    return arr;
}

```

## 希尔排序（Shell Sort）
希尔排序须知：
希尔排序是插入排序的一种更高效率的实现。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序的核心在于间隔序列的设定。既可以提前设定好间隔序列，也可以动态的定义间隔序列。动态定义间隔序列的算法是《算法（第4版》的合著者Robert Sedgewick提出的。在这里，我就使用了这种方法。

希尔排序JavaScript代码实现：
```javascript
function shellSort(arr) {
    var len = arr.length,
        temp,
        gap = 1;
    while(gap < len/3) {          //动态定义间隔序列
        gap =gap*3+1;
    }
    for (gap; gap > 0; gap = Math.floor(gap/3)) {
        for (var i = gap; i < len; i++) {
            temp = arr[i];
            for (var j = i-gap; j >= 0 && arr[j] > temp; j-=gap) {
                arr[j+gap] = arr[j];
            }
            arr[j+gap] = temp;
        }
    }
    return arr;
}
```

## 归并排序（Merge Sort）
归并排序须知：
作为一种典型的分而治之思想的算法应用，归并排序的实现由两种方法：

自上而下的递归（所有递归的方法都可以用迭代重写，所以就有了第2种方法）
自下而上的迭代
在《数据结构与算法JavaScript描述》中，作者给出了自下而上的迭代方法。但是对于递归法，作者却认为：
` However, it is not possible to do so in JavaScript, as the recursion goes too deep for the language to handle.  然而,在 JavaScript 中这种方式不太可行,因为这个算法的递归深度对它来讲太深了。`

说实话，我不太理解这句话。意思是JavaScript编译器内存太小，递归太深容易造成内存溢出吗？还望有大神能够指教。
和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(n log n）的时间复杂度。代价是需要额外的内存空间
归并排序动图演示：
[![归并排序](sort_algorithm/guibing.gif)](docs/sort_algorithm/guibing.gif)

Merge Sort 动图演示 算法可视化来源：http://visualgo.net/
归并排序JavaScript代码实现：
```javascript
function mergeSort(arr) {  //采用自上而下的递归方法
    var len = arr.length;
    if(len < 2) {
        return arr;
    }
    var middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right)
{
    var result = [];

    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }

    while (left.length)
        result.push(left.shift());

    while (right.length)
        result.push(right.shift());

    return result;
}
```

## 快速排序（Quick Sort）
快速排序须知：
又是一种分而治之思想在排序算法上的典型应用。本质上来看，快速排序应该算是在冒泡排序基础上的递归分治法。
快速排序的名字起的是简单粗暴，因为一听到这个名字你就知道它存在的意义，就是快，而且效率高! 它是处理大数据最快的排序算法之一了。虽然Worst Case的时间复杂度达到了O(n²)，但是人家就是优秀，在大多数情况下都比平均时间复杂度为O(n log n) 的排序算法表现要更好，可是这是为什么呢，我也不知道。。。好在我的强迫症又犯了，查了N多资料终于在《算法艺术与信息学竞赛》上找到了满意的答案：
`快速排序的最坏运行情况是O(n²)，比如说顺序数列的快排。但它的平摊期望时间是O(n log n) ，且O(n log n)记号中隐含的常数因子很小，比复杂度稳定等于O(n log n)的归并排序要小很多。所以，对绝大多数顺序性较弱的随机数列而言，快速排序总是优于归并排序。`

快速排序动图演示：
[![快速排序](sort_algorithm/kuaisu.gif)](docs/sort_algorithm/kuaisu.gif)
Quick Sort 动图演示 算法可视化来源：http://visualgo.net/
快速排序JavaScript代码实现：
```javascript
function quickSort(arr, left, right) {
    var len = arr.length,
        partitionIndex,
        left = typeof left != 'number' ? 0 : left,
        right = typeof right != 'number' ? len - 1 : right;

    if (left < right) {
        partitionIndex = partition(arr, left, right);
        quickSort(arr, left, partitionIndex-1);
        quickSort(arr, partitionIndex+1, right);
    }
    return arr;
}

function partition(arr, left ,right) {     //分区操作
    var pivot = left,                      //设定基准值（pivot）
        index = pivot + 1;
    for (var i = index; i <= right; i++) {
        if (arr[i] < arr[pivot]) {
            swap(arr, i, index);
            index++;
        }        
    }
    swap(arr, pivot, index - 1);
    return index-1;
}

function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

## 堆排序（Heap Sort）
堆排序须知：
堆排序可以说是一种利用堆的概念来排序的选择排序。分为两种方法：
1. 大顶堆：每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列
2. 小顶堆：每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列

堆排序动图演示：
[![堆排序](sort_algorithm/dui.gif)](docs/sort_algorithm/dui.gif)
Heap Sort 动图演示 算法可视化来源：`http://www.ee.ryerson.ca/~courses/coe428/sorting/heapsort.html`
堆排序JavaScript代码实现：

```javascript
//因为声明的多个函数都需要数据长度，所以把len设置成为全局变量
var len;    
function buildMaxHeap(arr) {   //建立大顶堆
    len = arr.length;
    for (var i = Math.floor(len/2); i >= 0; i--) {
        heapify(arr, i);
    }
}
 //堆调整
function heapify(arr, i) {    
    var left = 2 * i + 1,
        right = 2 * i + 2,
        largest = i;

    if (left < len && arr[left] > arr[largest]) {
        largest = left;
    }

    if (right < len && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, largest);
    }
}

function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

function heapSort(arr) {
    buildMaxHeap(arr);

    for (var i = arr.length-1; i > 0; i--) {
        swap(arr, 0, i);
        len--;
        heapify(arr, 0);
    }
    return arr;
}
```

## 计数排序（Counting Sort）
计数排序须知：
计数排序的核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。
作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

计数排序动图演示：
[![计数排序](sort_algorithm/jishu.gif)](docs/sort_algorithm/jishu.gif)
Counting Sort 动图演示 算法可视化来源：`http://visualgo.net/`
计数排序JavaScript代码实现：
```javascript
function countingSort(arr, maxValue) {
    var bucket = new Array(maxValue+1),
        sortedIndex = 0;
        arrLen = arr.length,
        bucketLen = maxValue + 1;

    for (var i = 0; i < arrLen; i++) {
        if (!bucket[arr[i]]) {
            bucket[arr[i]] = 0;
        }
        bucket[arr[i]]++;
    }

    for (var j = 0; j < bucketLen; j++) {
        while(bucket[j] > 0) {
            arr[sortedIndex++] = j;
            bucket[j]--;
        }
    }

    return arr;
}
```
## 桶排序（Bucket Sort）
桶排序须知：
桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。
为了使桶排序更加高效，我们需要做到这两点：
1. 在额外空间充足的情况下，尽量增大桶的数量
2. 使用的映射函数能够将输入的N个数据均匀的分配到K个桶中
同时，对于桶中元素的排序，选择何种比较排序算法对于性能的影响至关重要。

什么时候最快（Best Cases）：
当输入的数据可以均匀的分配到每一个桶中

什么时候最慢（Worst Cases）：
当输入的数据被分配到了同一个桶中

桶排序JavaScript代码实现：
```javascript
function bucketSort(arr, bucketSize) {
    if (arr.length === 0) {
      return arr;
    }

    var i;
    var minValue = arr[0];
    var maxValue = arr[0];
    for (i = 1; i < arr.length; i++) {
      if (arr[i] < minValue) {
          minValue = arr[i];                //输入数据的最小值
      } else if (arr[i] > maxValue) {
          maxValue = arr[i];                //输入数据的最大值
      }
    }

    //桶的初始化
    var DEFAULT_BUCKET_SIZE = 5;            //设置桶的默认数量为5
    bucketSize = bucketSize || DEFAULT_BUCKET_SIZE;
    var bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1;   
    var buckets = new Array(bucketCount);
    for (i = 0; i < buckets.length; i++) {
        buckets[i] = [];
    }

    //利用映射函数将数据分配到各个桶中
    for (i = 0; i < arr.length; i++) {
        buckets[Math.floor((arr[i] - minValue) / bucketSize)].push(arr[i]);
    }

    arr.length = 0;
    for (i = 0; i < buckets.length; i++) {
        insertionSort(buckets[i]);                      //对每个桶进行排序，这里使用了插入排序
        for (var j = 0; j < buckets[i].length; j++) {
            arr.push(buckets[i][j]);                      
        }
    }

    return arr;
}

```
## 基数排序（Radix Sort）
基数排序须知：
基数排序有两种方法：
1. MSD 从高位开始进行排序
2. LSD 从低位开始进行排序
基数排序 vs 计数排序 vs 桶排序
这三种排序算法都利用了桶的概念，但对桶的使用方法上有明显差异：
基数排序：根据键值的每位数字来分配桶
计数排序：每个桶只存储单一键值
桶排序：每个桶存储一定范围的数值

LSD基数排序动图演示：
[![基数排序](sort_algorithm/jishu_基数.gif)](docs/sort_algorithm/jishu_基数.gif)
Radix Sort 动图演示 算法可视化来源：`http://visualgo.net/`
基数排序JavaScript代码实现：
```javascript 
//LSD Radix Sort
var counter = [];
function radixSort(arr, maxDigit) {
    var mod = 10;
    var dev = 1;
    for (var i = 0; i < maxDigit; i++, dev *= 10, mod *= 10) {
        for(var j = 0; j < arr.length; j++) {
            var bucket = parseInt((arr[j] % mod) / dev);
            if(counter[bucket]==null) {
                counter[bucket] = [];
            }
            counter[bucket].push(arr[j]);
        }
        var pos = 0;
        for(var j = 0; j < counter.length; j++) {
            var value = null;
            if(counter[j]!=null) {
                while ((value = counter[j].shift()) != null) {
                      arr[pos++] = value;
                }
          }
        }
    }
    return arr;
}
```

## 二叉树排序
二叉树排序法借助了数据结构二叉排序树，二叉排序数满足三个条件：
（1）若左子树不空，则左子树上所有结点的值均小于它的根结点的值； 
（2）若右子树不空，则右子树上所有结点的值均大于它的根结点的值； 
（3）左、右子树也分别为二叉排序树。根据这三个特点，用中序遍历二叉树得到的结果就是排序的结果。

二叉树排序法需要首先根据数据构建二叉排序树，然后中序遍历，排序时间复杂度为O(nlogn)，构建二叉树需要额外的O(n)的存储空间，有相同的元素是可以设置排在后边的放在右子树，在中序变量的时候也会在后边，所以二叉树排序是稳定的。
在实现此算法的时候遇到不小的困难，指针参数在函数中无法通过new赋值，后来采用取指针地址，然后函数设置BST** tree的方式解决。



## 参考
[js十大排序算法](https://www.cnblogs.com/AlbertP/p/10847627.html)
[各种排序算法总结和比较](https://www.cnblogs.com/zhaoshuai1215/p/3448154.html)
[学习排序算法，结合这个方法太容易理解了](https://blog.csdn.net/weixin_44811417/article/details/90748011)