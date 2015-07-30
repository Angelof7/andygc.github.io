---
layout: post
title: 八大排序算法总结
categories: [algorithm]
tags: [Sort]
---

排序算法经过了很长时间的演变，产生了很多种不同的方法。对于初学者来说，对它们进行整理便于理解记忆显得很重要。每种算法都有它特定的使用场合，很难通用。因此，我们很有必要对所有常见的排序算法进行归纳。

![Sort.png][1]

## 一、直接插入排序(插入排序) ##
 **1. 思想**
每次选择一个元素K插入到之前已排好序的部分A[1…i]中，插入过程中K依次由后向前与A[1…i]中的元素进行比较。若发现发现A[x]>=K,则将K插入到A[x]的后面，插入前需要移动元素。

 **2. 算法时间复杂度**
最好的情况下：正序有序(从小到大)，这样只需要比较n次，不需要移动。因此时间复杂度为O(n)  
最坏的情况下：逆序有序,这样每一个元素就需要比较n次，共有n个元素，因此实际复杂度为O(n­^2)  
平均情况下：O(n­^2)

 **3. 稳定性**
*稳定性*，就是有两个相同的元素，排序先后的相对位置是否变化，主要用在排序时有多个排序规则的情况下。在插入排序中，K1是已排序部分中的元素，当K2和K1比较时，直接插到K1的后面(没有必要插到K1的前面，这样做还需要移动！！)，因此，插入排序是稳定的。

 **4. 代码实现**
{% highlight java %}
        public void insertSort(int[] arr, int n) {
            for (int i = 1; i < n; i++) {
                int tmp = arr[i];
                int j = i - 1;
                while (j >= 0 && arr[j] > tmp) {
                    arr[j + 1] = arr[j];
                    j--;
                }
                arr[j + 1] = tmp;
            }
        }
{% endhighlight %}    
## 二、希尔排序(插入排序) ##
 **1. 思想**
希尔排序也是一种插入排序方法,实际上是一种分组插入方法。先取定一个小于n的整数d1作为第一个增量,把表的全部记录分成d1个组,所有距离为d1的倍数的记录放在同一个组中,在各组内进行直接插入排序；然后,取第二个增量d2(＜d1),重复上述的分组和排序,直至所取的增量dt=1(dt<dt-1<…<d2<d1),即所有记录放在同一组中进行直接插入排序为止。    
![0_1306225549IdX1.gif.png][2]

 **2. 算法时间复杂度**
**最好情况**：由于希尔排序的好坏和步长d的选择有很多关系，因此，目前还没有得出最好的步长如何选择(现在有些比较好的选择了，但不确定是否是最好的)。所以，不知道最好的情况下的算法时间复杂度。  
**最坏情况**：O(N*logN)，最坏的情况下和平均情况下差不多。  
**平均情况**：O(N*logN)

 **3. 稳定性**
由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以shell排序是不稳定的。(*一般来说，若存在不相邻元素间交换，则很可能是不稳定的排序*。)

 **4. 代码实现**

{% highlight java %}
        public void shellSort(int[] arr, int n) {
            int d = n / 2;// 初始步长为n/2
            while (d > 0) {
                for (int i = d; i < n; i++) {
                    int j = i - d;
                    while (j >= 0 && arr[j] > arr[j + d]) {
                        // 交换arr[j]和arr[j+d]的值
                        int tmp = arr[j];
                        arr[j] = arr[j + d];
                        arr[j + d] = tmp;
                        j -= d;
                    }
                }
                d = d / 2;
            }
        }
{% endhighlight %}

## 三、冒泡排序(交换排序) ##
 **1. 思想**
通过无序区中相邻记录关键字间的比较和位置的交换,使关键字最小的记录如气泡一般逐渐往上“漂浮”直至“水面”。 

 **2. 算法时间复杂度**
最好情况下: 正序有序，则只需要比较n次。故，为O(n)  
最坏情况下: 逆序有序，则需要比较(n-1)+(n-2)+……+1，故，为O(n^2)

 **3. 稳定性**
排序过程中只交换相邻两个元素的位置。因此，当两个数相等时，是没必要交换两个数的位置的。所以，它们的相对位置并没有改变，冒泡排序算法是稳定的！

 **4. 代码实现**

{% highlight java %}
        public void bubbleSort(int[] arr, int n) {
            boolean flag = true;
            for (int i = 1; i < n; i++) {
                flag = true;//如果一趟没有数据交换就退出循环
                for (int j = 0; j < n - i; j++) {
                        if (arr[j] > arr[j + 1]) {
                        int tmp = arr[j];
                        arr[j] = arr[j + 1];
                        arr[j + 1] = tmp;
                        flag = false;
                        }
                }
                if (flag)
                    break;
            }
        }
{% endhighlight %}

## 四、快速排序(交换排序) ##
 **1. 思想**
