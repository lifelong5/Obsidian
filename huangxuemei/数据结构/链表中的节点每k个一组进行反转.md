思路是让每k个进行反转，再将反转的尾部指向下一个反转的头部，这里利用了递归的优势，利用递归可以先调用到最后，再一层一层执行上来，保证后面的每k个先进行翻转完成得到了翻转后的头节点，前面的反转之后指向下一组的头节点。
```c++
#include <cstdio>
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        //tail有两个目的，第一个也就是判断这一组的节点是否满足k个如果不满足则直接返回头节点，不用进行反转
        //第二个目的，其实虽然判断是否为k个，但是其实tail指向了k+1的节点而不是第k个节点，这样tail可以作为判断反转的节点的判断条件，如果cur等于tail的话说明k个都以及翻转完成
        ListNode* tail=head;
        for(int i=0;i<k;i++){
            //如果这一部分的链表长度不满足k不用反转直接返回head
            if(tail==nullptr){
                return head;
            }
            tail=tail->next;
        }
        ListNode* last=nullptr;
        ListNode* cur=head;
        //进行反转
        while(cur!=tail){
            ListNode* n=cur->next;
            cur->next=last;
            last=cur;
            cur=n;
        }
        //反转完的尾部节点其实是一开始的头节点，在执行过程中都是用新的变量来变化，其实也是为了保存head，尾节点的next其实是下一组的头节点，利用递归调用得到下一组的头节点。
        head->next=reverseKGroup(cur, k);
        //返回头节点，反转后的头节点是last
        return last;
    }
};
```