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

7. 封装hash函数

```javascript
    // 设计哈希函数
    // 1）将字符串转换成比较大的数字：hashCode
    // 2）将大的数字hashCode压缩到数组范围之内
    function hashFunc(str, size) {
      // 1.定义hashCode变量
      var hashCode = 0

      // 2.霍纳算法，来计算hashCode的值
      // cats -> Unicode编码
      for (var i = 0; i < str.length; i++) {
        hashCode = 37 * hashCode + str.charCodeAt(i)
      }

      // 3.取余操作
      var index = hashCode % size

      return index
    }

    // 测试哈希函数
    alert(hashFunc('abc', 4))
    alert(hashFunc('sdsdd', 4))
    alert(hashFunc('awwe', 4))
    alert(hashFunc('annn', 4))
    alert(hashFunc('wke', 4))
```

8. 哈希表的实现

```javascript
    // 封装哈希表类
    function HashTable() {
      // 属性
      this.storage = []
      this.count = 0
      this.limit = 7 // 容量
        // loadFactor扩容或缩容

      // 方法

      // 哈希函数
      // 设计哈希函数
      // 1）将字符串转换成比较大的数字：hashCode
      // 2）将大的数字hashCode压缩到数组范围之内
      HashTable.prototype.hashFunc = function(str, size) {
        // 1.定义hashCode变量
        var hashCode = 0

        // 2.霍纳算法，来计算hashCode的值
        // cats -> Unicode编码
        for (var i = 0; i < str.length; i++) {
          hashCode = 37 * hashCode + str.charCodeAt(i)
        }

        // 3.取余操作
        var index = hashCode % size

        return index
      }

      // 插入和修改数据（因为key唯一，所以插入和修改是同一种）
      HashTable.prototype.put = function(key, value) {
        // 1.根据key获取索引值（目的是将数据插入到对应的位置）
        var index = this.hashFunc(key, this.limit)

        // 2. 根据索引值取出bucket
        var bucket = this.storage[index]
          // 1）如果桶不存在，创建桶(bucket)，并且放置在该索引的位置
        if (bucket == null) {
          bucket = []
          this.storage[index] = bucket
        }

        // 3.判断新增还是修改原来的值
        // 如果已经有值了，就修改值
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            tuple[1] = value
            return
          }
        }
        // 如果没有，执行后续的添加操作

        // 4.新增操作
        bucket.push([key, value])
        this.count += 1
      }

      // 获取方法
      HashTable.prototype.get = function(key) {
        // 1.根据key获取对应的index
        var index = this.hashFunc(key, this.limit)
          // 2.根据index获取对应的bucket
        var bucket = this.storage[index]
          // 3.判断bucket是否为null，如果为null,直接返回null
        if (bucket == null) {
          return null
        }
        // 4.线性查找bucket中每一个key是否等于传入的key，如果等于，直接返回对应的value
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            return tuple[1]
          }
        }
        // 5.遍历完后，依然没有找到对应的key,return null即可
        return null
      }

      // 删除操作
      HashTable.prototype.remove = function(key) {
        // 1.根据key获取对应的index
        var index = this.hashFunc(key, this.limit)
          // 2.根据index获取bucket
        var bucket = this.storage[index]
          // 3.判断bucket是否存在，如果不存在，那么直接返回null
        if (bucket == null) {
          return null
        }
        // 4.线性查找bucket,寻找对应的数据，并且删除
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            bucket.splice(i, 1)
            this.count--
              return tuple[1]
          }
        }
        // 5.依然没有找到那么返回null
        return null
      }

      // 其他方法
      // 判断哈希表是否为null
      HashTable.prototype.isEmpty = function() {
        return this.count == 0
      }

      // 获取哈希表中元素的个数
      HashTable.prototype, size = function() {
        return this.count
      }
    }

    var myInfo = new HashTable()
    myInfo.put('gaozheng', {
      age: 18,
      height: 175,
      weight: 65
    })

    console.log(myInfo);

    var ht = new HashTable()
      // 1.插入数据
    ht.put('cba', 'one')
    ht.put('bca', 'two')
    ht.put('nba', 'three')
    ht.put('bbb', 'four')
    ht.put('mba', 'six')
    console.log(ht);
    // 2.修改数据
    ht.put('cba', 'five')
    console.log(ht)
      //   // 3.获取数据
    console.log(ht.get('cba'))
      //   // 4.删除数据
    ht.remove('cba')
    console.log(ht);
```

