---

title: 归并排序笔记.

categories:

    - Algorithm

tags:

    - Algorithm
    - Sort
    - Merge Sort

---

## 归并排序思想

中心思想为分而治之。就是将一个大数组切分成多个小数组进行排序，再将排序后的小数组进行
合并，从而完成整体的排序。

其过程简单概括就是将一个长度为n的数组，多次切分，直到子数组长度为1，整个过程递归调用。
之后再将切分的数组两两合并成有序数组，直到整个数组合并完毕。至此排序完成。

## 图例
图片转自[博文](https://blog.csdn.net/yushiyi6453/article/details/76407640)

<!--![avatar](https://img-blog.csdn.net/20161009190940095)-->

图片非常直观的反映出了排序的过程，可以为理解归并思想有一个整体的把握，剩下的就是把此过程抽象化细节化，转化为代码。

## 思考过程

整个排序分为两个过程：

- 自上而下的切分过程
- 自下而上的合并排序过程

### 1.自下而上的合并排序过程

我們跳过切分数组，先說下自下而上的合并排序过程，個人覺得這個比较容易理解，从图例也很容易看出来。
无论数组的长度。切分到最后都是长度为1的数组。那么最后的合并过程也是从这里开始。

数组两两合并，此时很容易的就想到了二叉树，两个子节点合并成一个父节点。没错合并的过程就是如此，
左右两个节点的数组，一一取值比较，再将较小的值按顺序放到父节点数组中，以此往复完成合并。很好理解。

### 2.自上而下的切分过程

再来说下自上而下的切分过程，开始自己总觉得这个过程是不必要的，
因为最后都是切分成长度为1的数组，也就是原数组的每一个
元素为一个数组，那还递归切分做什么？实则不然，回顾合并过程，合并在逻辑上是一个二叉树的遍历流程。
而切分则是此流程的逆流程，也就是这个二叉树的构建流程。切分流程的递归调用是为合并流程
指明方向，相当于绘制设计图并分配每个区块（节点）的工人（数组），而和并过程则是工人按照设计图进行施工。
当所有工人都汇集到根结点时也就是整体排序完毕的时候。

## 将思想转化为代码（效率对最低的实现）

在理清了实现思路后就可以动手撸代码了，以下为鄙人最苯的写码及改进过程，十分啰嗦。

回顾思考过程，将方法抽象为切分/合并两个部分。

```java
    public static void splite(int[] resArray) {

    }

    public static int[] mergeArray(int[] leftArray, int[] rightArray) {

    }
```

先为切分添加方法实现
由于切分为递归调用，那么确定停止递归的条件，也就是待切分数组长度为1。

再看其他情况，需要将当前数组一份为2——左数组，右数组，这个过程可能会有数组长度为奇数的情况，
此情况下我决定偏向左数组，也就是左数组比右数组多1个元素。并用+1 /2 方式进行计算的平衡。

```java
    public static void splite(int[] resArray) {
        if (resArray.length <= 1) {//无需切分数组。
            return;
        } else {
            //计算切分中点
            int mid = (resArray.length + 1) / 2;

            //复制左数组数组
            int[] leftArray = new int[mid];
            for (int i = 0; i < mid; i++) {
                leftArray[i] = resArray[i];
            }

            splite(leftArray);

            //复制右数组数组
            int[] rightArray = new int[resArray.length - mid];
            for (int i = 0; i < rightArray.length; i++) {
                rightArray[i] = resArray[i + mid];
            }

            splite(rightArray);
        }
    }

    public static int[] mergeArray(int[] leftArray, int[] rightArray) {

    }
```

切分过程完毕，之后就行将切分后的左右数组进行排序合并，发现并没有储存切分后的数组的变量，
更改切分返回值类型，得到左右数组后进行合并排序，增加合并方法调用代码后发现整个切分方法
就是排序方法，返回值就是最后的有序数列，更改切分方法名称。

```java
    public static int[] sortArray(int[] resArray) {
        if (resArray.length <= 1) {//无需切分数组。
            return resArray;
        } else {
            //计算切分中点
            int mid = (resArray.length + 1) / 2;

            //复制左数组数组
            int[] leftArray = new int[mid];
            for (int i = 0; i < mid; i++) {
                leftArray[i] = resArray[i];
            }
            //左数组递归排序
            int[] sortedLeft = sortArray(leftArray);

            //复制右数组数组
            int[] rightArray = new int[resArray.length - mid];
            for (int i = 0; i < rightArray.length; i++) {
                rightArray[i] = resArray[i + mid];
            }
            //右数组递归排序
            int[] sortedRight = sortArray(rightArray);

            //左右数组合并
            return mergeArray(sortedLeft, sortedRight);
        }
    }

    public static int[] mergeArray(int[] leftArray, int[] rightArray) {

    }
```

排序方法完成，为合并方法添加实现。合并的过程就是从左右两个数组中取值比较，再有序的放到
结果数组中，当两数组其中一个取值完毕后，因左右两数组已经有序，将另一个数组顺次放到结果数组中即可。

```java
    public static int[] sortArray(int[] resArray) {
        if (resArray.length <= 1) {//无需切分数组。
            return resArray;
        } else {
            //计算切分中点
            int mid = (resArray.length + 1) / 2;

            //复制左数组数组
            int[] leftArray = new int[mid];
            for (int i = 0; i < mid; i++) {
                leftArray[i] = resArray[i];
            }
            //左数组递归排序
            int[] sortedLeft = sortArray(leftArray);

            //复制右数组数组
            int[] rightArray = new int[resArray.length - mid];
            for (int i = 0; i < rightArray.length; i++) {
                rightArray[i] = resArray[i + mid];
            }
            //右数组递归排序
            int[] sortedRight = sortArray(rightArray);

            //左右数组合并
            return mergeArray(sortedLeft, sortedRight);
        }
    }

    public static int[] mergeArray(int[] leftArray, int[] rightArray) {
        int[] mergedArray = new int[leftArray.length + rightArray.length];

        int leftIndex = 0;//左数组取值下标
        int rightIndex = 0;//右数组取值下标
        int mergedIndex = 0;//比较值在合并数组中的下标
        boolean sort = true;//是否继续比较排序

        while (sort) {
            if (leftIndex >= leftArray.length) { //左数组取值完毕，将右数组剩余值填入结果数组。
                for (int i = rightIndex; i < rightArray.length; i++) {
                    mergedArray[mergedIndex] = rightArray[i];
                    mergedIndex++;
                }
                sort = false;
            } else if (rightIndex >= rightArray.length) { //右数组取值完毕，将左数组剩余值填入结果数组。
                for (int i = leftIndex; i < leftArray.length; i++) {
                    mergedArray[mergedIndex] = leftArray[i];
                    mergedIndex++;
                }
                sort = false;
            } else {
                //取值比较，将较小值填入结果数组，移动对应的取值下标。
                if (leftArray[leftIndex] <= rightArray[rightIndex]) {
                    mergedArray[mergedIndex] = leftArray[leftIndex];
                    leftIndex++;
                } else {
                    mergedArray[mergedIndex] = rightArray[rightIndex];
                    rightIndex++;
                }

                //移动数组结果下标
                mergedIndex++;
            }
        }

        //合并排序结束，返回结果。
        return mergedArray;
    }
```
打完收工，躺下睡觉～
然而你看看这代码有多丑？你的良心就不会痛吗？还有心思睡觉。起来接着优化代码！！！

![avatar](https://raw.githubusercontent.com/eliyetyang/eliyetyang.github.io/master/assets/images/author/author_700x611.jpg)

## 代码优化

观察方法的实现中，有太多数组声明与数据复制。不用运行也知道是不合理的，下面就以此为切入进行优化。

简单的优化思想，左右数组也不再进行单独声明，直接从原数组中根据下标取值。

此时会产生一个问题，合并后的数组无处储存，由于数据直接从原数组中读取也不能在合并的过程中
直接进行回写，那么就需要一个缓存数组来储存合并后的数组。

遵循一次内存开辟反复使用，合并数组直接在最开始就声明，而最后一次合并所需的长度与原数组长度相同，
故其长度也与原数组相同。

为保证合并后对左右数组的取值都是有序的，需要在每次合并完成后将缓存区数组进行回写，
由于回写是在合并完成后，下次和并开始前进行，所以也不会影响排序合并过程中的取值问题。

方法的返回值也变得不再必要，排序结束后，原数组即为生序数组。

更改过程中下标使用较多，注意其取值范围不要弄混算错。

改进后的代码如下。

```java
    /**
     * 归并排序方法.
     * @param resArray 待排序数组.
     * @return 有序数组.
     */
    public static int[] mergeSort(int[] resArray) {
        int[] tempArray = new int[resArray.length];
        int start = 0;
        int end = resArray.length;
        sortArray(resArray,
                tempArray,
                start,
                end);
        return resArray;
    }

    /**
     * 切分排序方法.
     * @param resArray 待排序数组.
     * @param mergeTempArray 合并缓存数组.
     * @param startIndex 排序起始下标(含).
     * @param endIndex 排序结束下标(不含).
     */
    public static void sortArray(int[] resArray,
                                 int[] mergeTempArray,
                                 int startIndex,
                                 int endIndex) {
        if (endIndex - startIndex <= 1) {//叶节点
            return;
        } else {
            //计算中点下标（左数组结束下标，右数组初始下标）
            int midIndex = (endIndex - startIndex + 1) / 2 + startIndex; //偏移量+初始下标

            //左数组递归排序
            sortArray(resArray,
                    mergeTempArray,
                    startIndex,
                    midIndex);

            //右数组递归排序
            sortArray(resArray,
                    mergeTempArray,
                    midIndex,
                    endIndex);

            //经过以上两步后mergeTempArray中储存的都是有序数组片段
            //将左右有序数组合并成一个有序数组
            mergeArray(resArray,
                    mergeTempArray,
                    startIndex,
                    midIndex,
                    endIndex);
        }

    }

    /**
     * 排序与合并.
     * @param resArray 待排序数组.
     * @param mergeTempArray 合并缓存数组.
     * @param startIndex 待合并起始下标.---左数组起始下标(含).
     * @param midIndex 待合并中点下标.---左数组结束下标(不含);右数组开始下标(含).
     * @param endIndex 待合并结束下标。---右数组结束下标(不含).
     */
    public static void mergeArray(int[] resArray,
                                  int[] mergeTempArray,
                                  int startIndex,
                                  int midIndex,
                                  int endIndex) {

        int leftIndex = startIndex;//左数组取值下标
        int rightIndex = midIndex;//右数组取值下标
        int mergedIndex = startIndex;//比较值在合并数组中的下标
        boolean sort = true;//是否继续比较排序

        while (sort) {
            if (leftIndex >= midIndex) { //左数组取值完毕，将右数组剩余值填入结果数组。
                for (int i = rightIndex; i < endIndex; i++) {
                    mergeTempArray[mergedIndex] = resArray[i];
                    mergedIndex++;
                }
                sort = false;
            } else if (rightIndex >= endIndex) { //右数组取值完毕，将左数组剩余值填入结果数组。
                for (int i = leftIndex; i < midIndex; i++) {
                    mergeTempArray[mergedIndex] = resArray[i];
                    mergedIndex++;
                }
                sort = false;
            } else {
                //取值比较，将较小值填入缓存数组，移动对应的取值下标。
                if (resArray[leftIndex] <= resArray[rightIndex]) {
                    mergeTempArray[mergedIndex] = resArray[leftIndex];
                    leftIndex++;
                } else {
                    mergeTempArray[mergedIndex] = resArray[rightIndex];
                    rightIndex++;
                }

                //移动数组结果下标
                mergedIndex++;
            }
        }

        //本次合并完成，将合并缓存区数据回写到原数组。
        for (int i = startIndex; i < endIndex; i++) {
            resArray[i] = mergeTempArray[i];
        }
    }
```

至此基本的归并排序方法完成。写几个用例跑一下看看效果。
用例如下

```java
    @Test
    public void sortEmpty() {
        int[] resArray = {};

        assertArrayEquals(new int[]{}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortSingle() {
        int[] resArray = {1};

        assertArrayEquals(new int[]{1}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortDescend() {
        int[] resArray = {9, 8, 7, 6, 5, 4, 3, 2, 1, 0};

        assertArrayEquals(new int[]{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortAscend() {
        int[] resArray = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

        assertArrayEquals(new int[]{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortDescendOddLength() {
        int[] resArray = {9, 8, 7, 6, 5, 4, 3, 2, 1};

        assertArrayEquals(new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortAscendOddLength() {
        int[] resArray = {0, 1, 2, 3, 4, 5, 6, 7, 8};

        assertArrayEquals(new int[]{0, 1, 2, 3, 4, 5, 6, 7, 8}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortHasSameNumber() {
        int[] resArray = {0, 0, 22, 3, 5, 22, 0, 1, 90, 40, 60};

        assertArrayEquals(new int[]{0, 0, 0, 1, 3, 5, 22, 22, 40, 60, 90}, SortSecond.mergeSort(resArray));
    }

    @Test
    public void sortNormal() {
        int[] resArray = {5, 4, 9, 7, 3, 6, 2, 8, 1};

        assertArrayEquals(new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9}, SortSecond.mergeSort(resArray));
    }
```

运行结果全部通过，美滋滋～

## 复杂度分析

- 时间复杂度：
一趟归并需要将待排序列中的所有记录遍历一边进行切分，因此耗费时间为O(n),而由完全二叉树的深度可知，
整个归并排序需要进行log2n,因此，总的时间复杂度为O(nlogn)

- 空间复杂度：
优化后的代码中只使用了一个与原数组同样长度的缓存数组，所以空间复杂度为O（n）




