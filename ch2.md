### Chapter 2. Buffers
* 每一种基本类型都有对应的buffer class，除boolean之外
* 但是更倾向于使用ByteBuffer

#### 2.1 Buffer Basics
* Capacity: buffer能容纳的元素的个数，不能改变，比如能容纳10个元素（0-9）
* Limit：第一个访问不到的元素的位置，代表buffer中现存多少个元素，比如limit=7，表示0-6可以访问，现存7个元素
* Position：下一次访问的元素的位置，比如5，表示0-4位置都已经访问过，下一次访问5位置上的元素（第6个）
* Mark：不能小于0（初始化<0），否则reset会抛异常，reset表示position=mark，表示重置下一次访问的位置，当position < mark || limit < mark时，mark当前的值被丢弃掉
* 从有意义的数值上看：0 <= mark <= position <= limit <= capacity
##### 2.1.3 Accessing
* 指定位置的put不会修改position
##### 2.1.5 Flipping
* flip操作把position和limit置位，让position和limit的值适用于读取数据
* 连续2次flip操作使得position=limit=0，这样读写都会越界
##### 2.1.6 Draining
* channel读取数据到buffer后，buffer调用get方法获取数据前，要有flip操作
* 遍历数据有两种方式
    1.      for (int i = 0; buffer.hasRemaining( ), i++) {
                myByteArray [i] = buffer.get( );
            }
    1.      int count = buffer.remaining( );
            for (int i = 0; i < count, i++) {
                myByteArray [i] = buffer.get( );
            }
* 第一种方式允许多个线程取数据(TODO 并发也会有问题吧？ position++)，但非线程安全（TODO）  
##### 2.1.7 Compacting
          