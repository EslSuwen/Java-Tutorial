# [熟记算法代码](https://www.zhihu.com/question/23148377/answer/907915556)

## 动态规划

```java
public int dpExample(int n) {
    int[] dp = new int[n + 1];  // 多开一位用来存放 0 个 1 相加的结果

    dp[0] = 0;      // 0 个 1 相加等于 0

    for (int i = 1; i <= n; ++i) {
        dp[i] = dp[i - 1] + 1;
    }

    return dp[n];
}
```

## 排序

### 冒泡排序

- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
- 针对所有的元素重复以上的步骤，除了最后一个。
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

```java
public class BubbleSort implements IArraySort {
 
     @Override
     public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        for (int i = 1; i < arr.length; i++) {
            // 设定一个标记，若为true，则表示此次循环没有进行交换，也就是待排序列已经有序，排序已经完成。
            boolean flag = true;

            for (int j = 0; j < arr.length - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;

                    flag = false;
                }
            }

            if (flag) {
                break;
            }
        }
        return arr;
    }
}
```

### 选择排序

- 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
- 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
- 重复第二步，直到所有元素均排序完毕。

```java
public class SelectionSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        // 总共要经过 N-1 轮比较
        for (int i = 0; i < arr.length - 1; i++) {
           int min = i;

           // 每轮需要比较的次数 N-i
           for (int j = i + 1; j < arr.length; j++) {
               if (arr[j] < arr[min]) {
                   // 记录目前能找到的最小值元素的下标
                   min = j;
                }
            }

            // 将找到的最小值和i位置所在的值进行交换
            if (i != min) {
                int tmp = arr[i];
                arr[i] = arr[min];
                arr[min] = tmp;
            }

        }
        return arr;
    }
}
```

### 插入排序

将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。

从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）

```java
public class InsertSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        // 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有序的
        for (int i = 1; i < arr.length; i++) {

            // 记录要插入的数据
            int tmp = arr[i];

            // 从已经排序的序列最右边的开始比较，找到比其小的数
            int j = i;
            while (j > 0 && tmp < arr[j - 1]) {
                arr[j] = arr[j - 1];
                j--;
            }

            // 存在比其小的数，插入
            if (j != i) {
                arr[j] = tmp;
            }

        }
        return arr;
    }
}
```

### 希尔排序

选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；

按增量序列个数 k，对序列进行 k 趟排序；

每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

```java
public class ShellSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        int gap = 1;
        while (gap < arr.length) {
            gap = gap * 3 + 1;
        }

        while (gap > 0) {
            for (int i = gap; i < arr.length; i++) {
                int tmp = arr[i];
                int j = i - gap;
                while (j >= 0 && arr[j] > tmp) {
                    arr[j + gap] = arr[j];
                    j -= gap;
                }
                arr[j + gap] = tmp;
            }
            gap = (int) Math.floor(gap / 3);
        }

        return arr;
    }
}
```

### 快速排序

- 从数列中挑出一个元素，称为 “基准”（pivot）;
- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

```java
public class QuickSort implements IArraySort {

    @Override
    public int[] sort(int[] sourceArray) throws Exception {
        // 对 arr 进行拷贝，不改变参数内容
        int[] arr = Arrays.copyOf(sourceArray, sourceArray.length);

        return quickSort(arr, 0, arr.length - 1);
    }

    private int[] quickSort(int[] arr, int left, int right) {
        if (left < right) {
            int partitionIndex = partition(arr, left, right);
            quickSort(arr, left, partitionIndex - 1);
            quickSort(arr, partitionIndex + 1, right);
        }
        return arr;
    }

    private int partition(int[] arr, int left, int right) {
        // 设定基准值（pivot）
        int pivot = left;
        int index = pivot + 1;
        for (int i = index; i <= right; i++) {
            if (arr[i] < arr[pivot]) {
                swap(arr, i, index);
                index++;
            }
        }
        swap(arr, pivot, index - 1);
        return index - 1;
    }

    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

}
```

## 搜索

### 二叉树的层次遍历

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    if(root == null)
        return new ArrayList<>();
    List<List<Integer>> res = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while(!queue.isEmpty()){
        int count = queue.size();
        List<Integer> list = new ArrayList<Integer>();
        while(count > 0){
            TreeNode node = queue.poll();
            list.add(node.val);
            if(node.left != null)
                queue.add(node.left);
            if(node.right != null)
                queue.add(node.right);
            count--;
        }
        res.add(list);
    }
    return res;
}

