## 什么是算法和数据结构

* 数据结构：计算机中存储和组织数据的方式

* 算法：解决办法的逻辑/操作

## 数组

JS数组就是API的调用

## 栈

1. 栈是受限的线性结构：（生活中类似于自助餐的托盘）

* **只能在一端添加/删除元素**（栈顶）
* 进入：进栈（压栈）
* 出去：出栈（退 栈）

2. 函数调用栈

   * A调用B，B调C，C调D
   * D,C,B,A的弹栈顺序

3. 一个栈结构面试题

   * 有6个元素 6，5，4，3，2，1的顺序进栈（要考虑到可以一边入栈一边出栈）

     * 3，4，6，5，2，1不合法

   4.使用数组实现栈

```javascript
    // Method：和某一个对象实例有关系
    // function
    // 封装栈类

    // 1.封装栈类
    function Stack() {
      // 栈中的属性
      this.items = []
        // 栈的相关操作
        // 1.将元素压入栈
        // function push(this){

      // } 这种方法添加方法不好，因为这样每一个new出来的对象都会有一个push实例，占用内存

      Stack.prototype.push = function(element) {
          return this.items.push(element)
        }
        // 2.从栈顶中取出元素
      Stack.prototype.pop = function() {
          return this.items.pop()
        }
        // 3.返回栈顶元素，仅仅返回
      Stack.prototype.peek = function() {
          return this.items[this.items.length - 1]
        }
        // 4.判断栈是否为空
      Stack.prototype.isEmpty = function() {
          return this.items.length == 0
        }
        // 5.返回栈里的元素个数
      Stack.prototype.size = function() {
          return this.items.length
        }
        // 6.将栈结构的内容以字符串形式返回
      Stack.prototype.toString = function() {
        var resultString = ''
        for (var i = 0; i < this.items.length; i++) {
          resultString += this.items[i] + ''
        }
        return resultString
      }
    }

    // 栈的使用
    var s = new Stack()
    s.push(208) //压栈
    s.push(10)
    s.push(7)
    s.push(4)
    s.push(100)
    s.push(13)
    console.log(s);

    s.pop() //弹栈
    console.log(s);
    console.log(s.peek()) //返回栈顶元素，仅仅返回
    console.log(s.isEmpty()) //判断栈是否为空
    console.log(s.size()); //返回栈里的元素个数
    console.log(s.toString()); //将栈结构的内容以字符串形式返回
```



5. 十进制转二进制

    利用栈结构来实现

   ```javascript
    // 函数：将十进制转为二进制
       function dec2bin(decNumber) {
         // 1.定义栈对象
         var stack = new Stack()
   
         // 2.循环操作
         while (decNumber > 0) {
           // 2.1获取余数并放到栈中
           stack.push(decNumber % 2)
   
           // 2.2获取整除后的结果作为下一次运行的数字
           decNumber = Math.floor(decNumber / 2)
         }
         // 3.从栈中取出0和1
         var binaryString = ''
         while (!stack.isEmpty()) {
           binaryString += stack.pop()
         }
         return binaryString
       }
   ```



## 队列

1. 一种受限的线性表，**先进先出**

* 类似于生活中的电影影院，厕所排队，商场
* 优先排队的人优先处理

2. 基于数组实现队列**,基于数组实现性能不高**(因为删除，从前面删除后，每个元素要往前移动)

```javascript
    // 基于数组实现性能不高(因为删除，从前面删除后，每个元素要往前移动)
    // 封装队列类
    function Queue() {
      // 属性
      this.items = []
        // 方法
        // 1.将元素加入到队列
      Queue.prototype.enqueue = function(element) {
          return this.items.push(element)
        }
        // 2.从队列中删除前端元素
      Queue.prototype.dequeue = function() {
          return this.items.shift()
        }
        // 3.查看前端元素
      Queue.prototype.front = function() {
          return this.items[0]
        }
        // 4.查看队列是否为空
      Queue.prototype.isEmpty = function() {
          return this.items.length == 0
        }
        // 5.查看队列中元素的个数
      Queue.prototype.size = function() {
          return this.items.length
        }
        // 6.toString方法
      Queue.prototype.toString = function() {
        var resultString = ''
        for (var i = 0; i < this.items.length; i++) {
          resultString += this.items[i] + ''
        }
        return resultString
      }
    }

    // 使用队列
    var queue = new Queue()
    queue.enqueue('abc')
    queue.enqueue('cba')
    queue.enqueue('nba')
    queue.enqueue('mba')
    alert(queue)

    queue.dequeue()
    alert(queue)

    alert(queue.front())
    alert(queue.isEmpty())
    alert(queue.size())
    alert(queue.toString())
```

