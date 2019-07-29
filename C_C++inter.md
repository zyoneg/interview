# C/C++面试

## 语法基础/STL

- 指针和引用的区别，应用场景
  - 指针是一个变量，存放的是一个地址，通过指针可以找到存储在某一个位置的内容，
  - 引用就是某一个变量的别名，操作引用就是操作这个变量本身
  - 指针是一个实体，而引用则是一个别名
  - 引用必须初始化，指针可以不用
  - 引用在定义的时候初始化一次，之后不可以改变，指针可以随意改变，或者可以存储任意对象的地址
  - 不存在空值引用，但是存在空值指针
  - sizeof（引用）得到的是所指向变量的大小，对指针进行sizeof得到的是指针的大小，根据操作系统决定
  - 指针和引用++的意义不同，引用是给所引用对象的值++，指针是指向下一个地址
  - 程序可以为指针分配空间，引用不需要分配内存空间
  - 有多级指针，但是没有多级引用
  - 作为函数参数传递，可以实现对实参进行修改的目的，使用引用作为函数形参，实质上传递的就是实参本身，而不是实参的一个拷贝，
  - 引用底层用的还是指针
  - 使用引用：
    - 常引用：不能通过引用对目标变量值进行修改，从而使被引用的目标称为const
    - 作为函数返回值：当类A中包含类B的实例，可以返回B的引用，返回的就是引用而不会有副本产生
      - 不能使用局部变量作为引用返回，局部变量在函数执行完毕会被销毁，返回引用就会处于一种未知状态，导致程序异常
      - 不能返回函数内部new分配的内存的引用，有这样的情况：被函数返回的引用只是作为一个临时变量出现，没有赋予实际的值，那么这个引用所指向的空间无法释放，造成内存泄露
      - 重载某一个操作符或者已经知道指向一个对象并且不需要再指向其他的对象
  - 使用指针：
    - 有指向不存在对象的可能
    - 在不同的时刻指向不同的对象
- static的作用，extern关键字，
  - static修饰局部变量，修改存储位置，位于静态存储区，作用域仍然是局部作用域，当函数或者语句块结束时，并没有销毁静态变量，而是仍然驻留在内存中，不可以对他进行访问
  - 修饰全局变量，修改存储位置，程序整个运行期都在，连接属性变为内连接，在其他文件中不可见，没有被初始化的会自动被初始化为0
  - 修饰函数，改变连接属性，对于其他文件不可见
  - 修饰类静态函数，
  - 修饰类静态成员，可以实现多个对象之间的数据共享，并且静态数据成员不会破坏隐藏，保证了安全性，一个静态成员供多个对象使用
- 多态是什么，多态的实现原理
  - C++中虚函数的作用是为了实现多态机制，多态机制就是在继承的层次中，父类的指针可以具有多种形态--当父类指针指向某个子类对象时，可以不调用父类的函数，而是去掉用子类的函数
- 智能指针，实现原理
  - auto_ptr: 释放权限的转移
  - unique_ptr：只能单一的管理
  - shared_ptr：内部计数器，线程安全，有循环引用问题
- 数组和链表的区别？插入复杂度
  - 数组在内存中是一块连续空间，每个元素占用的空间相同，可以通过下标访问，
  - 链表在内存中不是连续存放，通过指针连接，每个结点都有两个域，一个是数据域，一个是指针域，
- 虚函数讲一下，什么函数可以声明成虚函数，什么函数不可以，什么函数最好可以被声明为虚函数
  - 不可以是虚函数：
    - 普通函数：不属于类，不可以被继承，只能被重载，不能被重写
    - 静态函数：理论上是可以继承，但是静态函数是在编译时就已经确定的，无法执行动态绑定，不支持多态，所以不可以被重写，不能被声明成虚函数
    - 构造函数：虚函数机制是通过对象的前四个字节的指针来实现的，在构造函数未执行之前，这个对象并不完整，无法使用多态
    - 内联函数
    - 友元函数
  - 析构函数最好是虚函数
    - 派生类析构函数会主动去掉用父类析构函数，所以如果父类指针或者引用指向子类时，没有将析构函数设置为虚函数，那么这个指针就只会去调用父类析构函数，导致子类的某些资源无法释放，造成内存泄露
- 重载，重写（覆盖），重定义（隐藏）区别
  - 重载是在同一作用域下，函数名相同，其参数列表不同，与返回值没有多大关系
  - 重写，要求在具有继承关系的父类与子类之间，并且是虚函数，子类重写来自父类的虚函数，函数名，参数，返回值都必须相同
  - 重定义，在具有继承关系的父类与子类之间，只要函数名相同就会构成重定义
- 一个空类都有哪些函数
  - 构造/析构
  - 拷贝构造/复制重载
  - 取地址/const取地址
- 浅拷贝存在问题
  - 可能造成同一个对象析构两次，double free
- 重载如何实现与使用，覆盖实现和使用
  - 重写（覆盖）：虚函数实现的多态
  - 隐藏：有继承
- **vector插入及其他操作是如何实现的**
  - erase：将last-finish的区间内容拷贝到first，这个操作基本上都是用copy
  - insert：如果有空间，在后面插入，如果没有空间，就配置空间，先将插入结点之前的元素移动，然后插入，最后再将插入结点之后的元素移动
- 内存中C++的地址空间是什么结构
  - 由低到高：代码段，(数据段)：已初始化全局数据区，未初始化全局数据区，堆，共享映射区，栈区，内核空间（1G）
- 内存对齐，编译过程
  - 内存对齐见幕布
  - 预处理：头文件展开，预处理指令，去注释，展开宏定义，处理编译器指令（#pragma）
  - 编译
    - 词法分析：源程序被输入到扫描器，只是简单进行词法分析，运用一种有限状态机的算法，识别记号
    - 语法分析：对扫描器产生的记号进行分析，从而产生语法树，采用与上下文无关语法的分析手段，语法树就是以表达式为结点的树
    - 语义分析：由语义分析器分析语句是否有意义，比如两个指针做乘法，编译器所分析的语义都是静态语义，是在编译器可以确定的语义。静态语义包括声明和类型的匹配，类型的转换
  - 汇编：将汇编代码转变成机器可以执行的指令，每一个汇编语句都对应一条机器指令
  - 连接：静态连接，动态连接
    - 人们把每个源码模块独立的编译，然后按照需求将其组装，这个组装的过程就是连接，连接的主要内容就是将各个模块之间的相互引用的部分处理好，主要包括地址和空间分配，符号决议和重定位
- C/C++语言的内存分配方式，内存布局，大小端问题
  - new/delete，
  - 判断大小端
- 函数调用底层发生了什么，从形参和实参说起
- vector动态增长会发生拷贝，自己使用时如何减少这个消耗，如何避免频繁扩容，vector存储上界
  - 在定义容器时就确定好容器的大小
  - 存储上界，会有一个max_size来表示存储的最大容量
- list实现，空间是如何分配的
- **malloc原理，申请内存时如何加锁，与new有什么区别**
- 已经有malloc和free，为什么还要有new和delete
  - 对于malloc和free而言，无法满足非内部数据类型对象的动态创建与消亡，一个class，在对象的创建时期要执行构造函数，在销毁是要执行析构函数，malloc是库函数，不在编译器的控制权限之内，不能把构造的任务强加与malloc
- C++11的新特性，右值引用的使用场景
  - 
- C++如何做动态内存管理
- sort的排序是什么排序
  - stl中的排序并非只是普通的快排，对普通的快排进行了优化，结合了插入排序和堆排序，根据不同的量级决定不同的排序算法，量大时采用快排，分段递归，分段后的数据量小于某一个伐值，采用插入排序，如果有向坏的情况发展，会改用堆排序
  - <https://www.cnblogs.com/fengcc/p/5256337.html> 
- sizeof一个类的各种情况
- 从内存角度解释局部变量和全局变量的区别
- C++内存的堆栈选择场景
- **重载在一种情况下，函数名相同，各个参数不同但是会报错，什么场景，为什么**
- 一个局部变量返回指针为什么会出错
  - 函数执行完毕，栈空间销毁，使用这个指针会造成无法估计的后果
- 模板的分离编译问题
- C++的编译单元是什么
  - 一个c和h源文件