9. 哈希表的扩容/缩容

* 为什么需要扩容

目前，所有数据项都存在**长度为7的数组中**，因为使用的是链地址法，**loadFactor**可以大于1，所以这个哈希表可以无限制的插入新数据，但是随着数据量的增多，每个index对应的bucket会越来越长，也就造车过了**效率的降低**，所以需要扩容

* 如何扩容

可以简单的将容量**扩大两倍**（不是质数，质数第10会解决），但是这种情况下，所有的**数据项一定要同时进行修改**

* 什么情况下扩容

比较常见的是loadFactor>0.75的时候进行扩容

* 代码(再在add和remove方法后判断是否扩容)

```javascript
 // 哈希表的扩容
      HashTable.prototype.resize = function(newLimit) {
        // 1.保存旧的数组内容
        var oldStorage = this.storage

        // 2.重置所有的属性
        this.storage = []
        this.count = 0
        this.limit = newLimit

        // 3.遍历oldStorage中所有的bucket
        for (var i = 0; i < oldStorage.length; i++) {
          // 3.1取出对应的bucket
          var bucket = oldStorage[i]

          // 3.2判断bucket是否为null
          if (bucket == null) {
            continue
          }

          // 3.3 bucket中有数据，那么取出数据，重新插入
          for (var j = 0; j < bucket.length; j++) {
            var tuple = bucket[j]
            this.put(tuple[0], tuple[1])
          }
        }
      }
```

10. 容量质数（有利于均匀分布）

* 判断一个数是否是质数

```javascript
function isPrime(num){
    for(var i =2;i<num;i++){
        if(num%i==0){
            return false
        }
    }
    return true
}
```

* 但是这种方法效率并不高，可以只遍历它的开平方根之前（因为一个数如果不是质数，它能被两个数相乘得到，那么其中一个数一定小于它的开平方根）

  ```javascript
      // 更高效的方式
      function isPrime(num) {
        // 1.获取平方根
        var temp = parseInt(Math.sqrt(num))
  
        // 2.循环判断
        for (var i = 2; i <= temp; i++) {
          if (num % i == 0) {
            return false
          }
        }
  
        return true
      }
  ```

* 代码

  * 定义判断质数方法
  * 定义获取质数的方法
  * 在添加和删除的扩容缩容方法中得到一个质数的容量

  ```
        // 判断某个数字是否是质数
        HashTable.prototype.isPrime = function(num) {
          // 1.获取平方根
          var temp = parseInt(Math.sqrt(num))
  
          // 2.循环判断
          for (var i = 2; i <= temp; i++) {
            if (num % i == 0) {
              return false
            }
          }
  
          return true
        }
  
        // 获取质数方法
        HashTable.prototype.getPrime = function(num) {
          // 14-->17
          // 34-->37
          while (!this.isPrime(num)) {
            num++
          }
          return num
        }
  ```

* 哈希表完整代码