3. 击鼓传花面试算法题

   规则

   * 几个朋友一起玩一个游戏，围城一圈，开始叔叔，数到某个数字的人自动出局
   * 最后剩下的人会胜利，请问最后剩下的是在**哪一个位置**的人

```javascript
    function passGame(nameList, num) {
      // 1.创建一个队列
      var queue = new Queue()
        // 2.将所有人依次加入到队列中
      for (var i = 0; i < nameList.length; i++) {
        queue.enqueue(nameList[i])
      }
      // 3. 开始数数字
      while (queue.size() > 1) {
        // 不是num的时候，重新加入到队列的末尾
        // 是num的时候，将其从队列中删除
        // 3.1num数字之前的人重新放入到队列的末尾
        for (var i = 0; i < num - 1; i++) {
          queue.enqueue(queue.dequeue())
        }
        // 3.2num对应的人删除掉
        queue.dequeue()
      }
      var endName = queue.front()
      return nameList.indexOf(endName)
    }

    nameList = ['Lily', 'Tom', 'LiLei', 'carri', 'taozi']

    console.log(passGame(nameList, 3));
```

4. 优先级队列

* 如头等舱和商务舱乘客的优先级高于经济舱
* 医院会优先处理病情比较严重的患者
* 每个元素不仅包含元素还包含优先级

```javascript
// 只有插入方法和数组不同
    // 如头等舱和商务舱乘客的优先级高于经济舱
    // 医院会优先处理病情比较严重的患者

   function PriorityQueue() {
      // 在PriorityQueue重新创建了一个类，可以理解内部类
      function QueueElement(element, priority) {
        this.element = element
        this.priority = priority
      }

      // 封装属性
      this.items = []

      //实现插入方法
      PriorityQueue.prototype.enqueue = function(element, priority) {
          // 1.创建QueueElement对象
          var queueElement = new QueueElement(element, priority)

          // 2.判断队列是否为空
          if (this.items.length === 0) {
            this.items.push(queueElement)
          } else {
            var added = false
            for (var i = 0; i < this.items.length; i++) {
              if (queueElement.priority < this.items[i].priority) {
                this.items.splice(i, 0, queueElement)
                added = true
                break
              }
            }
            if (!added) {
              this.items.push(queueElement)
            }
          }
        }
   }
```



## 链表

1. 数组和链表优缺点

* 链表和数组一样，可以存储一系列的元素，但是链表和数组的实现机制完全不同
* 数组：
  * 数组创建通常需要一段**连续的内存空间**（一整块内存），并且**大小是固定的**（大多数编程语言是固定的），所以当当前数组不能满足容量需求时，需要**扩容**（一般情况下申请一个更大的数组，把原来的数组复制过去）
  * 而且数组的开头或中间位置**插入数据成本很高**，需要进行大量的元素位移
  * 尽管我们已经学过JavaScript的Array类方法可以帮我们做这些事，但背后的原理依然是这样

* 链表
  * 不同于数组，链表中的元素在内存中**不必是连续的空间**
  * 链表的每个元素有一个**存储元素本身的节点**和一个指向**下一个元素的引用**
  * 链表不必在创建的时候就**确定大小**
  * 链表在**插入和删除数据时**，时间复杂度可以达到O(1),相对数组效率高很多
  * 缺点：链表访问任何一个位置的元素的时候，都需要**从头开始访问**（无法跳过第一个元素访问任何一个元素），无法通过下标直接访问元素，需要从头一个个访问，直到找到对应的元素