- STL空间配置器，STL容器的应用场景和时间复杂度，
  - 为什么需要空间配置器：
    - 空间的申请与释放都需要程序员管理， 会造成内存泄露
    - 频繁申请空间会造成内存碎片，影响程序效率
    - 使用malloc申请空间会有额外的浪费
  - 内存内部碎片
    - 被分配的内存起始于4,8等数字处的地址处，用户申请一块9个字节的空间，但是可能返回的是12个字节的空间，这种因为四舍五入而造成的多余空间就是内存内部碎片
  - 内存外部碎片
    - 频繁的分配与回收内存会导致连续且小的页面夹杂在已经分配的物理页面之中，就会产生外部碎片
  - 一级空间配置器
    - 大于128字节的内存交给一级空间配置器来实现，实现原理也是非常简单，直接对malloc/free进行封装即可，
  - 二级空间配置器
    - 主要用来配置128字节一下的内存。先配置一块内存池
    - 内存池：先申请一块比较大的空间用作备用，当用户需要内存就去内存池中取数据，当内存池中的数据不够时再向内存去申请，用户空间使用完毕直接归还给内存池即可
    - 先去申请空间，如果对应桶下面有空间，就将第一个内存块直接返回给用户，如果没有就向这个桶中补充小块内存，然后将第一个内存返回给用户
    - 从内存池中索要空间，如果值索要了一块，就直接将这块空间返回，否则就将剩下的空间挂在对应的桶中
    - 内存池能否提供一块空间？能就返回，如果不能就补充内存池，先将内存池中的剩余内存挂在对应桶上，然后调用申请`本次申请空间的两倍+向系统申请总大小/16`这么大，然后申请空间，如果申请失败，就去找有没有更大的hash桶内有空间，如果有就把这个空间拿来用，没有就交给一级空间配置器

- map和set的区别
  - map是一种关联式容器
    - 以RBTree作为底层实现，
    - 所有元素都是键值对的形式存在
    - 不允许键重复
    - 所有元素都是通过键来自动排序
    - 键不能修改，但是值可以修改
  - set是一种关联式容器
    - 以RBTree作为底层实现
    - 所有的元素只有key没有val，key即使val
    - 不允许键重复
    - 所有元素都会被自动排序
    - 不能修改set的值，因为set的值就是键
- map，set，vector，list，的迭代器失效问题
  - <https://blog.csdn.net/lujiandong1/article/details/49872763> 
  - vector等顺序容器在内存中是一块连续的内存，当删除一个元素之后，内存中的数据会发生移动，导致删除一个元素之后后面所有的元素都会向前移动一个位置，导致后面的迭代器都失效，数据地址发生改变，之前获取的迭代器就访问不到正确的数据
  - 关联式容器删除当前iterator，只会导致当前的iterator失效，map采用红黑树来实现，插入，删除一个结点不会对其他结点造成影响，erase迭代器只是被删除元素的迭代器失效，使用erase（iter++）的方式来删除
    - map是关联式容器，以红黑树的形式组织数据，虽然删除了一个元素，整棵树也会调整，但是单个结点在内存中的地址没有变化，变化的是指向各个结点的关系
  - **经过erase（iter）之后的迭代器完全失效，该迭代器不能参加任何运算**
- strcpy如何实现，内部有没有动态内存管理
- const和宏的区别
- 空指针，数组越界是错误还是异常，如何解决
- 强引用和弱引用
  - <https://www.cnblogs.com/kuzhon/articles/5648807.html> 
- C++11中的auto有什么用，不给初始值可以？
  - auto必须在定义时初始化
  - 初始化表达式是引用，出去引用语义
  - 初始化表达式是 const或者volatile，除去这个语义
  - auto并不是真正的类型，而是一个占位符
- 构造函数能否抛出异常，析构函数呢？
  - <https://www.cnblogs.com/KevinSong/p/3323372.html> 
- C++对象模型
  - <https://blog.csdn.net/smile_zhangw/article/details/79341141> 
  - 对象模型概述
    - 成员函数：static func， nostatic func， virtual func
    - 成员数据：static data， nostatic data
- operator new的实现机制，delete掉的内存能否重用，如何重用
- C++线程库std::pthread，Posix多线程的pthread
- 讲解lambda函数吗，讲解一下实现原理，讲解一下捕获参数的实现 和stl里function中的bind的区别 
  - 定义一个lambda函数实际上是定义了一个类，然后在类中重载operator（）
  - 
- map-reduce
- `const vector<int>` 和 `vector<const int>`区别
- 拷贝构造函数调用的时机
  - 当做函数的参数
  - 函数的返回值是类的对象
- 1G地址空间申请1.2G？如果使用这个空间会有什么问题
- vector扩容的时间复杂度
  - <https://blog.csdn.net/qq_22080999/article/details/81199367> 
  - 每次增加倍数均摊时间复杂度为常数时间辅助度，但是每次增加固定值的时间复杂度大小是O(N)
  - 以2倍的方式增长，导致下一次申请的内存必然大于之前的分配内存的总和，导致之前分配的内存不能再使用，1.5倍增长更好的对内存实现重复利用
- 红黑树的插入，查询，修改时间复杂度，与AVL树相比他的优越性体现在哪
  - 
- vector `push_back` 和 `emplace_back` 的区别 -- 针对自定义类型
  - 如果是一个自定义类型的对象，vector在底层插入时不会直接使用这个对象，而是会根据这个对象去重新拷贝构造一个新的对象去插进入，所以会插入过程中会调用 构造函数+拷贝构造函数
  - 而emplace_back是一种效率更高的方式，他是直接会调用构造函数然后将构造好的这个对象插入进vector中，不会调用拷贝构造函数

## 操作系统

- 什么是进程，什么是线程

  - 进程是程序的一次动态执行，进程可以看作是程序的一个实例，进程是系统资源分配的独立实体，每个进程都拥有独立的地址空间，一个进程无法直接访问另一个进程的数据结构和变量，从内核角度而言，创建一个进程只是创建了一个task_struct结构体，因为操作系统内核要管理进程就需要对进程的数据进行描述并组织起来，内核就是用task_struct来进行描述，里面有进程的id，进程状态，还有进程优先级，mm_struct来描述进程的虚拟地址空间，pending位图来表示本进程的信号屏蔽以及安排，还有表示当前目录等等，总之一切能够描述这个进程的数据结构都在这个结构体中，然后内核调度进程只需要调度这个结构体就可以，进程创建完毕之后就可以完成用户指定的某种任务，但是现代计算机很少有一个进程独立完成某一个任务，因为各种各样的原因需要两个进程来进行通信交换数据，然后可以通过信号量，消息队列，文件，共享内存，信号量，等等来进行通信，或者可以通过fork来创建子进程来完成任务，父进程通过waitpid来等待子进程退出，如果父进程不知道何时调用，就可以通过信号来解决这个问题，注册信号的处理函数，如果有SIGCHLD信号，就使用waitpid等待回收子进程，然后子进程可以退出，本进程内部也可以通过调用exit进行函数退出
    - fork底层是通过调用clone来创建一个子进程，clone内部通过调用do_fork来完成更进一步的创建工作，调用dup_task_struct为新的进程创建一个内核栈和task_struct，这些值与当前进程值相同，检查新创建的进程数目没有超过用户限制，然后就开始使自己与父进程不同，将进程描述符内的许多标志位都清0，但是大多数数据都没有修改，这是因为Linux为了尽快创建一个进程，采用写时拷贝，因为如果一个进程刚创建就执行exec，如果创建之后就立即进行数据复制分离，那么这回大大浪费资源和时间，设置自己的标志位之后设置状态保证不会被投入运行，然后就开始更新子进程内部的标志位，分配一个新的pid，拷贝内部打开的文件以及描述信息，信号处理函数以及进程地址空间。
      - 父子进程的区别：1、fork返回值不同，进程id不同
      - 子进程不继承父进程的文件锁
      - 子进程的未处理信号集设置为空集
      - 子进程的未处理闹钟被清空
  - 线程就是进程内部的一个执行流，一个进程可以拥有多个线程，每个线程都有自己的独立栈空间，在Linux下面线程是用进程的PCB模拟的，所以 线程就是一个轻量级进程，所以进程就退化成了线程组
  - 进程是操作系统的资源分配的基本单位，线程是CPU调度的基本单位

- 进程和线程的区别，线程的优点，多线程的安全问题，各种锁，互斥锁和自旋锁

  - 一个进程是有多个线程，这些线程共享同一个虚拟地址空间，线程是运行在进程之中，所以线程之间通信极为方便，创建和销毁一个线程也是极为方便，调度切换相对于进程也是比较清凉的，
  - 线程共享：文件描述符表，一个线程打开文件，另一个线程也是可以直接操作，信号的处理方式，
  - 线程独有：标识符，栈区，上下文数据，errno，信号屏蔽字
  - 因为线程之间的通信极为方便，只需要全局变量就可，所以多线程的线程安全问题就尤为重要，因为线程的调度完全是随内核的，不知道什么时候这个线程退出，有可能一个线程正在对这个数据做修改，但是另一个线程抢占cpu，对这个数据再次做出修改，那么等到第一个线程读出来的数据就不安全
  - 互斥锁，读写锁，自旋锁（轻量级锁），