```javascript
   // 封装哈希表类
    function HashTable() {
      // 属性
      this.storage = []
      this.count = 0
      this.limit = 7 // 容量
        // loadFactor扩容或缩容

      // 方法

      // 哈希函数
      // 设计哈希函数
      // 1）将字符串转换成比较大的数字：hashCode
      // 2）将大的数字hashCode压缩到数组范围之内
      HashTable.prototype.hashFunc = function(str, size) {
        // 1.定义hashCode变量
        var hashCode = 0

        // 2.霍纳算法，来计算hashCode的值
        // cats -> Unicode编码
        for (var i = 0; i < str.length; i++) {
          hashCode = 37 * hashCode + str.charCodeAt(i)
        }

        // 3.取余操作
        var index = hashCode % size

        return index
      }

      // 插入和修改数据（因为key唯一，所以插入和修改是同一种）
      HashTable.prototype.put = function(key, value) {
        // 1.根据key获取索引值（目的是将数据插入到对应的位置）
        var index = this.hashFunc(key, this.limit)

        // 2. 根据索引值取出bucket
        var bucket = this.storage[index]
          // 1）如果桶不存在，创建桶(bucket)，并且放置在该索引的位置
        if (bucket == null) {
          bucket = []
          this.storage[index] = bucket
        }

        // 3.判断新增还是修改原来的值
        // 如果已经有值了，就修改值
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            tuple[1] = value
            return
          }
        }
        // 如果没有，执行后续的添加操作

        // 4.新增操作
        bucket.push([key, value])
        this.count += 1

        // 5.判断是否需要扩容操作
        if (this.count > this.limit * 0.75) {
          var newSize = this.limit * 2
          var newPrime = this.getPrime(newSize)
          this.resize(newPrime)
        }
      }

      // 获取方法
      HashTable.prototype.get = function(key) {
        // 1.根据key获取对应的index
        var index = this.hashFunc(key, this.limit)
          // 2.根据index获取对应的bucket
        var bucket = this.storage[index]
          // 3.判断bucket是否为null，如果为null,直接返回null
        if (bucket == null) {
          return null
        }
        // 4.线性查找bucket中每一个key是否等于传入的key，如果等于，直接返回对应的value
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            return tuple[1]
          }
        }
        // 5.遍历完后，依然没有找到对应的key,return null即可
        return null
      }

      // 删除操作
      HashTable.prototype.remove = function(key) {
        // 1.根据key获取对应的index
        var index = this.hashFunc(key, this.limit)
          // 2.根据index获取bucket
        var bucket = this.storage[index]
          // 3.判断bucket是否存在，如果不存在，那么直接返回null
        if (bucket == null) {
          return null
        }
        // 4.线性查找bucket,寻找对应的数据，并且删除
        for (var i = 0; i < bucket.length; i++) {
          var tuple = bucket[i]
          if (tuple[0] == key) {
            bucket.splice(i, 1)
            this.count--
              return tuple[1]
          }
        }
        // 5.缩小容量
        if (this.count > 7 && this.count < this.limit * 0.25) {
          var newSize = Math.floor(this.limit / 2)
          var newPrime = this.getPrime(newSize)
          this.resize(newPrime)
        }
        // 6.依然没有找到那么返回null
        return null
      }

      // 其他方法
      // 判断哈希表是否为null
      HashTable.prototype.isEmpty = function() {
        return this.count == 0
      }

      // 获取哈希表中元素的个数
      HashTable.prototype, size = function() {
        return this.count
      }

      // 哈希表的扩容/缩容
      HashTable.prototype.resize = function(newLimit) {
        // 1.保存旧的数组内容
        var oldStorage = this.storage

        // 2.重置所有的属性
        this.storage = []
        this.count = 0
        this.limit = newLimit

        // 3.遍历oldStorage中所有的bucket
        for (var i = 0; i < oldStorage.length; i++) {
          // 3.1取出对应的bucket
          var bucket = oldStorage[i]

          // 3.2判断bucket是否为null
          if (bucket == null) {
            continue
          }

          // 3.3 bucket中有数据，那么取出数据，重新插入
          for (var j = 0; j < bucket.length; j++) {
            var tuple = bucket[j]
            this.put(tuple[0], tuple[1])
          }
        }
      }

      // 判断某个数字是否是质数
      HashTable.prototype.isPrime = function(num) {
        // 1.获取平方根
        var temp = parseInt(Math.sqrt(num))

        // 2.循环判断
        for (var i = 2; i <= temp; i++) {
          if (num % i == 0) {
            return false
          }
        }

        return true
      }

      // 获取质数方法
      HashTable.prototype.getPrime = function(num) {
        // 14-->17
        // 34-->37
        while (!this.isPrime(num)) {
          num++
        }
        return num
      }
    }
```

