# Data structures and Algorithms with JavaScript

## Table of Contents
* Data Strucrures
    * Linked List
        * [Singly Linked List](#Singly-Linked-List)
        * [Doubly Linked List](#Doubly-Linked-List)
    * Tree
        * [Binary Tree](#Binary-Tree)
            * [Insert](#BST-Insert)
            * Traverse
                * [BFS]()
                * [DFS]()
                    * [PreOrder]()
                    * [PostOrder]()
                    * [InOrder]()
        
    * Stack
    * Queue
        * [Regular Queue]()
        * [Circular Queue]()
        * [Priority Queue]()
    * HashTable
    

* Algorithms
    * Recursion
        * Sum
        * Fibonacci
    * Search
        * Linear Search
        * Binary Search
    * Sort
        * Bubble Sort
        * Insertion Sort
        * Selection Sort
        * Merge Sort
        * Quick Sort
        * Heap Sort

---

## Linked List
Linked List(鏈結串列)，是一種透過指標指向節點的一種資料結構

**Time complexity:**

Write **O(1)**
Read **O(n)**
Delete **O(1)**

### Singly Linked List
***Methods:***
**append** - 新增節點至最後一個元素
**insertAfterNode** - 插入節點至目標後面
**traverse** - 遍歷
**getNode** - 取得目標節點
**getLength** - 回傳當前串列長度
```javascript=
//define node for pointer
class NodeStructure {
  data = null;
  next = null;
  constructor(data) {
    this.data = data;
  }
}

// main class
class LinkedList {
  // #is private modifier
  #head = null;
  #length = 0;

  append(data) {
    const node = new NodeStructure(data);
    if (this.#head == null) {
      this.#head = node;
      this.#length++;
    } else {
      let pointer = this.#head;
      while (pointer.next != null) {
        pointer = pointer.next;
      }
      pointer.next = node;
      this.#length++;
    }
  }

  traverse() {
    if (this.#length == 0) {
      return;
    }
    if (this.#length == 1) {
      console.log(this.#head);
      return;
    }
    if (this.#length > 1) {
      let pointer = this.#head;
      console.log(pointer.data);
      while (pointer.next != null) {
        pointer = pointer.next;
        console.log(pointer.data);
      }
    }
  }

  getLength() {
    return this.#length;
  }

  getNode(data) {
    let pointer = this.#head;
    if (pointer.data == data) {
      return pointer;
    } else {
      while (pointer.next != null) {
        pointer = pointer.next;
        if (pointer.data == data) {
          break;
        }
      }
      return pointer;
    }
  }
  insertAfterNode(node, data) {
    const nextTempNode = node.next;
    node.next = new NodeStructure(data);
    node.next.next = nextTempNode;
    this.#length++;
    this.traverse();
  }
}

const linkedList = new LinkedList();
linkedList.append("Dennis");
linkedList.append("Ivy");
linkedList.append("Allen");

const targetNode = linkedList.getNode("Ivy");
linkedList.insertAfterNode(targetNode, "Josh");
//Dennis Ivy Josh Allen
```
> updated on Oct,08,2023
### Doubly Linked List
```javascript=
class NodeStructure {
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}

//main
class DoublyLinkedList {
  #head = null;
  #length = 0;
  push(value) {
    const node = new NodeStructure(value);
    if (this.#length == 0) {
      this.#head = node;
      this.#length++;
    } else {
      let pointer = this.#head;
      while (pointer.next != null) {
        pointer = pointer.next;
      }
      pointer.next = node;
      node.prev = pointer;
      this.#length++;
    }
  }

  traverse() {
    if (this.#length == 0) {
      return "Sorry list is empty";
    } else {
      let pointer = this.#head;
      console.log(pointer);
      while (pointer.next != null) {
        pointer = pointer.next;
        // console.log(pointer.prev);
        console.log(pointer);
      }
    }
  }

  getNode(value) {
    let i = 1;
    let pointer = this.#head;
    while (i < this.#length) {
      if (pointer.data == value) {
        break;
      }
      pointer = pointer.next;
      i++;
    }
    return pointer;
  }

  getLength() {
    return this.#length;
  }
}

const doublyLinkedList = new DoublyLinkedList();
doublyLinkedList.push(2);
doublyLinkedList.push(4);
doublyLinkedList.push(6);

const length = doublyLinkedList.getLength();
console.log(length); //3
doublyLinkedList.traverse();
const target = doublyLinkedList.getNode(4);
console.log(target);
{
  /* <ref *1> NodeStructure {
  data: 2,
  prev: null,
  next: <ref *2> NodeStructure {
    data: 4,
    prev: [Circular *1],
    next: NodeStructure { data: 6, prev: [Circular *2], next: null }
  }
}
<ref *1> NodeStructure {
  data: 4,
  prev: NodeStructure { data: 2, prev: null, next: [Circular *1] },
  next: NodeStructure { data: 6, prev: [Circular *1], next: null }
}
<ref *2> NodeStructure {
  data: 6,
  prev: <ref *1> NodeStructure {
    data: 4,
    prev: NodeStructure { data: 2, prev: null, next: [Circular *1] },
    next: [Circular *2]
  },
  next: null
} */
}

```
> updated on Oct,09,2023


---

## Tree
Tree of data structures is according tree's structure to simulate leaf and branche。


1. Folder
2. BST
3. HTML DOM

When root haven't child, we can call that is leaf.

### Binary Tree
Binary search tree also called BST.

rule:

Each node to the left of root must less than the root.On the other hand, each node to right of root must greater than the root.


For example:
```bash=
            10            root
          /    \
         8      15        layer 1
        / \       \
       3   6       20     layer 2
```

Above graphic have a problem on layer 2.

```bash=
            10            root
          /    \
         8      15        layer 1
        /        \
       6          20      layer 2
      /
     3                    layer 3
```

#### BST-Insert
Below example, insert 10、20、20、5 into tree.

```javascript=
class NodeStructure {
  constructor(value) {
    this.data = value;
    this.right = null;
    this.left = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value) {
    const node = new NodeStructure(value);
    if (this.root == null) {
      this.root = node;
      return;
    } else {
      let pointer = this.root;

      while (true) {
        if (node.data > pointer.data) {
          if (pointer.right != null) {
            pointer = pointer.right;
          } else {
            pointer.right = node;
            break;
          }
        } else if (node.data < pointer.data) {
          if (pointer.left != null) {
            pointer = pointer.left;
          } else {
            pointer.left = node;
            break;
          }
        } else {
          // node.data exist in tree
          console.log("sorry can't insert this value");
          break;
        }
      }
    }
  }
}
const tree = new BinarySearchTree();
tree.insert(10);
tree.insert(20);
tree.insert(20); //sorry can't insert this value
tree.insert(5);

console.log(tree);
// BinarySearchTree {
//   root: NodeStructure {
//     data: 10,
//     right: NodeStructure { data: 20, right: null, left: null },
//     left: NodeStructure { data: 5, right: null, left: null }
//   }
// }
```
>updated on Oct,10,2023

#### BST-Traverse

