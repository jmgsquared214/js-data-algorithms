Linked Lists
 
A linked list is a data structure in which each node points to another node. Unlike arrays, which have a fixed size, a linked list is a dynamic data structure that can allocate and deallocate memory at runtime. By the end of this chapter, you will understand how to implement and work with linked lists.
There are two types of linked lists discussed in this chapter: singly and doubly linked lists. Let’s examine the singly linked list first.
Singly Linked Lists
The linked list data structure is one where each node (element) has reference to the next node (see Figure 13-1).

Figure 13-1Singly linked list
A node in a singly linked list has the following properties: data and next. data is the value for the linked list node, and next is a pointer to another instance of SinglyLinkedListNode.
1   function SinglyLinkedListNode(data) {
2       this.data = data;
3       this.next = null;
4   }
The following code is the base for the singly linked list example. You can find the code on GitHub.1 The code block has a helper function to check whether the singly linked list is empty.
1   function SinglyLinkedList(){
2           this.head = null;
3           this.size = 0;
4   }
5
6   SinglyLinkedList.prototype.isEmpty = function(){
7           return this.size == 0;
8   }
The start of the linked list is referred to as the head . This property defaults to null before inserting any element into the linked list.
INSERTION
The following code block shows how to insert into a singly linked list. If the head of the linked list is empty, the head is set to the new node. Otherwise, the old heap is saved in temp, and the new head becomes the newly added node. Finally, the new head’s next points to the temp (the old head).
 1   SinglyLinkedList.prototype.insert = function(value) {
 2       if (this.head === null) { //If first node
 3           this.head = new SinglyLinkedListNode(value);
 4       } else {
 5           var temp = this.head;
 6           this.head = new SinglyLinkedListNode(value);
 7           this.head.next = temp;
 8       }
 9       this.size++;
10   }
11   var sll1 = new SinglyLinkedList();
12   sll1.insert(1); // linked list is now: 1 -> null
13   sll1.insert(12); // linked list is now: 12 -> 1 -> null
14   sll1.insert(20); // linked list is now: 20 -> 12 -> 1 -> null
Time Complexity: O(1)
This is a constant time operation; no loops or traversal is required.
DELETION BY VALUE
The deletion of a node in a singly linked list is implemented by removing the reference of that node. If the node is in the “middle” of the linked list, this is achieved by having the node with the next pointer to that node point to that node’s own next node instead, as shown in Figure 13-2.

