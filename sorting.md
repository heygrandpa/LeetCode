# 这里主要是复习一下主要的几个排序算法
---
## 快速排序
**原理：** 快排的实际运用比较多。快排分为以下几个步骤：
- 指定哨兵: pivot
- 交换元素的位置，小于pivot值的均放在pivot的左边，大于pivot值的均放在pivot的右边
- 对于 <= pivot 以及 >pivot 两个部分再进行排序
- 重复以上步骤，直到每部分的区间都缩短为1

**Solution:**  
```cpp
#include<iostream>
using namespace std;

int Swap(int &a, int &b) {
	cout<<"Swap( "<< a <<","<<b<<")"<<endl;
	int temp;
	temp = a;
	a = b;
	b = temp;
	
}

int partition(int* array, int low, int high) {
	int key = array[low];
	while (low < high) {
		while (low < high && key < array[high]) high--;
		if (low == high) break;
		Swap(array[low], array[high]);
		for (int i = 0; i < 9; i++) {
			cout<<array[i]<<" ";
		}
		cout<<endl;
		while (low < high && key > array[low]) low++;
		if (low == high) break;
		Swap(array[low], array[high]);
		for (int i = 0; i < 9; i++) {
			cout<<array[i]<<" ";
		}
		cout<<endl;
	}
	return low;
}

void quicksort(int* array, int begin, int end) {
	if (begin < end) {
		int pivot = partition(array, begin, end);

		cout<<"pivot: "<<pivot<<endl;
		
		quicksort(array, begin, pivot - 1);
		quicksort(array, pivot + 1, end);
	}
}

int main() {
	int array[9] = {12, 4, 34, 6, 8, 3, 2, 988, 45};
	quicksort(array, 0, 8);
	for (int i = 0; i < 9; i++) {
		cout<<array[i]<<" ";
	}
	return 0;
} 
```

## 堆排序
参考链接：[堆排序](https://www.cnblogs.com/skywang12345/p/3602162.html)

## 归并排序
参考链接：[归并排序](https://www.cnblogs.com/rio2607/p/4489893.html)