- 如何设计一个线程池，什么时候用多进程，什么时候用多线程，

  - 一个线程池中要有任务队列用来存储任务，所以一个队列是必不可少的，还需要有互斥锁以及条件变量，如果没有任务就需要等待在条件变量上面，如果等到条件变量发送的消息，就说明有任务，直接将任务出队运行即可
  - 多进程的使用场景：没有与其他进程过多的数据交互，并发量不大，需要霸占整个系统资源的，需要绝对的数据安全
  - 多线程的使用场景：需要尽快的数据通信，适用于web服务器，因为高并发多进程很容易使系统宕机，就算使用进程池进程的调度也是没有线程调度便捷，还有就是适用于IO密集型场景，可以起一个线程单独等待IO，其他线程做处理即可

- 一个进程中能开多少个线程，能开的线程还受限于什么，线程过多会有什么不好的地方

  - 阿里云服务器测试：32747，在4G的内存空间下，有3G是用户的内存，通过ulimit 查看栈大小，用3G除以这个大小即可
  - 进程最多是可以设置的，通过ulimit查看

- 堆和栈的区别，栈的空间是不是有限的？从什么地方看，栈的大小如何修改

  - 堆和栈是操作系统管理内存的两种方式
    - 管理方式不同：栈由操作系统自动分配释放，无需手动控制，堆的申请与释放由程序员自己进行，容易造成内存泄露
    - 空间大小不同，每个进程拥有的栈空间大小远小于堆空间的大小，理论上程序员申请的最大内存为虚拟内存的大小，进程栈的大小是可以配置的，ulimit -s
    - 生长方向不同，栈是由高地址到低地址，堆则相反
    - 分配方式不同，堆都是通过动态分配，没有静态分配的堆，栈是两种分配：静态和动态，静态分配有操作系统进行，动态分配有alloc分配，由操作系统自己释放
    - 分配效率不同，栈由操作系统进行分配，有硬件支持，有专门的寄存器ebp和esp存放地址，有专门的指令执行，效率比较高，堆是由C/C++提供的函数和操作符来进行，频繁的申请内存可能会造成内存碎片
    - 存放内容不同：栈存放的是局部变量和函数返回值，相关参数，堆中一般是有一个字节的空间来存放堆的大小，具体内容由程序员填充

- Linux指令：查看进程相关（ps），查看网络（netstat），查看磁盘，查看内存，查看CPU利用率，抓取网络数据包

  - ps，netstat，free
    - free显示内存和交换空间的使用情况，free是真正未被使用的物理内存数量，
  - df显示文件系统的磁盘使用情况，top，tcpdump

- Linux/操作系统中断机制，开机启动流程

  - 中断是指CPU对系统发生的某一个事件做出的反应，CPU暂停正在执行的程序，保存现场之后执行相应处理程序
  - CPU外部引起（IO中断，时钟中断）
  - CPU内部中断（非法操作，地址越界，浮点溢出）
  - 系统调用

- 死锁概念，如何产生，如何解决，必要条件？

  - 两个或者两个以上的线程在执行过程中，因为争夺资源而产生的相互等待的现象，没有外力推动，将会一直等待下去无法正常执行任务，
  - 必要条件：互斥，请求与保持，环路等待，不可剥夺
  - 解决：死锁预防，死锁避免，死锁检测，死锁解除（强制收回资源）
  - 产生：

- **程序中如何判断死锁，如何写代码检测出死锁**

  - 

- 内存申请除了brk还有什么？mmap
  - brk是将虚拟内存中的（.data）的最高地址指针_edata向高地址推
  - <https://blog.csdn.net/weixin_36145588/article/details/78363836> 
  - 从操作系统的角度来看，进程分配内存有两种方式，由两个系统调用完成，brk和mmap
  - 这两个分配方式都是分配虚拟内存，没有分配物理内存，而是在第一次访问虚拟内存时发生缺页异常，然后由操作系统完成物理内存的分配，建立虚拟地址和物理内存之间的关系，malloc/free底层是有brk和mmap来实现的
  - malloc小于128K的内存，使用brk分配内存，将_edata指针向高地址推，只分配虚拟内存，第一次读写数据时，引起内核缺页中断，内核分配相应的物理内存，建立映射关系
  - malloc大于128K的内存，使用mmap分配内存，在堆栈之间找一块空闲的内存进行分配

- malloc底层实现--- mmap和brk

  - mmap实现原理：<https://blog.csdn.net/qq_33611327/article/details/81738195> --- 深度好文

    - 用户空间调用mmap只是在task_struct中找到一块连续空间来创建一个vm_area_struct结构，对这个结构进行初始化，然后将这个结构加入到链表或者树中	
    - 调用内核的mmap，实现文件物理地址和进程虚拟地址的一一映射关系，内核mmap函数通过虚拟文件系统inode模块定位到文件磁盘物理地址，然后建立页表页，实现文件地址和虚拟内存的映射关系
    - 当进程发起对这片空间的访问，引发缺页异常，实现文件内容到物理内存的拷贝，如果进程对这片空间的写改变其内容，操作系统在一定时间会自动写回脏页面，完成文件写入过程

  - mmap与一般文件对比：常规文件操作为了提高读写效率和保护磁盘，使用了页缓存机制。这样造成读文件时需要先将文件页从磁盘拷贝到页缓存中，由于页缓存处在内核空间，不能被用户进程直接寻址，所以还需要将页缓存中数据页再次拷贝到内存对应的用户空间中。这样，通过了两次数据拷贝过程，才能完成进程对文件内容的获取任务。写操作也是一样，待写入的buffer在内核空间不能直接访问，必须要先拷贝至内核空间对应的主存，再写回磁盘中（延迟写回），也是需要两次数据拷贝。

    而使用mmap操作文件中，创建新的虚拟内存区域和建立文件磁盘地址和虚拟内存区域映射这两步，没有任何文件拷贝操作。而之后访问数据时发现内存中并无数据而发起的缺页异常过程，可以通过已经建立好的映射关系，只使用一次数据拷贝，就从磁盘中将数据传入内存的用户空间中，供进程使用。

  - mmap优点：

    - 对文件的读取跨过页缓存，减少数据的拷贝次数，用内存代替IO读写，提高文件读取效率
    - 实现用户空间和内核空间的高效交互方式
    - 提供进程间共享内存以及相互通信的方式，进程可以将自身的用户空间映射到同一个文件或者匿名映射到同一片区域
    - 可以用于实现大规模的数据传输，

- 当一个进程发生缺页中断，进程陷入内核态，执行一下操作

  - 检查要访问的虚拟地址是否合法，
  - 查找/分配一个物理页，填充物理内容（读取磁盘或者置0，）
  - 建立映射关系，虚拟地址到物理地址
  - 重新执行缺页中断的指令

- **缺页异常和缺页中断**

  - <https://www.cnblogs.com/zhangtiezi/p/8404589.html> 

- 多进程的地址空间是独立的，在操作系统中是如何实现的？虚拟内存

- 两个进程去同时访问一个线性地址可以吗

- 段错误是如何导致的

  - 指针变量指向的地址不存在
  - 指针变量指向的地址存在，但是权限错误

- 操作系统如何去实现进程/线程的切换？CPU是如何运作的

  - 

- 信号机制的用途，信号如何处理？信号处理是同步还是异步？kill命令何时返回

- 阻塞和非阻塞的概念

- Socket编程的接口，客户端和服务端所经历的状态，当客户端不close直接退掉会有什么情况发生

- IO密集型和计算密集型，线程的数量

- **epoll和select的区别，有何优缺点，在什么情况下用，五种IO模型平时如何使用，ET和LT的区别**

- 如何保证多个线程获取的单例是同一个

- 互斥锁的底层实现

  - 内部使用原子计数：atomic_t count，自旋锁： spinlock，等待队列 list_head

- Linux下文件的三个属性，Access，Modify，Change，如何用命令获取

- 操作系统内存管理

- 进程间通信的方法（API）

  - 无名管道，一种半双工的通信方式，一般只适用于具有亲缘关系间的进程
  - 高级管道，两个进程运行在同一个地址空间
  - 有名管道：
  - 消息队列：消息的列表，存放在内核中由消息队列标识符表示，
  - 信号量：本质上是一个计数器，控制多个进程对共享资源的访问
  - 信号：是一个相对比较复杂的通信方式
  - 共享内存：与信号量配合使用
  - 套接字：用于不同网络上的进程进行通信
  - 共享内存不涉及到内核的拷贝，所以是最快的进程间通信方式

- 僵尸进程和孤儿进程，守护进程

- 多线程如何使用gdb进行调试

  - `info threads`：显示当前可以调试的所有线程
  - `thread id`：调试目标id对应线程
  - `set scheduler-locking[off|on|step]`
    - off表示不锁定任何线程，所有线程都可以执行
    - on表示当前被调试的线程会继续执行
    - step表示在单步执行的时候只有当前线程会执行

