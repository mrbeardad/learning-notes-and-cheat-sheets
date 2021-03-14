<!-- entry begin: cpp  预处理 -->
# 预处理
* 头文件保护
* 宏开关
<!-- entry end -->

<!-- entry begin: cpp  泛型 -->
# 泛型编程
* 抽离
    * 模板类型无关
    * 算数类型
    * 指针类型
<!-- entry end -->

<!-- entry begin: cpp  函数 -->
# 函数设计
```
GFM
    ? W
        ? tempT&&
        : const tempT&
    : !CWD
        ? T
        : W
            ? T& -> T&&
            : cT& -> T&  -> T&&
// 注：如果函数内部必定会copy一次形参，则可直接使用传值参数(T)
```
* 修饰
    * this
    * const
    * noexcept
    * unamedspace
    * extern
    * inline
    * constexpr
    * auto return
    * =delete

* 使用`temp T&&`时注意使用`std::decay_t<T>`与`std::forward<T>(t)`
* 使用`T&&`与`T`时注意使用`std::move(t)`
* 异步/延迟调用的函数注意引用参数的**生命周期**!
<!-- entry end -->

<!-- entry begin: cpp  类设计 -->
# 类的设计
* 取消友元
* 成员对象
    * handle
    * private
    * 顺序 & 对齐
    * const & 引用
* 构造
    * default?
    * explicit?
    * non-inline
    * never-call-virtual
* 析构
    * noexcept & .destroy()
    * virutal & definition
    * nonempty-non-inline
    * never-call-virtual
* copy? & move?
* operator
    * 单成
    * 算赋
    * 前后
    * this
    * 构造转换 > operator转换
    * explicit bool 1
<!-- entry end -->

<!-- entry begin: cpp 类间关系 -->
# 类间关系
* **is-a**：public继承
    * 抽象分化
    * `static<>`
* **has-a**：复合
* **impl-of**：复合或private继承
    * virtual
    * protect
    * EBO

* pure virtual      ：无默认定义
* non-pure virtual  ：提供默认定义
* non-virtual       ：提供强制定义
<!-- entry end -->

<!-- entry begin: cpp  初始化 -->
# 初始化
* 引用：
    * 可读性别名
* 指针
    * 需要NULL
    * 更改指向
* 形式
    * `auto = cast<>()`
    * `auto& = initializer`
    * `T t{}`
    * `T t()`
* 延后立初
* 循环声明
* 多个翻译单元中引用静态变量应利用reference-return技术
<!-- entry end -->

<!-- entry begin: cpp  异常 -->
# 异常
* 限制输入
    > 整数(U负溢出、-Tmin、除零)
    > 浮点数(有效、结合、比较)
* 代码前移
* 集中捕获可恢复异常
<!-- entry end -->

<!-- entry begin: cpp  并发 -->
# 并发
* atomic
* volatile
* mutable
<!-- entry end -->

<!-- entry begin: cpp  STL -->
# STL
* 优先使用可读性更好的重载`operator`
* Stream在循环中使用一定不要忘记调用`clear()`
* Stream仅持有StreamBuf的指针而非完整对象，不保证其生命周期
* `stringstream`只在需要格式化时才用来替代`string`
* Container优先使用`emplace`函数而非`push`函数
* Container引用元素时，若下标为非常量，则应使用`at()`代替`operator[]()`
* Container增删元素时，注意range-based-for、引用、指针、迭代器的有效性
<!-- entry end -->

<!-- entry begin: logic  循环 -->
# 循环
* 多，顺
* 变，迭，if-continue
* 条，相，穷，if-break
* 记，操，初，once
* 尾
* 终
> 循环计数器：
> * 迭后，则初始状态为N，计数N步，最后状态+1则为N-N==0
> * 迭前，则初始状态为N-1，计数N步，最后状态则为N-1-N+1==0
<!-- entry end -->

<!-- entry begin: logic  分支 条件 -->
# 分支
* if-if
* if-else if-continue if-break if-return if-exit
* if-elif-else  `<=>` if-else{if-else}
* if{for}       ` =>` for
* 多if `=>` switch `=>` 跳转表（数组） `=>`跳转表（hash表）
<!-- entry end -->

<!-- entry begin: logic  递归 -->
# 递归
* 明确功能：参数，返回值，所有副作用
* 一般进展：利用递归获取下层结果，与本层计算结合得到本层结果
* 基准情况：结束递归，并符合上述功能
* 技巧：
    * 将尾递归转为循环
    * 用`stack`存储递归路径
<!-- entry end -->

# 步数
获取数量：
* `[X0, Xn]`
* Xn - X0 + 1

获取距离：
* `(X0, Xn]`
* Xn - X0

获取尾后向量：
* Xn + 数量

获取尾部向量：
* Xn + 距离