```

## 查找

### 二分查找模版

```java
int start = 0, end = nums.length - 1;
while (start + 1 < end) {
    int mid = start + (end - start) / 2;

    if (...) {
        start = mid;
    } else {
        end = mid;
    }
}
```

**查找元素的第一个和最后一个位置**

```java
public int[] searchRange(int[] nums, int target) {
    int[] result = new int[]{-1, -1};
    if (nums == null || nums.length == 0) {
        return result;
    }

    // find front
    int start = 0, end = nums.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] >= target) {
            end = mid;
        } else {
            start = mid;
        }
    }

    if (nums[start] == target) {
        result[0] = start;
    } else if (nums[end] == target) {
        result[0] = end;
    }

    // find back
    start = 0; end = nums.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] <= target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (nums[end] == target) {
        result[1] = end;
    } else if (nums[start] == target) {
        result[1] = start;
    }

    return result;
}
```

## 字符串匹配

[BM 算法](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247486150&idx=1&sn=9e9f8c35805c66132005cb634ef18171&chksm=fa0e6547cd79ec51529d0510f18161b65e54826231fae025d2cfbbd4f8a9656460f5b2d424b3&scene=21#wechat_redirect)

[BF 算法](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247485906&idx=1&sn=f00a07cbca83d345cbacc327e335de2d&chksm=fa0e6653cd79ef45a9566cd8ea947d122cfde8e1c9459332e4d7d04f06644fc7a6e81da7ee10&scene=21#wechat_redirect)

```java
int index( String S, String T, int pos ){
    int i = pos;                        // i 表示主串 S 中当前位置下标
    int j = 1;                            // j 表示子串 T 中当前位置下标

    while( i <= S[0] && j <= T[0] ){    // i 或 j 其中一个到达尾部则终止搜索
        if( S[i] == T[i] ){             // 若相等则继续进行下一个元素匹配
            i++;
            j++;
        }else {                         // 若匹配失败则 j 回溯到第一个元素重新匹配
            i = i - j + 2;              // 将 i 重新回溯到上次匹配首位的下一个元素
            j = 1;
        }
    }

    if( j > T[0] ){
        return i - T[0];
    }else {
        return 0;
    }
}
```

[KMP 算法](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=2247486490&idx=3&sn=35ba410818207a1bef83d6578f4b332c&chksm=fa0e639bcd79ea8dff1141a8729cf4b1243d23ac276652a58fc23d7b6b2ce01ca2666feab293&scene=21#wechat_redirect)

```java
public class KMP {
    private int[][] dp;
    private String pat;

    public KMP(String pat) {
        this.pat = pat;
        int M = pat.length();
        // dp[状态][字符] = 下个状态
        dp = new int[M][256];
        // base case
        dp[0][pat.charAt(0)] = 1;
        // 影子状态 X 初始为 0
        int X = 0;
        // 构建状态转移图（稍改的更紧凑了）
        for (int j = 1; j < M; j++) {
            for (int c = 0; c < 256; c++) {
                dp[j][c] = dp[X][c];
            dp[j][pat.charAt(j)] = j + 1;
            // 更新影子状态
            X = dp[X][pat.charAt(j)];
        }
    }

    public int search(String txt) {
        int M = pat.length();
        int N = txt.length();
        // pat 的初始态为 0
        int j = 0;
        for (int i = 0; i < N; i++) {
            // 计算 pat 的下一个状态
            j = dp[j][txt.charAt(i)];
            // 到达终止态，返回结果
            if (j == M) return i - M + 1;
        }
        // 没到达终止态，匹配失败
        return -1;
    }
}
```

## 线性表

### 有序数组/链表去重[快慢指针]

```java
int removeDuplicates(int[] nums){
  int n = nums.lenght;
  if(n == 0) return 0;
  int slow = 0, fast =1;
  while (fast < n) {
    if (nums[fast] != nums[slow]) {
      slow++;
      // 维护 nums[0..slow] 无重复
      nums[slow] = nums[fast];
    }
    fast++;
  }
  // 长度为索引 + 1
  return slow + 1;
}
```

```java
ListNode deleteDuplicates(ListNode head) {
  if (head == null) return null;
  ListNode slow = head, fast = head.next;
  while (fast != null) {
    if (fast.val != slow.val) {
      // nums[slow] = nums[fast];
      slow.next =fast;
      // slow++;
      slow = slow.next;
    }
    // fast++;
    fast = fast.next;
  }
  // 断开与后面重复元素的连接
  slow.next = null;
  return head;
}
```