Figure 13-2Interior node removal from a singly linked list
If the node is at the end of the linked list, then the second-to-last element can dereference the node by setting its next to null.
 1   SinglyLinkedList.prototype.remove = function(value) {
 2       var currentHead = this.head;
 3       if (currentHead.data == value) {
 4           // just shift the head over. Head is now this new value
 5           this.head = currentHead.next;
 6           this.size--;
 7       } else {
 8           var prev = currentHead;
 9           while (currentHead.next) {
10               if (currentHead.data == value) {
11                   // remove by skipping
12                   prev.next = currentHead.next;
13                   prev = currentHead;
14                   currentHead = currentHead.next;
15                   break; // break out of the loop
16               }
17               prev = currentHead;
18               currentHead = currentHead.next;
19           }
20           //if wasn't found in the middle or head, must be tail
21           if (currentHead.data == value) {
22               prev.next = null;
23           }
24           this.size--;
25       }
26   }
27   var sll1 = new SinglyLinkedList();
28   sll1.insert(1); // linked list is now:  1 -> null
29   sll1.insert(12); // linked list is now: 12 -> 1 -> null
30   sll1.insert(20); // linked list is now: 20 -> 12 -> 1 -> null
31   sll1.remove(12); // linked list is now: 20 -> 1 -> null
32   sll1.remove(20); // linked list is now: 1 -> null
Time Complexity: O(n)
In the worst case, the entire linked list must be traversed.
DELETION AT THE HEAD
Deleting an element at the head of the linked list is possible in O(1). When a node is deleted from the head, no traversal is required. The implementation of this deletion is shown in the following code block. This allows the linked list to implement a stack. The last-added item (to the head) can be removed in O(1).
 1   DoublyLinkedList.prototype.deleteAtHead = function() {
 2       var toReturn = null;
 3
 4       if (this.head !== null) {
 5           toReturn = this.head.data;
 6
 7           if (this.tail === this.head) {
 8               this.head = null;
 9               this.tail = null;
10           } else {
11               this.head = this.head.next;
12               this.head.prev = null;
13           }
14       }
15       this.size--;
16       return toReturn;
17   }
18   var sll1 = new SinglyLinkedList();
19   sll1.insert(1); // linked list is now:  1 -> null
20   sll1.insert(12); // linked list is now: 12 -> 1 -> null
21   sll1.insert(20); // linked list is now: 20 -> 12 -> 1 -> null
22   sll1.deleteAtHead(); // linked list is now:  12 -> 1 -> null
SEARCH
To find out whether a value exists in a singly linked list, simple iteration through all its next pointers is needed.
 1   SinglyLinkedList.prototype.find = function(value) {
 2       var currentHead = this.head;
 3       while (currentHead.next) {
 4           if (currentHead.data == value) {
 5               return true;
 6           }
 7           currentHead = currentHead.next;
 8       }
 9       return false;
10   }
Time Complexity: O(n)
Like with the deletion operation, in the worst case, the entire linked list must be traversed.
Doubly Linked Lists
A doubly linked list can be thought of as a bidirectional singly linked list. Each node in the doubly linked list has both a next pointer and a prev pointer. The following code block implements the doubly linked list node:
1   function DoublyLinkedListNode(data) {
2       this.data = data;
3       this.next = null;
4       this.prev = null;
5   }
In addition, a doubly linked list has a head pointer as well as a tail pointer. The head refers to the beginning of the doubly linked list, and the tail refers to the end of the doubly linked list. This is implemented in the following code along with a helper function to check whether the doubly linked list is empty:
1   function DoublyLinkedList (){
2           this.head = null;
3           this.tail = null;
4           this.size = 0;
5  }
6   DoublyLinkedList.prototype.isEmpty = function(){
7           return this.size == 0;
8   }
Each node in a doubly linked list has next and prev properties. Deletion, insertion, and search implementations in a doubly linked list are similar to that of the singly linked list. However, for both insertion and deletion, both next and prev properties must be updated. Figure 13-3 shows an example of a doubly linked list.

