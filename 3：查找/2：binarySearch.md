### ���ֲ���,Ҫ�����Ա�����������

### ASL��O(logn)

ע�⣺���ж��ֵݹ�**��ֹ������(left > right)��������������left <= right�������ֵݹ���Զ�� ��ѡ���ұߵ�Ϊmid**


```java
int binarySearch(int[] a,int left,int right,int key)       //�ݹ����
{
	int mid;
	if (left > right)
		return -1;                              //-1��û���ҵ�
	else
	{
		mid = (left + right) / 2;
		if (a[mid] == key)
			return mid;
		else if (a[mid] > key)
				return binarysearch(a, left ,mid - 1, key);
		     else
				return binarysearch(a, mid + 1, right, key);
	}
}


int binarySearch(int[] a, int left, int right, int key)		 //�ǵݹ����
{                                                         
	int mid;
	while(left <= right)
	{
		mid = (left + right) / 2;
		if (a[mid] == key)
			return mid;
		else if (a[mid] > key)
				right = mid - 1;
		     else
				left = mid + 1;
	}
	return -1;
}
```