2. 链表到底是什么
   * 类似火车，一节车厢连着下一节，车厢上有乘客（数据），火车头指向第一个车厢

* 类似火车，一节车厢连着下一节，车厢上有乘客（数据）

3. 封装单向链表

```javascript
 // 封装链表类
    function LinkedList() {
      // 内部的类：节点类
      function Node(data, next) {
        this.data = data
        this.next = null
      }

      // 属性
      this.head = null
      this.length = 0

      // 1.追加方法
      LinkedList.prototype.append = function(data) {
        // 1.创建新节点
        var newNode = new Node(data)

        // 2.判断是否添加的是不是第一个节点
        if (this.length == 0) { //是第一个节点
          this.head = newNode
        } else { // 不是第一个节点
          var current = this.head
          while (current.next) {
            current = current.next
          }

          // 最后节点的next指向新的节点
          current.next = newNode
        }

        // 3.length加1
        this.length += 1
      }

      // 2.toString()方法
      LinkedList.prototype.toString = function() {
        // 1.定义变量
        var current = this.head
        var listString = ''

        // 2.循环获取一个个节点
        while (current) {
          listString += current.data + " "
          current = current.next
        }

        return listString
      }

      // 3.insert方法
      LinkedList.prototype.insert = function(position, data) {
        // 1.对position进行越界判断
        if (position < 0 || position > this.length) return false

        // 2.根据data创建newNode
        var newNode = new Node(data)

        // 3.判断插入的位置是否是第一个
        if (position == 0) {
          newNode.next = this.head
          this.head = newNode
        } else {
          var index = 0
          var current = this.head
          var previous = null
          while (index++ < position) {
            previous = current
            current = current.next
          }

          newNode.next = current
          previous.next = newNode
        }
        // 4.length+1
        this.length += 1

        return true
      }

      // 4.get方法
      LinkedList.prototype.get = function(position) {
        // 1.对position进行越界判断
        if (position < 0 || position >= this.length) return null

        // 2.获取对应的data
        var current = this.head
        var index = 0
        while (index++ < position) {
          current = current.next
        }

        return current.data
      }

      // 5.indexOf(element)方法
      LinkedList.prototype.indexOf = function(element) {
        // 1.定义变量
        var current = this.head
        var index = 0

        // 2.开始查找
        while (current) {
          if (current.data === element) {
            return index
          }
          current = current.next
          index += 1
        }

        // 3.找到最后没找到，返回-1
        return -1
      }

      // 6.update方法
      LinkedList.prototype.update = function(position, newData) {
        // 1.越界判断
        if (position < 1 || position >= this.length) return false

        // 2.查找正确的节点
        var current = this.head
        var index = 0
        while (index++ < position) {
          current = current.next
        }

        // 3.将position位置的node的data修改成newData
        current.data = newData

        return true
      }

      // 7.removeAt方法
      LinkedList.prototype.removeAt = function(position) {
        // 1.越界判断
        var current = this.head
        if (position < 0 || position >= this.length) return null

        // 2.判断是否删除的是第一个节点
        if (position == 0) {
          this.head = this.head.next
        } else {
          var index = 0
          var previous = null
          while (index++ < position) {
            previous = current
            current = current.next
          }
          // 前一个节点的next指向current.next即可
          previous.next = current.next
        }

        // 3.length-1
        this.length -= 1

        return current.data
      }

      // 8.remove方法
      LinkedList.prototype.remove = function(data) {
        // 1.获取data在列表中的位置
        var position = this.indexOf(data)

        // 2.根据位置信息，删除节点
        return this.removeAt(position)
      }

      // 9.isEmpty方法
      LinkedList.prototype.isEmpty = function() {
        return this.length === 0
      }

      // 10.size方法
      LinkedList.prototype.size = function() {
        return this.length
      }
    }

    // 测试代码
    // 1.创建LinkedList
    var list = new LinkedList()

    // 2.测试append方法
    list.append('abc')
    list.append('cba')
    list.append('mba')
    console.log(list.toString())

    // 3.测试insert
    list.insert(0, 'aaa')
    list.insert(3, 'bbb')
    list.insert(5, 'ccc')
      // 4.测试toString
    console.log(list.toString());
    // 5.测试get
    console.log(list.get(3));
    console.log(list.get(5));
    console.log(list.get(6));
    console.log(list.indexOf('ccc'));

    // 6.测试update
    console.log(list.toString());
    list.update(2, 'ooo')
    console.log(list.toString());

    // 7.测试removeAt方法
    console.log(list.removeAt(0));
    list.removeAt(3)
    console.log(list.toString());
    // 8.测试remove方法
    list.remove('ooo')
    console.log(list.toString());
    // 9.测试size方法和isEmpty
    console.log(list.isEmpty());
    console.log(list.size());
```