（注意在删除和插入操作时都做了质数变换）

## 树

1. 树的优点

* 综合了其他数据结构的优点（当然优点不足于盖过其他数据结构的优点，比如效率一般情况下没有哈希表高），并且弥补了上面数据结构的缺点
* 树结构的非线性使得它可以表示一对多的关系
* 比如文件的目录结构

2. 树的术语

* 树：n(n>=0)个节点构成的有限集合，当n=0时，称为空树
* 对于一个非空树，它有以下性质
  * 树中有一个称为"根（Root)"的特殊节点，用r表示
  * 其余节点可分为m(m>0)个互不相交的有限集T1，T2，...Tm,其中每个集合本身又是一颗树，称为原来树的“子树（SubTree）
* 节点的度（Degree）：节点的子树个数
* 树的度：树的所有节点中最大的度数
* 叶节点（Leaf）：度为0的节点
* 父节点：有子树的节点是其子树的根节点的父节点
* 子节点：若A节点是B节点的父节点，则称B节点是A节点的子节点；子节点也称孩子节点
* 兄弟节点（Sibling）：具有同一父节点的各节点彼此是兄弟节点
* 路径和路径长度：从节点n1到nk的路径为一个节点序列n1,n2,...nk，ni是ni+1的父节点。路径所包含边的个数为路径的长度
* 节点的层次（Leavel）：规定根节点在1层，其他任一节点的层数是其父节点的层数加1
* 树的深度（Depth）：树中所有节点中的最大层次是这棵树的深度

2. 树的表示

* 儿子，兄弟表示法

```javascript
Node {
    This.data = data
    This.leftChild = B
    This.sibling = C
}
```

任何一棵树，都可以用二叉树模拟出来

3. 二叉树的概念

* 如果树中的每个节点最多只能有两个子节点，这样的树称为'二叉树'
* 二叉树可以为空，也就是没有节点
* 若不为空，则它是由根节点和称为其左子树TL和右子树TR的两个不相交的二叉树组成
* 二叉树的5种形态：空，只有自身，自身加左子节点，自身加右子节点，自身加左右子节点

4. 二叉树的特性

* 一个二叉树第i层的最大节点数为2^(i-1),i>=1
* 深度为k的二叉树有最大节点总数为：2^k-1,k>=1
* 对任何非空二叉树T,若n0表示节点的个数，n2是度2为2的非叶节点个数，那么两者满足关系n0=n2+1

5. 特殊二叉树

* 完美二叉树：除了最下层的叶子节点外，每个节点都有二个子节点
* 完全二叉树：
  * 除二叉树最后一层外，其他各层的节点数都达到了最大个数
  * 且最后一层从左向右的叶节点连续存在，只缺右侧若干节点
  * 完美二叉树是特殊的完全二叉树

6. 二叉树的存储

常见方式是数组和链表

* 使用数组
  * 完全二叉树：从上往下，从左到右
  * 非完全二叉树：转成完全二叉树才能按上面的方案存储，但是会造成很大的空间浪费
* 使用链表（二叉树最常见的方式）
  * 每个节点封装成一个node，node中包含存储的数据，左节点的引用，右节点的引用

7. 二叉搜索树（也称为二叉排序树或者二叉查找树,BST）

* 二叉搜索树是一棵二叉树，可以为空
* 如果不为空
  * 非空左子树的所有键值小于其根节点的键值
  * 非空右子树的所有键值大于其根节点的键值
  * 左，右子树本身也都是二叉搜索树

