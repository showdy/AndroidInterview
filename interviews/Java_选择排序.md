### 简单选择排序

简单选择排序(selection sort)是一种原地(in-place)排序算法, 基本思想是: 寻找序列中的最小值, 用当前位置与之交换, 重复上述步骤.

![](https://images2015.cnblogs.com/blog/1153367/201704/1153367-20170428152457772-193301923.png)

```java

  public static void selectSort(int[] arr){
        for (int i = 0; i < arr.length-1; i++) {
            for (int j = i+1; j < arr.length; j++) {
                if (arr[i] > arr[j]){
                    int tem = arr[i];
                    arr[i] = arr[j];
                    arr[j] = tem;
                }
            }
        }
    }

```

优化后:

```java

  public static void selectSort(int[] arr){
        int k=0;
        for (int i = 0; i < arr.length-1; i++) {
            for (int j = i+1; j < arr.length; j++) {
                if (arr[i] > arr[j]){
					//没必要每次都交换, 找出最小值即可.
                    k=j;
                }
            }
            int tem = arr[i];
            arr[i] = arr[k];
            arr[k] = tem;
        }
    }


```