它是由冒泡排序改进而来的。在待排序的n个记录中任取一个记录(通常取第一个记录),把该记录放入适当位置后,数据序列被此记录划分成两部分。所有关键字比该记录关键字小的记录放置在前一部分,所有比它大的记录放置在后一部分,并把该记录排在这两部分的中间(称为该记录归位),这个过程称作一趟快速排序。
最核心的思想是将小的部分放在左边，大的部分放到右边，实现分割。

 **2. 算法时间复杂度**
最好的情况下：因为每次都将序列分为两个部分(一般二分都复杂度都和logN相关)，故为 O(N*logN)  
最坏的情况下：基本有序时，退化为冒泡排序，几乎要比较N*N次，故为O(n^2)

 **3. 稳定性**
由于每次都需要和中轴元素交换，因此原来的顺序就可能被打乱。如序列为 5 3 3 4 3 8 9 10 11会将3的顺序打乱。所以说，快速排序是不稳定的！

 **4. 代码实现**

{% highlight java %}
        public void quickSort(int[] arr, int start, int end) {
            int i = start;
            int j = end;
            if (i < j) {
                int tmp = arr[i];
                while (i != j) {
                    // 从右向左找到第一个比tmp小的元素
                    while (j > i && arr[j] > tmp)
                        j--;
                    if (i < j) {
                        arr[i] = arr[j];
                        i++;
                    }
                    // 从左向右找到第一个比tmp大的元素
                    while (i < j && arr[i] < tmp)
                        i++;
                    if (i < j) {
                        arr[j] = arr[i];
                        j--;
                    }
                }
                arr[i] = tmp;
                // 递归快排左区间序列
                quickSort(arr, start, i);
                // 递归快排右区间序列
                quickSort(arr, i + 1, end);
            }
        }
{% endhighlight %}
## 五、直接选择排序(选择排序) ##
 **1. 思想**
首先在未排序序列中找到最小元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小元素，然后放到排序序列末尾。以此类推，直到所有元素均排序完毕。
具体做法是：选择最小的元素与未排序部分的首部交换，使得序列的前面为有序。

 **2. 算法时间复杂度**
最好情况下：交换0次，但是每次都要找到最小的元素，因此大约必须遍历N*N次，因此为O(n^2)。减少了交换次数！ 
最坏情况下，平均情况下：O(n^2)

 **3. 稳定性**
由于每次都是选取未排序序列A中的最小元素x与A中的第一个元素交换，因此跨距离了，很可能破坏了元素间的相对位置，因此选择排序是*不稳定*的！

 **4. 代码实现**

{% highlight java %}
        public void selectSort(int[] arr, int n) {
            for (int i = 0; i < n - 1; i++) {
                int k = i;
                //在［j－n］找到最小的值的index
                for (int j = i + 1; j < n; j++) {
                    if (arr[j] < arr[k])
                        k = j;
                }
                if (k != i) {
                    int tmp = arr[i];
                    arr[i] = arr[k];
                    arr[k] = tmp;
                }
            }
        }
{% endhighlight %}
## 六、堆排序 ##
 **1. 思想**
利用完全二叉树中双亲节点和孩子节点之间的内在关系，在当前无序区中选择关键字最大(或者最小)的记录。也就是说，以最小堆为例，根节点为最小元素，较大的节点偏向于分布在堆底附近。
![0_13062255659TRQ.gif.png][3]

 **2. 算法时间复杂度**
最坏情况下，接近于最差情况下：O(N*logN)，因此它是一种效果不错的排序算法。

 **3. 稳定性**
堆排序需要不断地调整堆，在调整过程中有可能改变相同元素的初始相对位置，因此它是一种不稳定的排序！

 **4. 代码实现**

{% highlight java %}
        public void heapSort(int[] arr, int n) {
            // 从最后一个非叶节点开始调整
            for (int i = n / 2 - 1; i >= 0; i--) {
                adjustHeap(arr, i, n - 1);
            }
            for (int i = n - 1; i > 0; i--) {
                int tmp = arr[0];
                arr[0] = arr[i];
                arr[i] = tmp;
                adjustHeap(arr, 0, i - 1);
            }
        }

        public void adjustHeap(int[] arr, int start, int end) {
            int i = start;
            int j = i * 2 + 1;// i的左节点
            int tmp = arr[i];
            while (j <= end) {
                // j指向左右节点中较大的
                if (j < end && arr[j] < arr[j + 1])
                    j++;
                if (arr[j] > tmp) {
                    arr[i] = arr[j];
                    i = j;
                    j = i * 2 + 1;
                } else {
                    break;
                }
            }
            arr[i] = tmp;
        }
{% endhighlight %}
## 七、归并排序 ##
 **1. 思想**
多次将两个或两个以上的有序表合并成一个新的有序表。
缺点是，它需要O(n)的额外空间。但是很适合于*多链表排序*。
![0_1306225570mW6M.gif.png][4]

 **2. 算法时间复杂度**