4. 封装双向链表

* 单向链表
  * 只能**从头遍历到尾或者从尾遍历到头**（一般从头到尾）
  * 也就是链表相连的过程是**单向**的
  * 实现的原理就是上一个链表中有一个指向下一个的**引用**
  * 缺点：不能**回到上一个节点**

* 双向链表：
  * 既可以从头遍历到尾，又可以从尾遍历到头
  * 也就是链表相连的过程是**双向的**
  * 一个节点既有**向前连接的引用**，也有一个**向后连接的引用**
  * 缺点：插入或者删除时，需要处理四个引用，占用内存稍微大一些

```javascript
    // 封装双向链表
    function DoublyLinkedList() {
      // 内部类
      function Node(data) {
        this.data = data
        this.prev = null
        this.next = null
      }

      // 属性
      this.head = null
      this.tail = null
      this.length = 0

      // 常见操作：方法
      // 1.append 方法
      DoublyLinkedList.prototype.append = function(data) {
        // 1.根据data创建节点
        var newNode = new Node(data)

        // 2.判断是否是第一个节点
        if (this.length == 0) {
          this.head = newNode
          this.tail = newNode
        } else {
          newNode.prev = this.tail
          this.tail.next = newNode
          this.tail = newNode
        }

        // 3.length+1
        this.length += 1
      }

      // 2.将链表转成字符串形式
      // 2.1 toString方法
      DoublyLinkedList.prototype.toString = function() {
          // 直接返回backwardString()
          return this.backwardString()
        }
        // 2.2 forwardString方法
      DoublyLinkedList.prototype.forwardString = function() {
          // 1.定义变量
          var current = this.tail
          var resultString = ''
            // 2.依次向后遍历，获取每一个节点
          while (current) {
            resultString += current.data + ' '
            current = current.prev
          }
          return resultString
        }
        // 2.3 backwardString方法
      DoublyLinkedList.prototype.backwardString = function() {
        // 1.定义变量
        var current = this.head
        var resultString = ''
          // 2.依次向后遍历，获取每一个节点
        while (current) {
          resultString += current.data + ' '
          current = current.next
        }
        return resultString
      }

      // 3.insert方法
      DoublyLinkedList.prototype.insert = function(position, data) {
        // 1.越界判断
        if (position < 0 || position > this.length) return false

        // 2.根据data创建新的节点
        var newNode = new Node(data)

        // 3.判断原来的链表是否为空
        if (this.length == 0) {
          this.head = newNode
          this.tail = newNode
        } else {
          if (position == 0) { // 3.1判断position是否为0
            this.head.prev = newNode
            newNode.next = this.head
            this.head = newNode
          } else if (position == this.length) { // 3.2position==length相当于append
            this.tail.next = newNode
            newNode.prev = this.tail
            this.tail = newNode
          } else { // 3.3其他情况
            var current = this.head
            var index = 0


            while (index++ < position) {
              current = current.next
            }

            // 修改指针
            newNode.prev = current.prev
            newNode.next = current
            current.prev.next = newNode
            current.prev = newNode
          }
        }

        // 4.length+1
        this.length += 1
      }

      // 4.get方法
      DoublyLinkedList.prototype.get = function(position) {
        // 1.越界判断
        if (position < 0 || position >= this.length) return false

        // this.length/2 >position:从头向后遍历
        // this.length/2 《position:从尾向前遍历

        // 2.获取元素
        var current = this.head
        var index = 0

        while (index++ < position) {
          current = current.next
        }
        return current.data
      }

      // 5.indexOf方法
      DoublyLinkedList.prototype.indexOf = function(data) {
        // 1.定义变量
        var current = this.head
        var index = 0

        // 2.查找data相同的点
        while (current) {
          if (current.data == data) {
            return index
          }
          current = current.next
          index++
        }

        return -1
      }

      // 6.update方法
      DoublyLinkedList.prototype.update = function(position, newData) {
        // 1.越界判断
        if (position < 0 || position >= this.length) return false

        // 2.寻找正确的节点
        var current = this.head
        var index = 0
        while (index++ < position) {
          current = current.next
        }

        // 3.修改找到节点的data信息
        current.data = newData

        return true
      }

      // 7.removeAt方法
      DoublyLinkedList.prototype.removeAt = function(position) {
        // 1.越界判断
        if (position < 0 || position >= this.length) return false

        // 2.判断是否只有一个节点
        var current = this.head
        if (this.length == 1) {
          this.head = null
          this.tail = null
        } else {
          if (position == 0) { // 判断是否是第一个节点
            this.head.next.prev = null
            this.head = this.head.next
          } else if (position == this.length - 1) {
            this.tail.prev.next = null
            this.tail = this.tail.prev
          } else {
            var index = 0

            while (index++ < position) {
              current = current.next
            }

            current.prev.next = current.next
            current.next.prev = current.prev
          }
        }

        // 3.length-1
        this.length -= 1

        return current.data
      }

      // 8.remove方法
      DoublyLinkedList.prototype.remove = function(data) {
        // 1.根据data获取索引
        var index = this.indexOf(data)

        // 2.根据index删除
        return this.removeAt(index)
      }

      // 9.isEmpty和size方法
      DoublyLinkedList.prototype.isEmpty = function() {
        return this.length == 0
      }

      DoublyLinkedList.prototype.size = function() {
        return this.length
      }

      // 10.获取链表的第一个元素
      DoublyLinkedList.prototype.getHead = function() {
        return this.head.data
      }

      // 11.获取链表的最后元素
      DoublyLinkedList.prototype.getTail = function() {
        return this.tail.data
      }
    }

    // 测试代码
    var list = new DoublyLinkedList()

    // 1.测试append方法
    list.append('aaa')
    list.append('bbb')
    list.append('ccc')
    list.append('ddd')
    console.log(list)

    // 2.测试转出字符串方法
    console.log(list.toString())
    console.log(list.backwardString())
    console.log(list.forwardString())

    // 3.测试insert插入方法
    list.insert(0, '111')
    console.log(list.toString())
    list.insert(5, 'xxx')
    console.log(list.toString())
    list.insert(2, 'mmm')
    console.log(list.toString())

    // 4.测试get方法
    console.log(list.get(3));

    // 5.测试indexOf方法
    console.log(list.indexOf('mmm'));

    // 6.测试update方法
    list.update(2, 'nnn')
    console.log(list.toString())

    // 7.测试removeAt方法
    console.log(list.removeAt(6));
    console.log(list.removeAt(1));
    console.log(list.removeAt(0));
    console.log(list.toString());

    // 8.测试remove方法
    console.log(list.remove('bbb'));
    console.log(list.toString());

    // 9.测试isEmpty和size方法
    console.log(list.isEmpty());
    console.log(list.size());
    // 10.测试getHead和getTail
    console.log(list.getHead());
    console.log(list.getTail());
```

## 集合

1. 由一组**无序的**，**不能重复的**元素构成

* 一种特殊的数组，**没有顺序，也不能重复**
* 没有顺序意味着**不能通过下标值访问**，不能重复意味着**相同的对象**在集合中只会**存在一份**