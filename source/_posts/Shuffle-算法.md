---
title: Shuffle 算法
date: 2023-10-24 18:46:36
categories: 数据结构与算法
tags: 游戏
---

其中一种常见的洗牌算法是Fisher-Yates洗牌算法，我将用它来描述原理，并给出Java实现。

**Fisher-Yates洗牌算法原理：**

1. 从数组中最后一个元素开始，从后往前遍历每个元素。
2. 对于当前遍历到的元素，随机生成一个不超过当前下标的随机数（假设为randIdx）。
3. 将当前元素与数组中下标为randIdx的元素交换位置，将当前元素放到一个随机位置。
4. 继续遍历上一个位置的元素，重复步骤2至步骤3，直到第一个元素被处理。

**Fisher-Yates洗牌算法时间复杂度和空间复杂度：**

时间复杂度：洗牌算法的时间复杂度是O(n)，其中n为数组或集合中的元素数量。因为每个元素都需要进行随机交换，而总的交换次数是n次。

空间复杂度：洗牌算法的空间复杂度是O(1)，因为算法在原地对数组或集合进行交换，没有使用额外的数据结构。

## 代码实现

```java
import java.util.Random;

public class ShuffleAlgorithm {

    public static void shuffle(int[] array) {
        Random random = new Random();
        int n = array.length;

        for (int i = n - 1; i > 0; i--) {
            int randIdx = random.nextInt(i + 1); // 生成[0, i]之间的随机数
            swap(array, i, randIdx);
        }
    }

    private static void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 4, 5};
        shuffle(nums);
        System.out.println("Shuffled array: ");
        for (int num : nums) {
            System.out.print(num + " ");
        }
    }
}

```

