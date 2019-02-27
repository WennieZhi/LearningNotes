#### Reverse ListNode

* ListNode

  每个node都有一个next指针，用来指向下一个node

##### 暴力破解

* 思路

  遍历链表，找到链表的最后两个元素，对应pre和cur，删掉cur (也就是把prev.next = null)，然后把cur粘到tail上。再次遍历链表，找到最后两个元素，删掉最后一个元素，再把最后一个元素粘到tail上...直到最后只剩两个元素head和head.next。先把head.next粘到tail上，最后再把head粘到tail上。

  * 我们要return什么？

    题目要求我们return反转后的链表的头，也就是说我在**第一次**粘tail的时候，要记录下来这个头，最后return它。

  * 怎么找倒数第二个元素？

    while (prev.next.next == null)

  * 整个循环要怎么写？

    首先，我外层的循环是每次都要从第一个元素开始，找到倒数的两个元素，所以我的p1和p2都是置为head，保证每次都从第一个元素开始遍历。

    其次，内层的循环是为了找倒数两个元素，所以我里面用了p2 = p2.next，直到p2.next.next == null，是为倒数第二个元素，跳出循环。

  * 注意会有什么bug?

    剩下head和head.next两个元素的时候，先使得tail.next = head.next，把head.next这个元素沾到tail上。然后tail = tail.next， 再把head这个元素沾到taill上，也就是tail.next = head。这时，注意head是仍然指向head.next的，需要把head.next 置为null。

  * 变量起名

    变量起名要尽量简短且有意义，如pre, cur, head, tail, result, left, right ...

  * dummyNode

    因为null是不能有next的，所以我们采用了dummynode。

    想象一下，如果tail最开始是null的时候，我们还需要去判断一下，如果tail == null，那么我们就把值直接赋给tail；如果不是null，那么就是粘上去，也就是tail.next = node。

    如果我们用了dummyNode，那么就可以统一这种写法，全部统一为tail.next = node

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        if (head.next.next == null) {
            ListNode node = head.next;
            head.next = null;
            node.next = head;
            return node;
        }
        ListNode p1 = head;
        ListNode p2 = head;
        ListNode cur = head.next;
        ListNode dummyNode = new ListNode(0);
        ListNode tail = dummyNode;
        while (p1.next.next != null) {
            while (p2.next.next != null) {
                p2 = p2.next;
            }
            cur = p2.next;
            p2.next = null;
            tail.next = cur;
            tail = tail.next;
            p1 = head;
            p2 = head;
        }
        tail.next = head.next;
        tail = tail.next;
        tail.next = head;
        head.next = null;
        return dummyNode.next;
    }
}
```

#### reversed linked list聪明的解法

* 思路

  ​             1 --> 2 --> 3 --> null

  null <-- 1 <-- 2 <-- 3

  第一次，prev为null，cur为head，先记录下来下一个cur是谁(nextTemp = cur.next)，然后转换方向，cur.next = prev，然后prev = cur, cur = nextTemp，继续循环...直到cur == null，停止循环。

  ```java
  /**
   * Definition for singly-linked list.
   * public class ListNode {
   *     int val;
   *     ListNode next;
   *     ListNode(int x) { val = x; }
   * }
   */
  class Solution {
      public ListNode reverseList(ListNode head) {
          ListNode prev = null;
          ListNode cur = head;
          while (cur != null) {
              ListNode nextTemp = cur.next;
              cur.next = prev;
              prev = cur;
              cur = nextTemp;
          }
          return prev;
      }
  }
  ```