- Linux中的loadaverage是如何计算的

  - load average就是对当前CPU工作量的度量，三个数分别表示 最近1分钟的平均负载，最近五分钟的平均负载，最近15分钟的平均负载

- 虚拟地址空间和物理内存，shell原理

- 如何让一个应用程序一起来就挂掉

- write函数的作用，为什么要有一个缓冲区

- 虚拟内存在底层如何实现

- 事件驱动模型，REactor和Proactor的理解，IO速率，sort指令的内部实现

- accept系统调用，系统如何维护已连接队列

- 同步异步理解，

- 父进程创建两个进程，其中一个子进程除0错误，另外两个进程的状态，如果是线程？

- 线程栈空间给了一个超过栈内存的变量发生什么

- 进程和线程一般开辟几个

- 系统性能不高，但是CPU占用率高，如何解决

- 使用free查看内存，显示内存没有剩余，表示操作系统没有内存可用？buffer和cache的作用

- CPU分配给进程哪些资源

- 文件写到磁盘的流程，内存页的大小，磁盘块的大小

- 创建一个线程占用的内存，

- Linux Aio底层，同步异步阻塞非阻塞

- 多线程双buf拷贝，如何设计，避免使用锁，（参考：高并发，buf五分钟更新一次，http请求200ms内必须返回）

- 线程模型有哪些？内核线程是什么

- 堆和栈有什么不同，为什么要设计这两个东西，栈上的变量申请在堆上可不可以，本质为何需要堆和栈 

- 堆上内存分配管理，在操作系统层级怎么做的，如果让你设计，怎么办，采用何种数据结构，如何避免内存碎片，内存浪费 

- 共享库的映射区是做什么的，mmap映射到文件对象

- CPU调度是如何做的，操作系统内存的分配与释放

- 如何保证共享内存的安全性

- 多线程通信方法，多线程的安全问题如何解决？如果让数据私有该如何，tls底层实现

- unique_lock和lock_groud的区别？ send和recv如何设置非阻塞，非阻塞如果没有数据可读返回什么？

  - send，recv使用非阻塞发送和读取消息，将flags设置为 MSG_DONTWAIT
  - 函数返回0，表示对端关闭，没有数据可读，返回-1，并且设置错误码为EAGAIN
  - send函数：`send (int fd, const char *buf, int len, int flags)`
    - 先比较len和fd发送缓冲区的长度，如果len>s，就直接返回SOCKET_ERROR
    - 检查是否正在发送缓冲区的数据，如果是就等协议把数据发送完毕，如果没有发送或者缓冲区没有数据，那么就比较len和剩余缓冲区的大小
    - len > 剩余缓冲区的大小，一直等协议把发送缓冲区的数据发送完毕
    - 如果小于，就直接copy到发送缓冲区中
    - 在传送数据时如果对端断开，那么调用send的进程会触发一个SIG_PIPE信号，这个信号的默认动作是终止进程
  - recv函数：`recv(int fd, char *buf, int len, int flags)`
    - 

- 一个进程最多可以用多少内存空间

- 阻塞，非阻塞，同步，异步

  - 同步：调用者必须自己去查看事件有没有发生，
  - 异步：调用者不去自己查看事件是否发生，而是等待注册在事件上的回调函数通知

- 虚拟内存，缺页中断，缺页异常，CPU调度，

- 服务器如何处理多个session

- 系统调用的底层过程

- Linux读写文件的底层过程

- 进程切换开销为什么大？

- 磁盘IO的性能瓶颈，磁头和磁盘的设计

- 局部性原理在设备上如何实现

- 如何设计一个文件系统

- read和write读写文件的具体过程

## 数据结构/算法

- 剑指offer
- strlen的递归实现，非递归实现
- rand（）1-6等概率，如何实现and（）1-10等概率
- 101个硬币，其中有一个是假的，有一个天平，称两次判断这个假硬币是轻是重
- 程序运行中一直进入if语句，如何进入else语句（不能重新编译，不能改动代码）
- 两个单链表相交，遍历一次找到相交结点，写出推倒过程
- 快排的时间复杂度？最坏的情况是什么？
- 一个数在一个递增数组中出现多次，找到那个数统计次数，写代码
- 树查找的复杂度，最坏的情况
- 有两个鸡蛋，在某一层是不会仍坏的，但是在这一层上面会坏，如何找出这一层，最坏找几次
- 发扑克牌，平均分为三推，大王和小王在一起的概率
- 最大子序列和，
- 二叉树的遍历（广度和深度遍历），判断二叉树是否中心对称，大数相乘
- 哈弗曼树和哈希表，堆的调整算法，
- 有n个有序的数组，只能两两合并，最终成为一个m长度的数组，有几种方法
- 门外有四个开关，初始状态都是一样的，门内四个灯，每个开关对应一个灯，可以操作开关，但是门只能开一次，确定对应关系
- 前序和中序如何建立树
- 鼠标点击一个响应，有网络延迟的话，如何让多个响应只处理一次
- 二叉平衡树的判断
- 两个字符串的最长公共子串
- 哈希和红黑树的区别，红黑树的优缺点
- 一个班相同生日的概率
- 顺时针打印二维数组，旋转数组的二分查找
- AVL树的插入实现，如果两个AVL树合并，要求N的时间复杂度
- 一个骰子的所有点的数学期望
- 如何判断一个IPv4地址是否合法
- 若干个文件，文件内容有序，如何合并成一个大文件
- 一个单链表如何排序
- 如何用数组实现一个循环队列
- 如何判断一棵树是不是另一颗树的子树
- 查找单链表中的倒数第k个结点
- 一个天平，8个弹珠，7个重量一样，称几次可以找到重的那一个
- 排序算法的稳定性？分治法，大规模分治法？
- 二叉树遍历非递归，大量url如何去重
- 哈希和一致性哈希，哈希桶查找，如何确定哈希表大小，哈希冲突的解决办法，二次探测的删除如何删除，哈希表和链表的区别，哈希冲突如何解决
  - hash元素删除必须使用惰性删除，因为hash表中每一个元素不仅代表他自己，还关系到其他元素的排列
  - 当表格大小为质数，并且永远保持负载因子在0.5以下，那么没插入一个新的元素所需要的探测次数不多于2
  - STL中 hashtable的buckets聚合体以vector完成
  - STL以质数来设计表格，并且将28个质数计算好，以备随时访问，同时提供函数来查询最接近某数并大于某数的质数s
- 3升和5升的桶，如何打出4升水
- 八皇后算法
- BST中两个结点差的最小值
- 爬楼梯，一次智能爬一次或者两次，n阶楼梯一共有多少种爬法---斐波那契数列
- 一枚硬币，质量不均匀，正面概率0.7，反面概率0.3，两个人喝一瓶水，如何等概率确定分给哪一个人
- 给一个数组，找出从最小值开始的第一个缺失的数字，时间复杂度O(n)
- 给定一个数组，有若干个数，找到a+b+c=99的所有abc组合
- 在一维数组中找出超过数组长度一半的元素
- 64匹马8条赛道，找出最快的四匹
- 从一个socket中读取字符，判断是否有目标字符串，30s后没有找到就返回
- 实现memove
- 实现10进制转任意进制
- K行，每一行都有无穷多的数，每一行都是从小到大排列，在这K行中找前N最大的数
- 在无序数中找到中位数
- 二维数组，最长递增序列 -- leetcode 329
- TOP K问题: 多个文件无重复记录TOP K
  - 有重复记录: 多个文件记录IP访问频率,
- LRU
- 哈希结构什么时候重新hash
- trapping-rain-water 

## 网络

- tcp和udp的区别？TCP是如何做到有序的
  - TCP有连接，可靠，面向字节流
  - UDP无连接，不可靠，面向数据报
  - TCP使用序号做到数据有序：
    - 在TCP每次主机发送信息时，都会随机给每个数据包分配一个序号，在一段时间内会等待对端主机对这个数据包进行确认
    - 发送主机在一段时间之后没有收到确认序号，会对这个数据包进行重发
    - 接收主机利用确认序号对接收到的数据进行确认，用来确定发送发的数据是否丢包或者乱序
    - 接收主机接收到已经顺序化的数据，会交给上层来处理这些数据
  - 具体步骤
    - 为了保证数据包的可靠传递，发送放必须将已经发送的数据包留在缓冲区
    - 为每个已经发送的数据包绑定一个超时计时器
    - 在超时计时器超时之前收到应答，释放该数据包占用的缓冲区
    - 定时器超时，重传该数据包，直到收到应答或者达到最大重传次数
    - 接收方接收到数据之后，先对其进行CRC校验，如果正确就交给上层应用处理数据，给对方发送一个确认应答包，表示该数据已经收到，如果刚好有数据发送给对方，则一并捎带过去
