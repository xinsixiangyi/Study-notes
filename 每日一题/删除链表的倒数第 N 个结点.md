#### 删除链表的倒数第 N 个结点

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？

 

示例 1：

输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
示例 2：

输入：head = [1], n = 1
输出：[]
示例 3：

输入：head = [1,2], n = 1
输出：[1]

题解：

C

算法思路：前后指针

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
struct ListNode* fast=head;
struct ListNode* slow=head;             //前后指针
struct ListNode* pre=NULL;           //临时指针
for(int i=n-1;i>0;i--){
    fast=fast->next;                    //设定前后指针的距离
}
while(fast->next!=NULL){
    pre=slow;
    slow=slow->next;
    fast=fast->next;                    //确定删除的结点
}
if(pre==NULL){                     //边界判断
    
    return head->next;
}
 
pre->next=slow->next;           //将后结点断开
slow->next=NULL;
return head;

}
   
```

