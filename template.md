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

### 二叉树

#### 非递归遍历二叉树

```C++
        while(!stk.empty() || root){
            while(root){
                // pre-order
                // res.push_back(root->val);
                stk.push(root);
                root=root->left;
            }
            root=stk.top();
            stk.pop();
            // in-order
            // res.push_back(root->val);
            // directly call: root=root->right;
            // 如果右边没有节点 或者 右边已经遍历过了 ->才输出节点并且pop out
            if(!root->right || root->right == record){
                res.push_back(root->val);
                record = root;
                root = nullptr;
            // 如果右边还有节点，往右走
            }else{
                stk.push(root);
                root=root->right;
            }
        }
```

#### Morris 遍历

```C++
        while (p1 != nullptr) {
            p2 = p1->left;
            if (p2 != nullptr) { //左边有东西需要处理
                // 持续向右走，直到找到尾或者找到链接节点
                while (p2->right != nullptr && p2->right != p1) {
                    p2 = p2->right;
                }
                if (p2->right == nullptr) { // 如果找到了尾
                    res.emplace_back(p1->val);
                    // 链接底下的节点到上面的节点，标记已经遍历过的同时方便遍历回去
                    p2->right = p1;
                    p1 = p1->left;
                    continue;
                } else { // 如果找到了连接节点，则说明p1已经通过节点跳转回来，直接断联
                    p2->right = nullptr;
                    // 中序：push_back(p1->val) 后续：addPath(p1->left);
                }
            } else { // 如果左边什么都没有，直接跳过。 后序则删除这个条件
                res.emplace_back(p1->val);
            }
            p1 = p1->right;
        }
	// 后序：addPath(root);



```

### KMP

```C++
        vector<int> next(n,0);
        // build next vector
        for(int i=1,k=0;i<n;i++){
            while(needle[i] != needle[k] && k) k=next[k-1];
            if(needle[i] == needle[k]) k++;
            next[i] = k;
        }
        // finding part
        for(int i=0,k=0;i<haystack.size();i++){
            while(haystack[i] != needle[k] && k) k=next[k-1];
            if(haystack[i] == needle[k]) k++;
            if(k == needle.size()) return i-needle.size()+1;
        }
```