- HTTP基于什么实现，位于哪一层
  - HTTP基于TCP实现，是位于应用层的协议
- CGI是什么，如何判断CGI请求
- ICMP和ARP协议
- 交换机的工作原理，路由器的工作原理，路由器如何管理路由表，规划最短网络路径
- udp调用connect
- 四次挥手，TIME_WAIT状态，
- TCP可靠传输，UDP如何保证可靠传输
- 输入域名后发生了什么，url太长超过限制会发生什么事情
  - 浏览器首先需要将url解析成ip地址，解析域名用到DNS协议，首先会查主机的DNS缓存，如果没有就向DNS服务器发起请求，查询分为两种方式，一种递归查询，一种迭代查询，迭代查询本地DNS服务器向根域名服务器发起查询请求，根域名服务器告知该域名的一级域名服务器，本地服务器给该一级域名服务器发起查询请求，依次类推到查询该域名的IP地址，NDS协议基于UDP，
  - 得到IP地址之后，浏览器会与服务器建立连接，会用到HTTP协议，HTTP协议会生成一个GET请求报文，将该报文传给TCP做出处理，如果使用HTTPS还会涉及到SSL，会对数据进行加密，TCP如果有需要会将数据包进行分片，分片依据MTU和MSS，TCP的数据包会发送给IP层，应用IP协议，IP层通过路由选择，一跳一跳发送到目的地，在一个网段内的寻址是通过以太网实现的，以太网需要MAC地址，所以会用到ARP协议
  - DNS，HTTP，HTTPS属于应用层协议，是整个体系的最高层，应用层需要确定进程之间的通信性质以满足用户的需求，应用层不仅需要提供应用进程所需要的信息交换和其他操作，而且还要作为相互作用的应用进程的用户代理
  - TCP/UDP属于传输层，传输层负责主机中两个进程之间的通信，面向连接的服务提供可靠的交付，无连接的服务不保证提供可靠的交付，只是尽最大努力交付，
  - 网络层将传输层产生的数据封装成组选择合适的路由进行转发，
  - 数据链路层的任务是将网络层交付的IP数据报组装成帧，在两个相邻的结点的链路上进行传送
  - 物理层知识简单的传送数据比特流，
- HTTPS如何实现，与HTTP有什么区别，HTTPS的优缺点
  - HTTP是以明文在网络中进行传输，而HTTPS是将经过TLS加密之后的数据进行传输，HTTPS具有更高的安全性
  - HTTP在三次握手之后，还需要经过SSL建立连接，HTTPS是传输在SSL上的数据，协商加密使用的对称加密秘钥
  - HTTPS协议需要申请证书，浏览器安装对应证书
  - HTTP端口80，HTTPS端口443
  - **HTTPS优点**：
    - 数据传输使用秘钥加密，安全性更高
    - 可以认证服务器和客户端，确保数据发送到正确的用户和服务器
  - **HTTPS缺点**
    - 握手时延高，在进行HTTP会话之前还需要进行SSL握手，因此握手阶段时延增加
    - 部署成本高，需要购买证书验证自身安全性，由于使用HTTPS需要进行加密解密计算，占用CPU资源多
- 网卡接收数据到数据被处理期间发生了什么
- TCP的三次握手，四次挥手，三次握手的必要性，为什么三次握手，四次挥手，三次握手的缺陷
  - **三次握手**：
    - Client将标志位SYN置1，随机产生一个seq=X，将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认
    - Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位ACK和SYN都置1，ack=X+1，随机产生一个seq=Y，将该数据包发送给Client确认连接请求，Server进入SYN_RECV状态
    - Client收到确认之后，检查ack，ACK是否为1，如果正确则将标志位ACK置1，ack=Y+1，将数据包发送给Server，Server检查ack是否为Y+1，ACK是否为1，正确则建立连接，Client和Server进入ESTABLEISHED，双方建立连接完成，可以进行数据传输
  - **四次挥手**：由于建立连接是全双工的，因此每个方向都必须单独进行关闭，当一方发送完数据，发送一个FIN来终止这个连接，只收到一个FIN知识代表着一个方向上没有数据流动，但是另一个方向上的连接还是可以发送数据，直到这一方也发送了FIN，
    - 数据传输结束，客户端的应用发出连接释放报文，停止发送数据，客户端进入FIN_WAIT_1，客户端仍然可以接收服务器发送的数据
    - 服务器接受到FIN之后，发送一个ACK给客户端，服务器进入CLOSE_WAIT状态，客户端收到之后进入FIN_WAIT_2状态，
    - 服务器没有数据发送，服务器发送一个FIN报文，服务器进入LAST_ACK，等待客户端的确认
    - 客户端收到FIN之后，给服务器发送一个ACK，客户端进入CLOSE_WAIT状态，等待2MSL，关闭连接
- TCP报文和IP报文，MTU在什么情况下会发生改变
  - 
- MAC帧的最大生成时间
- TCP的滑动窗口，拥塞控制
- DNS向服务器发送的是什么请求
- 服务的端口如何获取
- HTTP的长连接，短连接和死连接
  - 本质上是TCP的长连接和短连接
  - http/1.0中默认短连接，客户端和服务器每进行一次http请求操作就需要建立一次连接，任务结束就中断连接
  - http/1.1起默认使用长连接，保持连接特性，当一个网页打开完成之后，连接并不会断开，客户端再次进行访问时，使用的还是这一条连接
- 四次挥手FIN_WAIT_2一直持续怎么办
  - FIN_WAIT_2表示客户端正常接受到来自服务器对于FIN的ACK确认，表示客户端对于服务器没有数据发送
  - 但是没有接受到来自服务器的FIN，表示服务器还有数据发送给客户端，一直处于FIN_WAIT_2，可能是客户端应用程序没有将来自服务器的数据处理完毕
- ssh原理（RSA非对称加密，生成一个公钥个私钥）
- IP分包如何去分，mtu，局域网间和广域网
- Tcp连接建立成功后，保持高流量发送数据，突然client发送流量变成了几KB，分析一下原因
- Ip报文在经过路由时会改变哪些标志位 
- IP报文经过路由器的转发过程以及变化
  - 防火墙解析以太网帧头部，提取MAC地址，查看目的MAC地址是否是本机地址
  - 如果不是自己的MAC地址则丢弃，如果是自己的MAC地址就交给上层IP解析
  - 解析目的IP地址，判断是不是本机，如果是本机，交给上层传输层解析
  - 如果不是指向自己，查询路由表，匹配接口
  - 
- 什么时候会收到RST报文
  - 访问端口不存在，或者服务端处在TIME_WAIT状态下
  - 异常终止连接，一方直接发送RST报文，表示异常终止连接，发送端所有排队等待发送的数据都会被丢弃
  - 处理半打开连接，往未建立连接的对端写数据，会发送RST报文
- 大量处于CLOSE_WAIT状态怎么办，大量处于TIME_WAIT可能的原因，造成什么影响，如何解决？
  - CLOSE_WAIT表示服务器收到对方的FIN包被动关闭，发送了ACK，后面发送FIN进LAST_ACK，但是这中间还需要将部分数据发送出去，才能发送FIN，大量处于CLOSE_WAIT表示这些数据没有发送出去或者发送出去没有应答，由于自己是主动发数据的一方，所以肯定是客户端没有做出应答，所以判断应该是断开连接关闭socket，
    - **解决**：代码需要判断socket，如果读到0就断开连接，read返回负数，检查errno是否为EAGAIN
  - TIME_WAIT表示客户端收到对方的FIN请求并发送ACK，是一种主动关闭方才会存在的状态