8. 二叉搜索树的封装

```javascript
    // 封装二叉搜索树
    function BinarySearchTree() {

      function Node(key) {
        this.key = key
        this.left = null
        this.right = null
      }

      // 属性
      this.root = null

      // 方法
      // 1.插入数据
      BinarySearchTree.prototype.insert = function(key) {
        // 1.根据key创建节点
        var newNode = new Node(key)

        // 2.判断根节点是否有值
        // 有
        if (this.root == null) {
          this.root = newNode
        } else {
          this.insertNode(this.root, newNode)
        }
        // 无
      }



      // 内部的方法
      BinarySearchTree.prototype.insertNode = function(node, newNode) {
        if (newNode.key < node.key) { // 向左查找
          if (node.left == null) {
            node.left = newNode
          } else {
            this.insertNode(node.left, newNode)
          }
        } else { // 向右查找
          if (node.right == null) {
            node.right = newNode
          } else {
            this.insertNode(node.right, newNode)
          }
        }
      }

      // 2.树的遍历
      // 1.先序遍历
      BinarySearchTree.prototype.preOrderTraversal = function(handler) {
          this.preOrderTraversalNode(this.root, handler)
        }
        // 内部的方法
      BinarySearchTree.prototype.preOrderTraversalNode = function(node, handler) {
        if (node != null) {
          // 1.处理经过的节点
          handler(node.key)

          // 2.查找经过节点的左子节点
          this.preOrderTraversalNode(node.left, handler)

          // 3.查找经过节点的右子节点
          this.preOrderTraversalNode(node.right, handler)
        }
      }

      // 2.中序遍历
      BinarySearchTree.prototype.midOrderTraversal = function(handler) {
        this.midOrderTraversalNode(this.root, handler)
      }

      BinarySearchTree.prototype.midOrderTraversalNode = function(node, handler) {
        if (node != null) {
          // 1.查找左子树中的节点
          this.midOrderTraversalNode(node.left, handler)

          // 2.处理节点
          handler(node.key)

          // 3.查找右子树中的节点
          this.midOrderTraversalNode(node.right, handler)
        }
      }

      // 3.后序遍历
      BinarySearchTree.prototype.postOrderTraversal = function(handler) {
        this.postOrderTraversalNode(this.root, handler)
      }
      BinarySearchTree.prototype.postOrderTraversalNode = function(node, handler) {
        if (node != null) {
          // 1.查找左子树的节点
          this.postOrderTraversalNode(node.left, handler)
            // 2.查找右子树的节点
          this.postOrderTraversalNode(node.right, handler)
            // 3.处理节点
          handler(node.key)
        }
      }

      // 3.查找最值
      BinarySearchTree.prototype.max = function() {
        // 1.获取根节点
        var node = this.root
          // 2.依次不断的向右查找，直到节点为null
        while (node.right != null) {
          node = node.right
        }
        return node.key
      }

      BinarySearchTree.prototype.min = function() {
        var node = this.root
        while (node.left != null) {
          node = node.left
        }
        return node.key
      }

      // 搜索特定的key
      BinarySearchTree.prototype.search = function(key) {
        // 1.获取根节点
        var node = this.root
          // 2.循环搜索key
        while (node != null) {
          if (key < node.key) {
            node = node.left
          } else if (key > node.key) {
            node = node.right
          } else {
            return true
          }
        }
        return false
      }

      // 删除节点
      BinarySearchTree.prototype.remove = function(key) {
        // 1.寻找要删除的节点
        // 1.1定义变量，保存一些信息
        var current = this.root
        var parent = null
        var isLeftChild = true
          // 1.2开始寻找删除的节点
        while (current.key != key) {
          parent = current
          if (key < current.key) {
            isLeftChild = true
            current = current.left
          } else {
            isLeftChild = false
            current = current.right
          }
          // 某种情况：已经找到叶子节点，依然没有==key
          if (current == null) return false
        }
        // 2.根据对应的情况删除节点
        // 找到current.key==key
        // 2.1删除的节点是叶子节点(没有子节点)
        if (current.left == null && current.right == null) {
          // 是根节点
          if (current == this.root) {
            this.root == null
          } else if (isLeftChild) { // 叶子节点
            parent.left = null
          } else {
            parent.right = null
          }
        }
        // 2.2删除的节点有一个子节点
        else if (current.right == null) {
          if (current == this.root) {
            this.root = current.left
          } else if (isLeftChild) {
            parent.left = current.left
          } else {
            parent.right = current.left
          }
        } else if (current.left == null) {
          if (current == this.root) {
            this.root = current.right
          } else if (isLeftChild) {
            parent.left = current.right
          } else {
            parent.right = current.right
          }
        }
        // 2.3删除的节点有两个节点(后继)
        else {
          // 1.获取后继节点
          var successor = this.getSuccessor(current)

          // 2.判断是否是根节点
          if (current == this.root) {
            this.root = successor
          } else if (isLeftChild) {
            parent.left = successor
          } else {
            parent.right = successor
          }

          // 3.将删除的左子树=current.left
          successor.left = current.left
        }
      }

      // 找后继的方法
      BinarySearchTree.prototype.getSuccessor = function(delNode) {
        // 1.定义变量，保存找到的后继
        var successor = delNode
        var current = delNode.right
        var successorParent = delNode

        // 2.循环查找
        while (current != null) {
          successorParent = successor
          successor = current
          current = current.left
        }

        // 3.判断寻找的后继节点是否直接就是delNode的right节点
        if (successor != delNode.right) {
          successorParent.left = successor.right
          successor.right = delNode.right
        }

        return successor
      }
    }

    // 测试方法
    var bst = new BinarySearchTree()

    // 1.插入数据
    bst.insert(11)
    bst.insert(22)
    bst.insert(13)
    bst.insert(12)
    bst.insert(9)
    bst.insert(9)
    bst.insert(16)

    // 2. 测试遍历
    // 先序遍历
    var resultString = ''
    bst.preOrderTraversal(function(key) {
      resultString += key + ' '
    })
    console.log(resultString);
    // 中序遍历
    resultString = ''
    bst.midOrderTraversal(function(key) {
      resultString += key + ' '
    })
    console.log(resultString);
    // 后序遍历
    resultString = ''
    bst.postOrderTraversal(function(key) {
      resultString += key + ' '
    })
    console.log(resultString);
    // 测试最值
    console.log(bst.max());
    console.log(bst.min());
    // 测试搜索
    console.log(bst.search(26), bst.search(11));
```

9. 删除操作思路

* 先找到要删除的节点，如果没找到，不需要删除
* 找到要删除的节点
  * 删除叶子节点
  * 删除只有一个子节点的节点
  * 删除有两个子节点的节点
* 删除的节点有两个节点的情况
  * 如果要删除的节点有两个子节点，甚至子节点还有子节点，这种情况下我们需要从下面的子节点中找到一个节点，来替换当前的节点。
  * 但是找到的这个节点有什么特征呢？应该是current节点下面所有节点中最接近current节点的
    * 要么比current节点小一点点，要么比current节点大一点点
    * 最接近current，你就可以用来替换current的位置
  * 这个节点怎么找呢
    * 比current小一点点的节点，一定是current左子树的最大值
    * 比current大一点点的节点，一定是current右子树的最小值
  * 前驱&后继
    * 二叉搜索树中，这两个特别的节点，有两个特别的名字
    * 比current小一点的节点，称为current节点的前驱
    * 比current大一点的节点，称为current节点的后驱
  * 要么找他的前驱，要么找他的后驱
  * 所以，要先找到这样的节点