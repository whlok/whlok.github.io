---
title: TopK理解与实现
date: 2023-10-25 12:40:03
categories: 数据结构与算法
---

## 堆实现Topk
时间复杂度：O(nlogk)
思路：

1. 通过小顶堆实现TopK（小顶堆的堆顶元素小于左右子树的值，当设定堆空间为K时，每次选择是否更新堆顶元素，若更新则进行相应调整）
2. 构建堆：构建一个heap,元素个数为K，以完全二叉树的结构去理解数组中元素之间的关系，根节点为i(下标从0开始)，则左儿子下标为2*i+1,右儿子为2*i+2；
3. 调整小顶堆：从n/2-1的下标开始构建（完全二叉树的最后一个分支），选择左右子节点的最小值，进行交换，为确保最后所有分支都满足小顶堆，所以当每次交换后，以被换掉的下标重新开始调整
4. 如果当前元素比堆顶元素大，则更新堆顶元素，从堆顶元素的下标开始调整。
```cpp
#include <iostream>
#include <vector>
using namespace std;
void uptodown(vector<int>& heap, int k, int pos)
{
	int i = pos;
	int j = 2 * i + 1;
	while (j < k)
	{
		if (j < k - 1 && heap[j] > heap[j + 1])
			j++;
		if (heap[i] <= heap[j])
			break;
		else
		{
			swap(heap[i], heap[j]);
			i = j;
			j = 2 * i + 1;
		}
	}
}
void create_heap(vector<int>& heap, int k)
{
	int pos = k / 2 - 1;
	for (int i = pos; i >= 0; i--)
	{
		uptodown(heap, k, i);
	}
}
int main()
{
	int k = 5;
	vector<int> nums = { 12,52,78,59,46,49,65,42,15,34,28,9,5 };
	vector<int> heap(k, 0);
	for (int i = 0; i < k; ++i)
	{
		heap[i] = nums[i];
	}
	create_heap(heap, k);
	for (int i = k; i < nums.size(); ++i)
	{
		// 小顶堆
		if (nums[i] > heap[0])
		{
			heap[0] = nums[i];
			uptodown(heap, k, 0);
		}
	}
    for (int i=0; i < heap.size(); i++)
    {
        cout << heap[i] <<" ";
    }
	return 0;
}
```
## 快排实现Topk
时间复杂度：平均O(N)
思路：
1. 利用快排的思想实现，每次可以得到一个元素下标，在这个元素下标左边，所有元素比这个元素小，在这个元素右边，所有元素都比这个元素大
2. 如果右边的元素个数等于K-1，则加上当前元素，达到K个，可知TOPK的元素为这K个；
3. 如果右边的元素个数小于K-1, 则在左边范围寻找K-len个元素
4. 如果右边的元素个数大于K-1, 则在右边范围寻找K个元素
```cpp
#include <iostream>
#include <vector>
using namespace std;
int partition(vector<int>& nums, int low, int high)
{
    int pk = nums[low];
    while(low < high)
    {
        while(low < high && nums[high] >= pk)
            high--;
        nums[low] = nums[high];
        while(low < high && nums[low] < pk)
            low++;
        nums[high] = nums[low];
    }
    nums[high] = pk;
    return low;
}

int quick_topk(vector<int>& nums, int low, int high, int k)
{
    int index = -1;
    if(low < high)
    {
        int pos = partition(nums, low, high);
        int len = high - pos - 1;
        if(len == k)
            index = pos;
        else if(len < k)
        {
            index = quick_topk(nums, low, pos-1, k-len);
        }
        else
        {
            index = quick_topk(nums, pos+1, high, k);
        }
    }
    return index;
}
int main()
{
	int k = 5;
	vector<int> nums = { 12,52,1,59,46,49,65,58,15,34,28,9,5 };
    int index = quick_topk(nums, 0, nums.size()-1, k);
    for (int i=nums.size()-k; i < nums.size(); i++)
    {
        cout << nums[i] <<" ";
    }
	return 0;
}
```