- TCP如何保证可靠？
  - 序列号，确认应答，超时重传：
    - 数据到达接收方，接收方需要发出一个确认应答包，表示已经收到当前数据段，并且确认序号说明了下一次需要接受的数据序号，确认序号是对序号进行+1，如果长时间没有收到确认应答，可能是发送的数据包丢失，也可能是确认应答丢失，发送放在等待一段时间后就会进行重传，这个时间**查看Linux复习**
  - 滑动窗口和快速重传
    - TCP会利用窗口大小来提高传输速度，在一个窗口内，不一定要等到应答才发出下一段数据，窗口内的大小就是无需经过确认等待继而可以发送的的数据的最大值，如果不使用滑动窗口，每一个没有收到确认应答的数据都要重传
    - 使用滑动窗口，数据段丢失一小部分，后面每次数据传输，都会不停地发送序号为上一段的数据，表示我需要那部分数据，如果发送端收到三次相应的应答就会立即进行重发，还有一种情况是数据都受到了，但是应答丢失，这种情况不会进行重发，因为可以根据后面的确认序号来确定之前的发送报文
  - 拥塞控制：防止过多的数据注入网络，使得网络中的路由器或者链路过载，双方都有一个拥塞窗口--cwnd
    - 如果窗口大小很大，发送端发送大量的数据，可能会造成网络的拥堵，甚至造成网络的瘫痪，TCP为了防止这种情况进行拥塞控制
    - 慢启动：最开始发送方的拥塞窗口是1，由小到大逐渐增大，每经过一个传输，拥塞窗口就增大一倍，cwnd超过慢开始门限，就开始执行拥塞避免算法
    - 拥塞避免：每经过一个往返时间RTT，wcnd就增大1，一旦发现拥塞，就把慢开始门限设置为当前值的一半，并且从新设置cwnd为1，重新启动满启动
    - 快重传：接收方每收到一个失序报文就立即发出重复确认，遇到三次重复确认应答，不需要等待超时重传，立即进行转发
    - 快恢复：当发送方连续收到了三个重复确认，就乘法减半（慢开始门限减半），将当前的cwnd设置为慢开始门限，并且采用拥塞避免算法（连续收到了三个重复请求，说明当前网络可能没有拥塞）。 
- session和Cookie的区别  cookie的加密与安全性保证, cookie和session的生命周期,什么时候失效
  - session在服务器端，cookie在客户端以文本格式存储，并且有数量限制
  - session默认被存在服务器的一个文件中，或者数据库中
  - session的运行依靠session id，session id是存在cookie中，浏览器禁用cookie，session也会失效，可以通过url传递session id
  - token类似于一个令牌，无状态，用户信息都被加密到token中，uid+time+sign+固定参数
- **HTTP + 加密 + 认证 + 完整性保护 = HTTPS**
- HTTPS的握手建立过程，什么时候使用对称加密, 什么时候使用非对称加密
  - 通常HTTP都和TCP通信，使用SSL时，先和SSL通信，再由SSL和TCP通信
  - 加密和解密用同一个密钥的方式称为**共享秘钥加密**，也被称作对称秘钥加密
  - **公开秘钥加密**使用一对非对称的秘钥，发送密文的一方使用对方的公开密钥进行加密处理，收到被加密的信息之后使用私钥来进行解密，另外，想要根据公开秘钥以及密文获取到信息原文是基本不可能的
  - **HTTPS使用共享秘钥加密和公开秘钥加密两者并用的混合加密机制**，公开秘钥加密与共享秘钥加密相比其处理速度要慢许多
- HTTP2.0 和 HTTP1.X区别
  - **HTTP2使用二进制流传送**，HTTP1.X基于字符串即文本传送传送。相比之下更加安全可靠
  - **HTTP2支持多路复用**，HTTP1.X一个连接提交一个请求，HTTP2一个连接可以处理多个请求，即连接共享，每一个request都是用作连接共享机制，每一个request对应一个id，这样一个连接就可以有多个request，每个连接的request可以随机混杂，接收方可以根据request的id来处理不同的请求
  - **HTTP2支持头部压缩**：HTTP2支持头部压缩（gzip或者compress），并且在客户端与服务端之间各自维护一个cache索引表，只需要根据索引id就可以进行头部信息传输，减少头部容量，提升传输效率
    - HTTP协议不带有状态，每次请求都会带上附有信息，但是有好多信息都是重复的，浪费很多带宽
  - **HTTP2支持服务器推送**：服务端可以主动推送资源给客户端，避免客户端花费过多的时间来逐个请求资源，可以降低整个请求的相应时间
- HTTP1.X的区别
  - HTTP/0.9
    - 这个版本及其简单，只有一个GET请求，请求完毕就关闭连接
    - 只能响应html格式的字符串，不能响应别的格式
  - HTTP/1.0
    - 任何格式的内容都可以发送，包括图像，视频，二进制文件
    - 除了GET方法，还增加了POST和HEAD方法
    - 每次通信包括头信息，支持多字符集发送，多部分发送，支持数据压缩
    - 每次处理完毕就关闭请求连接
  - HTTP/1.1
    - 当前最流行版本，完善http协议
    - 引入持久连接，TCP默认不关闭连接，可以被多个请求复用
    - 客户端可以主动关闭连接，发送Connection： close明确要求服务器关闭连接
    - 引入管道机制：在同一个连接中可以同时发送多个请求
    - 增加其他方法： PUT， DELETE， HEAD， OPTIONS， PATCH
    - 缺点：允许复用连接，但是所有通信都是按序处理，要是前一个回应慢，后面的许多请求就会被阻塞，这就是队头阻塞
  - 1.1 与 1.0的一些区别
    - 缓存处理： 
    - 优化带宽以及网络连接：1.0中存在一些浪费现象，客户端只是需要一部分，但是服务器却将整个对象传送，不支持断点续传功能，1.1引入range头部，允许只请求资源的某一个部分，返回码是206
    - 错误处理通知：409表示请求资源与当前状态发生冲突，410表示服务器上的某一个资源被永久性的删除
    - Host头处理：1.0认为每一个服务器上都绑定唯一的一个ip地址，因此传递的url并没有传递主机名，随着虚拟主机的发展，一个物理机上可能存在多个虚拟主机，并且共享同一个ip地址，1.1支持host头部，
    - 长连接：
- HTTP请求方法和状态码，GET和POST的区别
  - HTTP1.0 定义了三种方法
    - GET：用于请求指定页面，返回实体主体
    - POST：向指定资源提交数据进行处理请求，
    - HEAD：类似于GET，返回的响应中没有具体的内容，用于获取报头
  - HTTP1.1中又新增了五种方法
    - POTIONS：
    - PUT：从客户端向服务器传送的数据取代指定的文档内容
    - PATCH：对PUT的补充，对已知资源进行局部更新
    - DELETE：请求服务器删除指定页面
    - TRACE：回显服务器收到的请求，用于测试和诊断
    - CONNETC：预留给能够将连接改为管道方式的代理服务器
  - GET和POST的区别
- HTTP响应码
  - 1XX：信息，服务器收到请求，需要请求者继续操作
    - 100：继续，客户端继续其请求
    - 101：切换协议，服务器根据客户端的请求切换协议，切换到更高版本的协议
  - 2XX：成功，操作被成功处理
    - 200：请求成功，一般用于GET和POST请求
    - 201：已经成功请求并且创建了新的资源
    - 202：已经接收请求，但是还未处理完成
    - 204：服务器成功处理，但是并没有返回内容
    - 205：重置内容，服务器处理成功，用户中断应该重置文档视图
    - 206：服务器成功处理部分GET请求
  - 3XX：重定向，需要进一步的操作来完成请求
    - 300：多种选择，请求的资源可以包括多个位置，可以返回一个列表供浏览器选择
    - 301：永久性重定向，请求的资源被永久的移动到新的url，返回的信息会包含新的url
    - 302：临时重定向，资源临时移动，客户端使用原来的url
    - 305：已经使用代理，请求的资源必须通过代理访问
    - 307：临时重定向，使用GET请求重定向
  - 4XX：客户端错误，请求中包含语法错误或者无法完成请求
    - 400：客户端请求语法错误，服务器无法理解
    - 401：要求用户的身份认证
    - 403：服务器拒绝执行此请求
    - 404：服务器无法找到请求资源
    - 405：客户端的请求方法被禁止
    - 408：服务器等待客户端发送的请求时间过长
    - 409：服务器处理请求发生状态冲突
    - 410：客户端请求资源被永久删除
    - 413：请求实体过大，服务器无法处理，拒绝请求，断开连接
    - 414：请求的url过长
  - 5XX：服务器错误，服务器在处理请求的过程中产生了错误
    - 500：服务器内部错误，无法完成请求
    - 501：服务器不支持请求功能，无法完成请求
    - 503：服务器超载或者处于维护状态
    - 504：充当网关或者代理的服务器没有及时从远端服务器获取请求
    - 505：服务器不支持请求的HTTP协议版本，无法完成处理请求

## 数据库/其他/Docker

- B树和B+树的区别
- 跳表是什么
- 搭建一个web服务，什么会影响web服务器的性能，如何提升并发性
- 服务器高峰时流量太高，服务器承受不住，如何解决？--暂想高并发
- 如果操作无法用原子性，一个线程不断的放任务，一个线程从其中拿请求，如何做到无锁编程
- MySQL索引底层的数据结构，索引存储结构和原理
- MySQL的内外连接，索引类型，乐观锁和悲观锁
- 主键和唯一索引的区别，事务的ACID特性，常见的加密算法，两个事务同时处理一行
- 什么是Docker
  - Docker是一个容器化的平台，以容器的形式将应用程序和所有依赖项进行打包，确保应用程序在任何环境下无缝运行
