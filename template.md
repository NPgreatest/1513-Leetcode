### qSort - qSelect

```C++
void quickSort(vector<int> &target, int lp, int rp) {
    if(lp>=rp)return;
    int mid=(lp+rp)>>1;
    int tar = target[mid];
    int left=lp,right=rp;
    while(left<=right){ // <= 很重要，保证交换之后left>right
        while(target[left] < tar) left++;
        while(target[right] > tar) right--;
        if(left<=right) swap(target[left++],target[right--]);
    }
    // k-select: if (k<=right){} else {}
    quickSort(target, lp,right); //递归的下标很重要
    quickSort(target, left,rp);
}
```

### mergeSort

```C++
void msort(vector<int>& nums,int left,int right){
    if(left>=right) return;
    int mid=(left+right)/2;
    msort(nums,left,mid);
    msort(nums,mid+1,right);
    vector<int> l(nums.begin()+left,nums.begin()+mid+1)
    vector<int> r(nums.begin()+mid+1,nums.begin()+right+1);
    int lp=0,rp=0;
    while(lp<l.size() && rp<r.size()){
        if(l[lp]<r[rp]) nums[left++]=l[lp++];
        else nums[left++]=r[rp++];
    }
    while (lp<l.size()){
        nums[left++]=l[lp++];
    }
    while (rp<r.size()){
        nums[left++]=r[rp++];
    }
}
```

### heapSort

```C++
void heapify(vector<int>& arr, int n, int i) {
    int largest = i; // 初始化最大为根
    int left = 2 * i + 1; // 左子节点
    int right = 2 * i + 2; // 右子节点
    if (left < n && arr[left] > arr[largest])    largest = left;
    if (right < n && arr[right] > arr[largest])  largest = right;
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}
void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```



### Merge two sorted arrays - dummy node

```C++
while(list1 && list2){
	if(list1->val > list2->val){
		dummy->next=list2;
		list2=list2->next;
	}else{
		dummy->next=list1;
		list1=list1->next;
	}
	dummy=dummy->next;
}
if(list2) dummy->next=list2;
else dummy->next=list1;
```



### Sort List O(nlogn) O(1)

快慢指针找中点，切断节点

```C++
ListNode* sortList(ListNode* head) {
    if(!head || !head->next) return head;
    auto fast=head->next,slow=head;
    while(fast && fast->next){
        fast=fast->next->next;
        slow=slow->next;
    }
    auto next=slow->next;
    slow->next=nullptr;
    head=sortList(head);
    next=sortList(next);
    ListNode *dummy=new ListNode();
    dummy->next=head;
    auto ret=dummy;
    while(head && next){
        if(head->val > next->val){
            dummy->next=next;
            next=next->next;
        }else{
            dummy->next=head;
            head=head->next;
        }
        dummy=dummy->next;
    }
    dummy->next=head?head:next;
    auto tmp=ret->next;
    delete ret;
    return tmp;
}
```



### sorting function

```C++
// 静态函数
static bool comparePair(const pair<string, int>& pair1, const pair<string, int>& pair2) {
    if (pair1.second != pair2.second) {
        return pair1.second > pair2.second;
    } else {
        return pair1.first < pair2.first;
    }
};
//sort(arr.begin(),arr.end(),comparePair);

//匿名函数
unordered_map<string, int> cnt;
sort(arr.begin(), arr.end(), [&](const string& a, const string& b) -> bool {
            return cnt[a] == cnt[b] ? a < b : cnt[a] > cnt[b];
});

// decltype
auto cmp = [](const pair<string,int>& pair1, const pair<string,int>& pair2) -> bool{
	return pair1.second==pair2.second?pair1.first>pair2.first:pair1.second<pair2.second;
};
priority_queue<pair<string,int>,vector<pair<string,int>>, decltype(cmp)> q;
```

### 链表技巧

* 链表原地反转

```C
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }
```

* 寻找链表中点(快慢指针)

```C++
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
```



1. 链表合并
