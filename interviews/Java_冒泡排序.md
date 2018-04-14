
### 冒泡排序

冒泡排序(bubble sort), 基本思想就是迭代序列中第一个元素到最后一个元素, 当需要时交换两个元素的位置.

![](https://images2015.cnblogs.com/blog/1153367/201704/1153367-20170428154208990-180812772.png)

代码实现:

```java

	public static void bubbleSort(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            for (int j = i; j < arr.length-1; j++) {
                if (arr[j]>arr[j+1]){
                    int temp= arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                }
            }
        }
    }

```