- CI（持续集成）服务器的功能
  - 在每次提交之后不断的集成所有提交到存储库的代码，编译检查错误
- 什么是Docker镜像
  - 是Docker容器的源代码，Docker镜像用于创建容器，
- 什么是Docker容器
  - Docker容器是包括应用程序及其依赖项，作为操作系统的独立进程运行
- Docker容器的状态
  - 运行
  - 暂停
  - 重新启动
  - 退出
- Docker使用流程
  - 使用Dockerfile构建镜像
  - 拉取推送镜像
- Dockerfile常见指令
  - FROM: 指定基础镜像
  - RUN: 运行指定命令
  - CMD: 容器启动是运行的命令
- Dockerfile中ADD与COPY的区别
- docker常用命令
- docker 隔离机制，挂载点，线程，内核调度
- docker内网IP段
- docker中的cgroup详解 
- docker限制流量, 防止用尽服务器资源
- 项目中创建进程对fork的优化（创建进程有消耗，fastcgi），阿帕奇内部如何做优化
- 简介协程概念, 协程如何实现?
- 协程和进程线程的区别
- 内存上面有什么区别
- 协程切换的时机
- 协程的应用场景
- 存储or队列
- MySQL一些基础操作
  - `SELECT prod_name FROM products ORDER BY pro_name`
  - `SELECT prod_id, prod_price, prod_name FROM products ORDER BY prod_price DESC, prod_name`
  - `SELECT prod_price FROM products ORDER BY prod_price DESC LIMIT 1`
  - `SELECT prod_name, prod_price DROM products WHERE prod_price = 2.5`
  - `SELECT prod_name, prod_price FROM products WHERE prod_price BETWEEN 5 AND 10`
  - `SELECT prod_name FROM products WHERE prod_price IS NULL`
  - `SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 AND prod_price <= 10`
  - `SELECT prod_name, prod_price FROM products WHERE(vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10`
  - `SELECT prod_name, prod_price FORM products WHERE vend_id IN (1002, 1003) ORDER BY prod_name`
  - ``SELECT prod_name, prod_price FORM products WHERE vend_id NOT IN (1002, 1003) ORDER BY prod_name`` 
  - `SELECT prod_id, prod_name FROM products WHERE prod_name LIKE '%jec%'`
  - `SELECT COUNT(*) AS num_prod FROM products WHERE vend_id = 1003`
  - `SELECT vend_id, COUNT(*) AS num_prods FROM products GROUP BY vend_id`, 分组
  - `SELECT cust_id, COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) >= 2`, WHERE过滤行，HAVING过滤分组，除此之外没有任何区别
  - 字句顺序: `SELECT, FORM, WHERE, GROUP BY, HAVING, ORDER BY, LIMIT`
  - 多表子查询
- 数据库隔离级别: 
- 数据库触发器
- 联合索引
- B树与B+树的区别

## 开放性问题/大数据

- 单例模式如何实现，单例有什么用处
- 工厂模式，画出UML图
- 服务器模型架构
- 100G文件，多个ip如何进行排序
- 有一个论文和一个很多论文你的库，如何设计一个查重系统
- 后台如何设计海量用户的在线实时通信，设计核心计技术方案
- 数组在扩容时，没有完整的大块空间，有若干小空间让我们使用，计算机如何扩容
- 100万个数据中找出重复数，不用辅助空间
- 对于一个已经有的分布式架构来说，下游机器发生拒绝，从哪些角度考虑设计一个拒绝原因追查平台
- 海量字符串查找
- 精简指令集和复杂指令集的区别，应用场景
- 程序设计：一个16G的文本，每一行都是一个无符号的长整形数字，均匀分布。要求：程序运行空间1G
- 服务端客户端模型和浏览器服务端模型做对比，分析其优劣，安全性. 
- 如何分析服务器系统性能低的原因，服务器负载均衡
- 设计支持很多数据库的同步备份系统
- muduo网络库，高并发网站设计，大量已经排序的数据进行合并说明优化思路
- 给定两个1T的文件在2G的内存中找出相同的项
- 如何解决高并发中大量互斥锁带来的巨大开销

## Redis

- redis的基本数据结构：字符串（string-sds），列表（list），hash，集合（set），有序集合（zset）

###底层数据结构

- 字符串实现：SDS（简单动态字符串）
  - 三个属性：len表示已经使用的空间，free表示还未使用的空间，buf表示内存空间，可以常数时间复杂度获得字符串长度
  - 在每次扩充之前都会检查字符串的空间，确保不会有字符串缓冲区溢出，会自动扩充SDS的空间
    - SDS空间预分配：减少连续执行字符串增长操作所需要的内存重分配次数
      - 对SDS进行修改之后，SDS的长度小于1M，那么程序分配和len一样的空间，即free == len
      - 对SDS进行修改之后，SDS的长度大于1M，会分配1M的空闲空间
    - SDS惰性删除：避免缩短字符串时所需要的内存重新分配操作
  - SDS是二进制安全的，可以正常存储二进制
  - 可以正常使用C库函数
- 链表（list）实现：普通链表
  - list结构为链表提供了表头指针head，表尾指针tail，以及链表长度计数器len，
  - 双端链表，获取前置和后置结点都是O(1)
  - 无环，表头结点的prev和表尾的next都指向NULL
  - 可以直接获取表头和表尾结点
  - 多态：链表结点使用void*指针来保存结点值，
- hash实现：字典
  - 哈希表结构：table是一个数组，指向dictEntry结构（保存键值对链表），size记录hash表的大小，即table数组的大小，used记录hash表目前已有结点的数量
  - 字典结构：type是指向操作特定类型键值对的函数，private指向特定函数（计算hash索引等等），哈希表ht[2]，rehashidx--没有在进行rehash，值为-1
  - 当一个新的键值对添加到字典中，程序先根据键计算出hash值和索引值
  - rehash步骤：为了扩展或者收缩
    - 为字典ht[1]分配空间，这个hash表的空间取决于要执行的操作，很据ht[0]所包含的键值对数量，即ht[0].used的值，
      - 执行扩展操作，ht[1]的大小为第一个大于等于ht[0].used*2
      - 执行扩展操作
    - 将保存在ht[0]中的所有键值对rehash到ht[1]上面，重新计算hash索引
    - 将ht[0]都迁移到ht[1]中时，释放ht[0]，ht[0] = ht[1], 为ht[1]重新简历新的空白hash表，为下一次rehash做准备
    - 扩展与收缩时机：
      - 没有执行BGSAVE或者BGREWRITEAOF，并且hash负载因子是大于等于1
      - 正在执行BGSAVE或者BGREWRITEAOF，并且hash负载因子大于等于5
      - 在执行上面命令时，服务器创建子进程实现，大多OS使用写时拷贝，在子进程存在期间，服务器提高执行扩展操作所需的负载因子，避免在子进程存在期间执行hash扩展
  - 渐进式rehash：避免rehash对服务器性能造成影响，多次rehash
    - 为ht[1]分配空间，让字典同时保持两个hash表
    - 设置字典rehash标志位rehashidx=0
    - 在rehash执行期间，每次对字典执行增删改查时，顺带将ht[0]表在rehashidx索引上的位置rehash到ht[1]，rehash完成之后，将rehashidx++
    - rehash过程不断进行，在完成ht[0]表rehash后，将rehashidx=-1
- 跳跃表（实现有序集合键）：支持平均O(logN)，最坏O(N)的复杂度结点查找
- 整数集合intset，是集合键的底层实现之一
- 压缩列表ziplist：列表键和哈希键的底层实现之一

## Nginx

- 以多进程模型运行，后台有一个master进程和多个worker进程，master进程主要用来管理worker进程，接收来自外界的信号，向worker进程发送信号，监控worker进程的运行状态，当worker进程异常退出后启动新的worker进程

- 每个worker进程都是从master进程中fork出来的，在master进程中，先建立好需要listen的socket，然后再fork出多个worker进程，当worker进程中的listenfd在连接到来时变得可读，为了保证只有一个worker进程处理连接，所有worker进程在注册listenfd读事件之前抢占accept_mutex锁，抢到互斥锁的那个进程注册listenfd读事件，在读事件中accept新的请求，然后开始度请求，处理请求，产生数据，发送请求

- 每个worker都是独立的进程，不需要加锁，省略了锁带来的开销，一个进程推出之后，其他进程还在工作，服务不会中断，

- nginx使用异步非阻塞处理请求，还是同步？？

- nginx中每个进程都会有一个链接数的最大上限，nginx设置worker_connectons设置每个进程的最大连接数，这里的worker_connectons并不是真是的连接，只是一个大小为worker_connectons的ngx_connection_t的数组，一个nginx能建立连接的最大连接数就是 worker_connectons * worker_processes