Figure 13-3Doubly linked list example with five nodes
INSERTION AT THE HEAD
Inserting into the head of the doubly linked list is the same as the insertion for the singly linked list except that it has to update the prev pointer as well. The following code block shows how to insert into the doubly linked list. If the head of the linked list is empty, the head and the tail are set to the new node. This is because when there is only one element, that element is both the head and the tail. Otherwise, the temp variable is used to store the new node. The new node’s next points to the current head, and then the current head’s prev points to the new node. Finally, the head pointer is updated to the new node.
 1   DoublyLinkedList.prototype.addAtFront = function(value) {
 2       if (this.head === null) { //If first node
 3           this.head = new DoublyLinkedListNode(value);
 4           this.tail = this.head;
 5       } else {
 7           var temp = new DoublyLinkedListNode(value);
 8           temp.next = this.head;
 9           this.head.prev = temp;
10           this.head = temp;
11       }
12       this.size++;
13   }
14   var dll1 = new DoublyLinkedList();
15   dll1.insertAtHead(10); // ddl1's structure: tail: 10  head: 10
16   dll1.insertAtHead(12); // ddl1's structure: tail: 10  head: 12
17   dll1.insertAtHead(20); // ddl1's structure: tail: 10  head: 20
Time Complexity: O(1)
INSERTION AT THE TAIL
Similarly, a new node can be added to the tail of a doubly linked list, as implemented in the following code block:
 1   DoublyLinkedList.prototype.insertAtTail = function(value) {
 2       if (this.tail === null) { //If first node
 3           this.tail = new DoublyLinkedListNode(value);
 4           this.head = this.tail;
 5       } else {
 6           var temp = new DoublyLinkedListNode(value);
 7           temp.prev = this.tail;
 8           this.tail.next = temp;
 9           this.tail = temp;
10       }
11       this.size++;
12   }
13
14   var dll1 = new DoublyLinkedList();
15   dll1.insertAtHead(10); // ddl1's structure: tail: 10  head: 10
16   dll1.insertAtHead(12); // ddl1's structure: tail: 10  head: 12
17   dll1.insertAtHead(20); // ddl1's structure: tail: 10  head: 20
18   dll1.insertAtTail(30); // ddl1's structure: tail: 30  head: 20
Time Complexity: O(1)
DELETION AT THE HEAD
Removing a node at the head from a doubly linked list can be done in O(1) time. If there is only one item in the case that the head and the tail are the same, both the head and the tail are set to null. Otherwise, the head is set to the head’s next pointer. Finally, the new head’s prev is set to null to remove the reference of the old head. This is implemented in the following code block. This is great because it can be used like a dequeue function from the queue data structure.
 1   DoublyLinkedList.prototype.deleteAtHead = function() {
 2       var toReturn = null;
 3
 4       if (this.head !== null) {
 5           toReturn = this.head.data;
 6
 7           if (this.tail === this.head) {
 8               this.head = null;
 9               this.tail = null;
10           } else {
11               this.head = this.head.next;
12               this.head.prev = null;
13           }
14       }
15       this.size--;
16       return toReturn;
17   }
Time Complexity: O(1)
DELETION AT THE TAIL
Similarly to removing the node at the head, the tail node can be removed and returned in O(1) time, as shown in the following code block. By having the ability to remove at the tail as well, the doubly linked list can also be thought of as a bidirectional queue data structure. A queue can dequeue the first-added item, but a doubly linked list can dequeue either the item at the tail or the item at the head in O(1) time.
 1   DoublyLinkedList.prototype.deleteAtTail = function() {
 2       var toReturn = null;
 3
 4       if (this.tail !== null) {
 5           toReturn = this.tail.data;
 6
 7           if (this.tail === this.head) {
 8               this.head = null;
 9               this.tail = null;
10           } else {
11               this.tail = this.tail.prev;
12               this.tail.next = null;
13           }
14       }
15       this.size--;
16       return toReturn;
17   }
18   var dll1 = new DoublyLinkedList();
19   dll1.insertAtHead(10); // ddl1's structure: tail: 10  head: 10
20   dll1.insertAtHead(12); // ddl1's structure: tail: 10  head: 12
21   dll1.insertAtHead(20); // ddl1's structure: tail: 10  head: 20
22   dll1.insertAtTail(30); // ddl1's structure: tail: 30  head: 20
23   dll1.deleteAtTail();
24   // ddl1's structure: tail: 10  head: 20
Time Complexity: O(1)
SEARCH
To find out whether a value exists in a doubly linked list, you can start at the head and use the next pointer or start at the tail and use the prev pointer. The following code block is the same implementation as the singly linked list search implementation, which starts at the head and looks for the item:
 1   DoublyLinkedList.prototype.findStartingHead = function(value) {
 2       var currentHead = this.head;
 3       while(currentHead.next){
 4           if(currentHead.data == value){
 5               return true;
 6           }
 7           currentHead = currentHead.next;
 8       }
 9       return false;
10   }
11   var dll1 = new DoublyLinkedList();
12   dll1.insertAtHead(10); // ddl1's structure: tail: 10  head: 10
13   dll1.insertAtHead(12); // ddl1's structure: tail: 10  head: 12
14   dll1.insertAtHead(20); // ddl1's structure: tail: 10  head: 20
15   dll1.insertAtTail(30); // ddl1's structure: tail: 30  head: 20
16   dll1.findStartingHead(10); // true
17   dll1.findStartingHead(100); // false
Time Complexity: O(n)
The following code traverses the doubly linked list starting with the tail using prev pointers:
 1   DoublyLinkedList.prototype.findStartingTail = function(value) {
 2       var currentTail = this.tail;
 3       while (currentTail.prev){
 4           if(currentTail.data == value){
 5               return true;
 6           }
 7           currentTail = currentTail.prev;
 8       }
 9       return false;
10   }
11
12   var dll1 = new DoublyLinkedList();
13   dll1.insertAtHead(10); // ddl1's structure: tail: 10  head: 10
14   dll1.insertAtHead(12); // ddl1's structure: tail: 10  head: 12
15   dll1.insertAtHead(20); // ddl1's structure: tail: 10  head: 20
16   dll1.insertAtTail(30); // ddl1's structure: tail: 30  head: 20
17   dll1.findStartingTail(10); // true
18   dll1.findStartingTail(100); // false
Time Complexity: O(n)
Although the time complexity for search is the same as the singly linked list’s search, only the doubly linked list can search bidirectionally (using prev or next). This means that if given a reference to a doubly linked list node, doubly linked lists can perform a full search, but a singly linked list is limited to only its next pointers .
Summary
The linked list data structure works by each node having a next pointer (and previous, or prev, pointer if doubly linked) to a different node. Insertion for both singly and doubly linked lists has a constant time complexity of O(1). The time complexity of deleting from the head of the singly and doubly linked lists is O(1) as well. However, searching for an item in both singly and doubly linked list takes O(n) time. Doubly linked lists should be used over singly linked lists when bidirectional traversal/search is required. Furthermore, doubly linked lists allow you to pop from either the tail or the head of the linked list for a flexible and fast O(1) operation.
Exercises
You can find all the code for the exercises on GitHub.2
REVERSE A SINGLY LINKED LIST
To reverse a singly linked list, simply iterate through each node and set the next property on the current node to the previous node.
 1   function reverseSingleLinkedList(sll){
 2           var node = sll.head;
 3           var prev = null;
 4           while(node){
 5                   var temp = node.next;
 6                   node.next = prev;
 7                   prev = node;
 8                   if(!temp)
 9                           break;
10                   node = temp;
11           }
12           return node;
13   }
Time Complexity: O(n)
Space Complexity: O(1)
To fully reverse a linked list, the entire N elements of the linked list must be traversed.
DELETE DUPLICATES IN A LINKED LIST
Deleting an item in a linked list is simple. Simply iterate and store visited nodes inside an array. Delete the current element if the current element has already been seen previously.
 1   // delete duplicates in unsorted linkedlist
 2   function deleteDuplicateInUnsortedSll(sll1) {
 3       var track = [];
 4
 5       var temp = sll1.head;
 6       var prev = null;
 7       while (temp) {
 8           if (track.indexOf(temp.data) >= 0) {
 9               prev.next = temp.next;
10              sll1.size--;
11           } else {
12               track.push(temp.data);
13               prev = temp;
14           }
15           temp = temp.next;
16       }
17       console.log(temp);
18   }
Time Complexity: O(n2)
Space Complexity: O(n)
However, this algorithm must iterate over the array with the .indexOf() method, which is O(n) as well as iterating n times. Hence, it is O(n2) in time complexity. In addition, the track array grows to size of N, and this causes the space complexity to be O(n). Let’s cut the time complexity down to O(n).
 1   //delete duplicates in unsorted linkedlist
 2   function deleteDuplicateInUnsortedSllBest(sll1) {
 3       var track = {};
 4
 5       var temp = sll1.head;
 6       var prev = null;
 7       while (temp) {
 8           if (track[temp.data]) {
 9               prev.next = temp.next;
10              sll1.size--;
11           } else {
12               track[temp.data] = true;
13               prev = temp;
14           }
15           temp = temp.next;
16       }
17       console.log(temp);
18   }
Time Complexity: O(n)
Space Complexity: O(n)
Use of the JavaScript Object as a hash table to store and check for seen elements cuts it down to O(n) but O(n) in space as extra memory is required for the hash table.