最好的情况下：一趟归并需要n次，总共需要logN次，因此为O(N*logN) 
最坏的情况下，接近于平均情况下，为O(N*logN) 
**说明**：对长度为n的文件，需进行logN 趟二路归并，每趟归并的时间为O(n)，故其时间复杂度无论是在最好情况下还是在最坏情况下均是O(nlogn)。

 **3. 稳定性**
 归并排序最大的特色就是它是一种**稳定**的排序算法。归并过程中是不会改变元素的相对位置的。 

 **4. 代码实现**

{% highlight java %}
        public class MergeSort {
        	public void sort(int[] nums, int low, int high) {
        		int mid = (low + high) / 2;
        		if (low < high) {
        			// 左边
	        		sort(nums, low, mid);
	        		// 右边
		        	sort(nums, mid + 1, high);
		        	// 左右归并
	        		merge(nums, low, mid, high);
		        }
	        }

	        public void merge(int[] nums, int low, int mid, int high) {
		        int[] temp = new int[high - low + 1];
        		int i = low;// 左指针
	        	int j = mid + 1;// 右指针
        		int k = 0;

	        	// 把较小的数先移到新数组中
        		while (i <= mid && j <= high) {
        			if (nums[i] < nums[j]) {
        				temp[k++] = nums[i++];
        			} else {
        				temp[k++] = nums[j++];
        			}
        		}

	        	// 把左边剩余的数移入数组
        		while (i <= mid) {
	        		temp[k++] = nums[i++];
        		}

	        	// 把右边边剩余的数移入数组
        		while (j <= high) {
        			temp[k++] = nums[j++];
        		}

        		// 把新数组中的数覆盖nums数组
        		for (int k2 = 0; k2 < temp.length; k2++) {
        			nums[k2 + low] = temp[k2];
        		}
        	}

	        // 归并排序的实现
            public static void main(String[] args) {
	        	int[] arr = { 3, 2, 8, 9, 1, 5, 4 };
	        	System.out.println(Arrays.toString(arr));
	        	new MergeSort().sort(arr, 0, arr.length - 1);
	        	System.out.println(Arrays.toString(arr));
	        }
	        /* Output:
	         * [3, 2, 8, 9, 1, 5, 4]
                 * [1, 2, 3, 4, 5, 8, 9]
                 */
            }
{% endhighlight %}
## 八、基数排序 ##
 **1. 思想**
它是一种非比较排序。它是根据位的高低进行排序的，也就是先按个位排序，然后依据十位排序……以此类推。示例如下：
![0_1306225575fKfh.gif.png][5]
![0_13062255771KD1.gif.png][6]
如果有一个序列，知道数的范围(比如1～1000)，用快速排序或者堆排序，需要O(N*logN)，但是如果采用基数排序，则可以达到O(4*(n+10))=O(n)的时间复杂度。算是这种情况下排序最快的！！

 **2. 算法时间复杂度**
分配需要O(n),收集为O(r),其中r为分配后链表的个数，以r=10为例，则有0～9这样10个链表来将原来的序列分类。而d，也就是位数(如最大的数是1234，位数是4，则d=4)，即"分配-收集"的趟数。因此时间复杂度为O(d*(n+r))。

 **3. 稳定性**
基数排序过程中不改变元素的相对位置，因此是**稳定**的！

 **4. 代码实现**

{% highlight java %}
        public class RadixSort {
            // 基于计数排序的基数排序算法
            private static void radixSort(int[] array, int radix, int distance) {
                // array为待排序数组
                // radix，代表基数
                // 代表排序元素的位数
                int length = array.length;
                int[] temp = new int[length];// 用于暂存元素
                int[] count = new int[radix];// 用于计数排序
                int divide = 1;
                for (int i = 0; i < distance; i++) {
                    System.arraycopy(array, 0, temp, 0, length);
                    Arrays.fill(count, 0);

                    for (int j = 0; j < length; j++) {
                        int tempKey = (temp[j] / divide) % radix;
                        count[tempKey]++;
                    }

                    for (int j = 1; j < radix; j++) {
                        count[j] = count[j] + count[j - 1];
                    }

                    for (int j = length - 1; j >= 0; j--) {
                        int tempKey = (temp[j] / divide) % radix;
                        count[tempKey]--;
                        array[count[tempKey]] = temp[j];
                    }
                    divide = divide * radix;
                }
            }
            public static void main(String[] args) {
                int[] array = { 3, 2, 3, 2, 5, 333, 45566, 2345678, 78, 990, 12, 432, 56 };
                radixSort(array, 10, 7);
                System.out.println(Arrays.toString(array));
            }
        }
{% endhighlight %}
  [1]: /album/2015/04/4058612502.png
  [2]: /album/2015/04/3691571767.png
  [3]: /album/2015/04/1100870601.png
  [4]: /album/2015/04/4290075307.png
  [5]: /album/2015/04/3842296812.png
  [6]: /album/2015/04/3132708080.png