- 为了防止进程竞争连接资源，防止连接数过大的进程去竞争连接，nginx使用ngx_accept_disabled的变量去控制worker进程竞争accept_mutex，`ngx_accept_disabled = 单进程连接总数的1/8 - 空余连接数量`，当剩余连接小于总连接的1/8，这个值才大于0，并且剩余的空闲连接越小，这个值就越大，当这个值大于0，并不会去获得锁，而是将这个值减去1，当空余连接越小，这个值越大，让出的机会越多，其他进程的机会也就越大

  ```c
  ngx_accept_disabled = ngx_cycle->connection_n / 8
      - ngx_cycle->free_connection_n;
  
  if (ngx_accept_disabled > 0) {
      ngx_accept_disabled--;
  
  } else {
      if (ngx_trylock_accept_mutex(cycle) == NGX_ERROR) {
          return;
      }
  
      if (ngx_accept_mutex_held) {
          flags |= NGX_POST_EVENTS;
  
      } else {
          if (timer == NGX_TIMER_INFINITE
                  || timer > ngx_accept_mutex_delay)
          {
              timer = ngx_accept_mutex_delay;
          }
      }
  }
  ```

- **延迟关闭**： nginx在接收客户端的请求时，可能由于客户端或服务端出错了，要立即响应错误信息给客户端，而nginx在响应错误信息后，大分部情况下是需要关闭当前连接。nginx执行完write()系统调用把错误信息发送给客户端，write()系统调用返回成功并不表示数据已经发送到客户端，有可能还在tcp连接的write buffer里。接着如果直接执行close()系统调用关闭tcp连接，内核会首先检查tcp的read buffer里有没有客户端发送过来的数据留在内核态没有被用户态进程读取，如果有则发送给客户端RST报文来关闭tcp连接丢弃write buffer里的数据，如果没有则等待write buffer里的数据发送完毕，然后再经过正常的4次分手报文断开连接。所以,当在某些场景下出现tcp write buffer里的数据在write()系统调用之后到close()系统调用执行之前没有发送完毕，且tcp read buffer里面还有数据没有读，close()系统调用会导致客户端收到RST报文且不会拿到服务端发送过来的错误信息数据。那客户端肯定会想，这服务器好霸道，动不动就reset我的连接，连个错误信息都没有。 

  - 对于这种情况，只需要关闭写端（服务器不会再进行写入）就可以，读可以继续进行，最终丢掉读的任何数据就行，一段时间之后再设置读端关闭就行

- **keepalive**：

  - 对于body长度的确定：针对HTTP/1.0来说，有content-length就知道body的长度，但是如果没有这个头，就一直接收数据直到主动断开连接，针对HTTP/1.1来说，响应投为chunked传输，表示是流式传输，body会被分成多个块，每个块的开始会标示当前块的长度，不需要长度来指定，如果不是分块，那么就和1.0相同
  - 对于请求量比较大的nginx来说，关掉keepalive最后可能产生较多的time_wait状态，一般来说，当客户端，打开keepalive的可以有效减少time_wait的数量

- **pipe**：流水线作业，基于长连接，目的就是利用一个连接处理多个请求，客户端不必等待第一个请求处理完毕，可以直接发送后面的请求，但是服务器依旧是一个接一个的处理，nginx在读取数据时，会将读取到的数据放到一个缓冲区中，如果处理完一个请求之后还发现有数据存在，就认为是下一个请求的开始，开始处理请求

- **ngx_pool_t**：提供一种机制，帮助管理一系列资源，如内存和文件，使得对这些资源的使用和释放统一进行，避免资源泄露，内部管理资源的声明周期与这个结构体的声明周期一致，造成有些资源一直存在，无法及早被释放，nginx处理的每个http request都会产生一个ngx_pool_t类型的对象与这个请求request相关联，所有申请的资源都从ngx_pool_t里面申请，等到处理完毕就一起释放

- **ngx_hash_t**：不像其他hash的表现，可以插入删除元素，hgx_hash只能初始化一次，就构建起整个hash表，不能插入和删除元素，除此之外，桶中的结构也不是链表，而是类似于数组的一块连续内存，在初始化时会计算所用空间的大小，所以一段连续的内存足以，在一定程度上节约了内存的使用，但是这个初始化的值越大，内存浪费越严重

- **nginx模块分类**

  - **event module**：独立于操作系统的事件处理机制的框架，包括ngx_event_modul，ngx_event_core_module和ngx_epoll_module等，具体使用何种事件处理模块，依赖于操作系统和编译选项
  - **phase handler**：处理客户端请求并且产生响应内容，比如ngx_http_static_module负责客户端的静态页面请求处理并将对应的磁盘文件准备为响应内容作为输出
  - **output filter**：负责对输出的内容进行处理，可以对输出内容进行修改
  - **upstream**：实现反向代理功能，将真正的请求转发到后端服务器上，并从后端服务器读取响应，发回客户端，响应内容不是由自己产生
  - **load balancer**：负载均衡模块，实现特定算法，在众多的后端服务器中选出一个作为某个请求的转发服务器

- **nginx的处理请求**：worker进程中的`ngx_worker_process_cycle()`一直在循环执行处理函数，简单的处理流程

  - 操作系统提供的机制（epoll）产生的相关事件
  - 接收和处理事件，如果接收到数据，产生更高层的request对象
  - 处理request的header和body
  - 产生响应，发送给客户端
  - 完成request的处理
  - 重新初始化定时器和其他事件
  - **请求处理流程**
    - 初始化HTTP Request对象
    - 处理请求头
    - 处理请求体
    - 如果有的话，调用与此请求关联的handler
    - 一次调用各个phase handler进行处理

- 负载均衡的好处：优化资源利用率，最大化吞吐量，减少延迟，系统的伸缩性和可靠性也可以得到保障

- **Nginx负载均衡策略**：

  - 基于轮询的均衡策略：按照遍历方式进行分发
  - 基于最少连接数的均衡策略：nginx判断后端集群服务器那个Server当前的Active Connection数最少，那么新的连接就发送给这个
  - 基于ip-hash的均衡策略：每个请求客户端都有相应的ip地址，会根据有关hash函数对每个ip作为键得出hash值，根据得出的hash值将对应请求发送给对应server
  - 基于加权轮询的均衡策略：nginx为每个server配置相应的权值，权重越大，接收的request越多

- **nginx启动**

  - 时间、正则，错误日志，ssl等初始化
  - 读入命令行参数，OS相关初始化
  - 读入并解析配置，核心模块初始化
  - 创建各种暂时文件和目录，创建共享内存
  - 打开listen端口，所有模块初始化，启动worker进程

- **accept锁**

  - Nginx是多进程程序，端口被多个进程所共享，多个进程监听同一个端口势必会有竞争，也有所谓的“惊群效应”，当内核accept一个连接时会唤醒所有等待中的进程，但是实际上也只有一个进程可以获取连接，所以Nginx有自己的accept加锁机制，避免多个进程同时调用accept，Nginx多进程锁是使用CPU的自旋锁实现的，如果操作系统不支持自旋锁，就是用文件锁来实现
  - ngx_process_events是所有事件的处理入口，会遍历所有的事件，抢到了accept锁的进程会加上NGX_POST_EVENTS标志，在ngx_process_events函数里面只接收而不去处理事件，加入到post_events的队列中，去掉ngx_accept_mutet锁之后才去处理事件，因为accept锁是全局锁，这样做可以减少accept持有的时间，以便其他进程继续接收新的连接，提高吞吐量
  - 除了加锁，Nginx也对各个进程请求处理做了均衡性优化，在负载高的时候，进程抢到的锁过多，会导致这个进程被进制接收请求一段时间

- **高性能服务器设计**

- nginx事件模型处理器接收到这个读事件之后，会交给**ngx_event_accept**这个函数来处理：

  - 在这个函数中，nginx调用accept函数，从已连接队列中取得一个连接以及对应的套接字，接着再分配一个连接结构**ngx_connection_t**，并将得到的新的套接字保存在该连接的结构中，并作以下初始化工作
  - 分配一个内存池，初始化大小为256字节，可以设置，内存池使用申请不释放原则，等到最后一起释放
  - 分配日志结构，保存在其中，让后续的日志系统使用
  - 初始化连接相应的io收发函数，具体的io收发函数和使用的事件模型及操作系统相关
  - 分配一个套接字地址，并将accept得到的对端地址拷贝在其中
  - 将连接的写事件设置为已经就绪，默认连接第一次为可写
  - 



## 分布式

- 什么是分布式：分布式是由一组通过网络进行通信，为了完成共同的任务而协调工作的计算机结点组成的系统，分布式的出现是为了用廉价、普通的计算机完成单个计算机无法完成的计算，存储任务。目的是为了利用更多的机器，处理更多的数据
