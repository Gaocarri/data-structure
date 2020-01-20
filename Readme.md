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
* 就是ES6里的Set

2. 封装集合类

```javascript
    // 封装集合类
    function Set() {
      // 属性
      this.items = {}

      // 方法
      // add方法
      Set.prototype.add = function(value) {
        // 判断当前集合中是否已经包含了该元素
        if (this.has(value)) return false

        //将元素添加到集合中
        this.items[value] = value
        return true
      }

      // has方法
      Set.prototype.has = function(value) {
        return this.items.hasOwnProperty(value)
      }

      // remove方法
      Set.prototype.remove = function(value) {
        // 1.判断该集合中是否包含该元素
        if (!this.has(value)) {
          return false
        }

        // 2.将元素从属性中删除
        // delete操作符删除对象的某个属性
        delete this.items[value]
        return true
      }

      // clear方法
      Set.prototype.clear = function() {
        this.items = {}
      }

      // size方法
      Set.prototype.size = function() {
        return Object.keys(this.items).length
      }

      // 获取集合中所有的值
      Set.prototype.values = function() {
        return Object.keys(this.items)
      }
    }

    // 测试Set类
    var set = new Set()

    // 添加元素
    set.add('aaa')
    set.add('bbb')
    set.add('ccc')
    set.add('ddd')
    console.log(set);

    // has
    console.log(set.has('aaa'));

    // remove
    set.remove('aaa')
    console.log(set);

    // size
    console.log(set.size());
    // values
    console.log(set.values());
```

3. 集合间操作

* 并集

```javascript
      Set.prototype.union = function(otherSet) {
        // 1.创建一个新的集合
        var unionSet = new Set()

        // 2.将A集合中所有的元素添加到新集合中
        var values = this.values()
        for (var i = 0; i < values.length; i++) {
          unionSet.add(values[i])
        }

        // 3.取出B集合中的新的元素，判断是否需要添加到新集合
        values = otherSet.values()
        for (var i = 0; i < values.length; i++) {
          unionSet.add(values[i])
        }

        return unionSet
      }
```

* 交集

```javascript
      // 交集
      Set.prototype.intersection  =function(otherSet){
        // this集合A
        // otherSet 集合B
        // 1.创建新的集合
        var intersectionSet = new Set()

        // 2.从A中取出一个个元素，判断是否同时存在于集合B中，存放在新集合中
        var values = this.values()
        for(var i = 0;i<values.length;i++){
          var item = values[i]
          if(otherSet.has(item)){
            intersectionSet.add(item)
          }
        }
          
          return intersectionSet
      }
```



* 差集

```javascript
 // 差集（x存在于A当中，但不存在于B当中）
      Set.prototype.difference = function(otherSet) {
        // this集合A
        // otherSet 集合B
        // 1.创建新的集合
        var differenceSet = new Set()

        // 2.取出A集合一个个元素，判断是否同时存在于B中，不存在B中，则添加到新集合中
        var values = this.values()
        for (var i = 0; i < values.length; i++) {
          var item = values[i]
          if (!otherSet.has(item)) {
            differenceSet.add(item)
          }
        }

        return differenceSet
      }
```



* 子集

```javascript
      //子集（A是B的子集）
      Set.prototype.subset = function(otherSet) {
        // this集合A
        // otherSet 集合B
        // 遍历集合A中所有的元素，如果发现集合A中的元素，在集合B中不存在，那么false
        // 如果遍历完了整个集合，依然没有返回false,那么返回true即可
        var values = this.values()
        for (var i = 0; i < values.length; i++) {
          var item = values[i]
          if (otherSet.has(item)) {
            return false
          }
        }
        return true
      }
```



## 字典

* 在JavaScript中，似乎对象本身就是一种字典，所以在早期的JavaScript中，没有字典这种数据类型，因为你完全可以用对象做字典
* ES6添加了Map（映射关系，相当于字典）

# 哈希表

哈希表通常是基于**数组**进行实现的，但是相对于数组，他有很多优势

* 它可以提供非常快速的**插入-删除-查找操作**
* 无论多少数据，插入和删除值需要接近常量的时间：即O(1)的时间级，实际上，只需要**几个机器指令**即可完成
* 哈希表的速度比树还快，基本可以瞬间查找到想要的原元素
* 哈希表相对于树来说编码要容易很多

哈希表相对于数组的劣势

* 哈希表中的数据是**没有顺序的**，所以不能以一种固定的方式（比如从小到大）来遍历其中的元素
* 通常情况下，哈希表的key是**不允许重复的**，不能放置相同的key，用于保存不同的元素

1. 哈希表的原理

* 哈希化：将**大数字**转化成**数组范围内下标**的过程，我们称之为**哈希化**
* 哈希函数：通常我们会将**单词**转成**大数字**，**大数字**在进行**哈希化**的代码实现放在一个函数中，这个函数我们称之为**哈希函数**
* 哈希表：最终数据插入到这个数组，对整个**结构的封装**，我们就称之为一个**哈希表**

2. 解决冲突的方案

* 链地址法（每个下标值存储一个数组，而不是一个单独的对象），也称拉链法

* 开放地址法（寻找空白的单元格）

  * 线性探测（线性的查找空白的单元）

    * 注意：删除一个数据的时候，不能将这个位置下标的内容**设置为null**
    * 因为将它设置为null时可能会影响我们后续的其他操作，所以通常我们删除一个位置的数据项时，我们将它进行特殊的处理（比如设置为-1）
    * 比如我们设置的两个数据都放在了依次的两个null里，当我们删除第一个数据并将它设置为null的时候，我们查找第二个数据时会查找不到（查找的规则是查找原本的位置，没有的话再查找它后面的第一个null,存在数据再查找下一个，不存在数据直接返回不存在）
    * 存在一个严重的问题：聚集（一连串的填充单元），不如我们插入一个32，会发现**连续的单元都不允许**我们放置数据，并且这个过程中我们需要**探索多次**

  * 二次探测（为解决线性探测的问题）

    * 主要优化的是探测时的步长，什么意思呢
    * 步长，线性探测（比如从下标值x开始，x+1,x+2,x+3）,二次探测（x+1^2,x+2^2,x+3^2）,这样就可以依次探测比较长的距离
    
  * 再哈希法（不同的关键字求出不同的地址）
    * 第二次哈希化具备如下特点
      * 和第一个哈希函数不同
      * 不能输出0（否则原地踏步）
  
3. 哈希化的效率

* 如果**没有产生冲突**，那么效率就会更高
* 如果**发生冲突**，存取时间就依赖后来的探测长度
* 平均探测长度以及平均存取时间，取决于**填装因子**，随着填装因子变大，探测长度也越来越长
* 随着填装因子变大，效率下降的情况，将不同开放地址方案中比链地址法更严重，所以我们来对比一下它们的效率，再决定我们选取的方案

4. 在分析效率之前，我们先了解一个概念：装填因子

* 装填因子表示当前哈希表中已经**包含的数据项**和整个**哈希表长度**的**比值**
* 装填因子=总数据项/哈希表长度
* **开放地址法的装填因子**最大是多少呢？1，因为他必须寻找到空白的单元才能将元素放入
* 链地址法的装填因子呢？可以大于1，因为链地址法可以无限的眼伸下去，只要你愿意
* 一般的真实开发中，使用链地址法的情况较多（因为它不会因为添加了某元素后性能急剧下降）

5. 霍纳法则提升时间复杂度(减少了乘法的次数)

* 时间复杂度从O(N^2)降到了O(N)

6. 均匀分布（尽可能使用质数）

* 质数的使用
  * 哈希表的长度
  * N次幂的底数