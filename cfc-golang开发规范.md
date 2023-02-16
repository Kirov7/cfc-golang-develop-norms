# CFC-Golang 开发规范

> 注：此开发规范整合了部分网络上有价值的参考意见和开发实践中的总结

## 为什么需要编程规范？

编程规范又叫代码规范，是团队之间在程序开发时需要遵守的约定。俗话说，无规矩不成方圆，一个开发团队应该就一种编程规范达成一致。编程规范有很多好处，我们简单说几个最主要的。

- 促进团队合作

现代项目大多是由团队完成的，但是如果每个人书写出的代码风格迥异，最后集成代码时很容易杂乱无章、可读性极差。相反，风格统一的代码将大大提高可读性，易于理解，促进团队协作。

- 规避错误

每一种语言都有容易犯的错误，Go语言也不例外。但是编码规范可以规避掉像Map并发读写等问题。不仅如此，规范的日志处理、错误处理还能够加快我们查找问题的速度。

- 提升性能

优秀的开发者，能够在头脑中想象出不同程序运行的过程和结果，写出高性能的程序非常考验开发者的内功。但每个人的水平都有差异，这一点并不可控。但是如果我们将高性能编程的常见手段归纳整理出来，开发者只需要遵守这些简单的规则，就能够规避性能陷阱、极大提升程序性能。

- 便于维护

我们习惯于关注编写代码的成本，但实际上维护代码的成本要高得多。大部分的项目是在前人的基础上完成开发的。我们在开发代码的时候，也会花大量时间阅读之前的代码。符合规范的代码更容易上手维护、更少出现牵一发动全身的耦合现象、也更容易看出业务处理逻辑。

知道了编程规范的好处，那我们应该规范什么内容呢？这其实涉及到我们对好代码的定义。针对这个问题，每个人都能够说个几句。但总体来说，好的代码首先是整洁、一致的，同时它还是高效、健壮和可扩展的。

有一些规范可以是强制的，因为我们可以通过工具和代码review强制要求用户遵守，还有一些规范是建议的，因为它更具有灵活性，很难被约束。在后面的规范中，[强制 xxx]中的“xxx”代表的就是可强制检查的工具。

## 整洁、一致

好代码的第一个要求，是整洁和一致。有一句话是这样说的：

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

它的意思是，任何傻瓜都可以编写计算机可以理解的代码，而优秀的程序员编写的是人类可以理解的代码。

如果我们的代码看起来乱七八糟，就像喝醉的人写的那样，这样不严谨的代码让我们有理由相信，项目的其他各个方面也隐藏着对细节的疏忽，并埋下了重大的隐患。

阅读整洁的代码就像看到精心设计的手表或汽车一样赏心悦目，因为它凝聚了团队的智慧。

阅读整洁的代码也像读到的武侠小说，书中的文字被脑中的图像取代，你看到了角色，听到了声音，体验到了悲怆和幽默。

但是，明白什么是整洁的代码并不意味着你能写出整洁的代码。就好像我们知道如何欣赏一幅画不意味着我们能成为画家。

整洁的代码包括对格式化、命名、函数等细节的密切关注，更需要在项目中具体实践。接下来我们就来看看整洁代码关注的这些细节和最佳的实践。

### **格式化**

**代码长度**

代码应该有足够的垂直密度，能够肉眼一次获得到更多的信息。同时，单个函数、单行、单文件也需要限制长度，保证可阅读性和可维护性。

[强制 lll] 一行内不超过 120 个字符，同时应当避免刻意断行。如果你发现某一行太长了，要么改名，要么调整语义，往往就可以解决问题了。

[强制 funlen] 单个函数的行数不超过 40 行，过长则表示函数功能不专一、定义不明确、程序结构不合理，不易于理解。当函数过长时，可以提取函数以保持正文小且易读。

[强制] 单个文件不超过 2000 行，过长说明定义不明确，程序结构划分不合理，不利于维护。

**代码布局**

我们先试想一篇写得很好的报纸文章。在顶部，你希望有一个标题，它会告诉你故事的大致内容，并影响你是否要阅读它。文章的第一段会为你提供整个故事的概要、粗略的概念，但是隐藏了所有细节。继续向下阅读，详细信息会逐步增加。

[建议]  Go 文件推荐按以下顺序进行布局。

1. 包注释：对整个模块和功能的完整描述，写在文件头部

2. Package：包名称

3. Imports：引入的包

4. Constants：常量定义

5. Typedefs：类型定义

6. Globals：全局变量定义

7. Functions：函数实现

每个部分之间用一个空行分割。每个部分有多个类型定义或者有多个函数时，也用一个空行分割。示例如下：

```go
/*
注释
*/
package http

import (
 "fmt"
 "time"
)

const (
 VERSION = "1.0.0"
)
type Request struct{
}

var msg = "HTTP success"

func foo() {
 //...
}
```

[强制 goimports] 当 import 多个包时，应该对包进行分组。同一组的包之间不需要有空行，不同组之间的包需要一个空行。标准库的包应该放在第一组。

**空格与缩进**

为了让阅读代码时视线畅通，自上而下思路不被打断，我们需要使用一些空格和缩进。

空格是为了分离关注点，将不同的组件分开。缩进是为了处理错误和边缘情况，与正常的代码分隔开。

较常用的有下面这些规范：

[强制 gofmt] 注释和声明应该对齐。示例如下：

```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```

[强制 gofmt] 小括号()、中括号[]、大括号{} 内侧都不加空格。

[强制 gofmt] 逗号、冒号（slice中冒号除外）前都不加空格，后面加 1 个空格。

[强制 gofmt] 所有二元运算符前后各加一个空格，作为函数参数时除外。例如`b := 1 + 2`。[强制 gofmt] 使用 Tab 而不是空格进行缩进。

[强制 nlreturn] return前方需要加一个空行，让代码逻辑更清晰。

[强制 gofmt] 判断语句、for语句需要缩进1个 Tab，并且右大括号`}`与对应的 if 关键字垂直对齐。例如：

```go
if xxx {

} else {

}
```

[强制 goimports] 当 import 多个包时，应该对包进行分组。同一组的包之间不需要有空行，不同组之间的包需要一个空行。标准库的包应该放在第一组。这同样适用于常量、变量和类型声明：

```go
import (
    "fmt"
    "hash/adler32"
    "os"

    "appengine/foo"
    "appengine/user"
    "github.com/foo/bar"
    "rsc.io/goversion/version"
)
```

[推荐] 避免 else 语句中处理错误返回，避免正常的逻辑位于缩进中。如下代码实例，else中进行错误处理，代码逻辑阅读起来比较费劲。

```go
if something.OK() {
        something.Lock()
        defer something.Unlock()
        err := something.Do()
        if err == nil {
                stop := StartTimer()
                defer stop()
                log.Println("working...")
                doWork(something)
                <-something.Done() // wait for it
                log.Println("finished")
                return nil
        } else {
                return err
        }
} else {
        return errors.New("something not ok")
}
```

如果把上面的代码修改成下面这样会更加清晰：

```go
if !something.OK() {
    return errors.New("something not ok")
}
something.Lock()
defer something.Unlock()
err := something.Do()
if err != nil {
    return err
}
stop := StartTimer()
defer stop()
log.Println("working...")
doWork(something)
<-something.Done() // wait for it
log.Println("finished")
return nil
```

[推荐] 函数内不同的业务逻辑处理建议用单个空行加以分割。

[推荐] 注释之前的空行通常有助于提高可读性——新注释的引入表明新思想的开始。



### **命名**

> Good naming is like a good joke. If you have to explain it, it’s not funny.
>                                                                               ———Dave Cheney

一个好的名字应该满足几个要素：

- 短，容易拼写；
- 保持一致性；
- 意思准确，容易理解，没有虚假和无意义的信息。

例如，像下面这样的命名就是让人迷惑的：

```go
int d; // elapsed time in days
```

[强制 revive] Go中的命名统一使用驼峰式、不要加下划线。

[强制 revive] 缩写的专有名词应该大写，例如： ServeHTTP、IDProcessor。

[强制] 区分变量名应该用有意义的名字，而不是使用阿拉伯数字：a1, a2, .. aN。

[强制] 不要在变量名称中包含你的类型名称。

[建议]变量的作用域越大，名字应该越长。

现代 IDE 已经让更改名称变得更容易了，巧妙地使用IDE的功能，能够级联地同时修改多处命名。

**包名**

包名应该简短而清晰。

[强制] 使用简短的小写字母，不需要下划线或混合大写字母。

[建议]  合理使用缩写，例如：

```
strconv（字符串转换）
syscall（系统调用）
fmt（格式化的 I/O）
```

[强制] 避免无意义的包名，例如`util`,`common,base`等。

**接口命名**

[建议]单方法接口由方法名称加上 -er 后缀或类似修饰来命名。例如：`Reader`, `Writer`, `Formatter`, `CloseNotifier` ，当一个接口包含多个方法时，请选择一个能够准确描述其用途的名称（例如：net.Conn、http.ResponseWriter、io.ReadWriter）。

**本地变量命名**

[建议]尽可能地短。在这里，i 指代 index，r 指代 reader，b 指代 buffer。

例如，下面这段代码就可以做一个简化：

```go
for index := 0; index < len(s); index++ {
	//
}
```

可以替换为：

```go
for i := 0; i < len(s); i++ {
	//
}
```

**函数参数命名**

[建议]如果函数参数的类型已经能够看出参数的含义，那么函数参数的命名应该尽量简短：

```go
func AfterFunc(d Duration, f func()) *Timer
func Escape(w io.Writer, s []byte)
```

[建议]如果函数参数的类型不能表达参数的含义，那么函数参数的命名应该尽量准确：

```go
func Unix(sec, nsec int64) Time
func HasPrefix(s, prefix []byte) bool
```

**函数返回值命名**

[建议] 对于公开的函数，返回值具有文档意义，应该准确表达含义，如下所示：

```go
func Copy(dst Writer, src Reader) (written int64, err error)

func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)
```

**可导出的变量名**

[建议] 由于使用可导出的变量时会带上它所在的包名，因此，不需要对变量重复命名。例如bytes包中的ByteBuffer替换为Buffer，这样在使用时就是bytes.Buffer，显得更简洁。类似的还有把strings.StringReader修改为strings.Reader，把**errors.NewError 修改为errors.New。**

**Error值命名**

[建议] 错误类型应该以Error结尾。

[建议] Error变量名应该以Err开头。

```go
type ExitError struct {
    ...
}
var ErrFormat = errors.New("image: unknown format")
```



### **函数**

[强制 cyclop] 圈复杂度（Cyclomatic complexity）<10。

[强制 gochecknoinits] 避免使用init函数。

[强制 revive] Context 应该作为函数的第一个参数。

[强制] 正常情况下禁用unsafe。

[强制] 禁止return裸返回，如下例中第一个return：

```go
func (f *Filter) Open(name string) (file File, err error) {
	for _, c := range f.chain {
		file, err = c.Open(name)
		if err != nil {
			return
		}
	}
	return f.source.Open(name)
}
```

[强制] 不要在循环里面使用defer，除非你真的确定defer的执行流程。

[强制] 对于通过:=进行变量赋值的场景，禁止出现仅部分变量初始化的情况。例如在下面这个例子中，f函数返回的res是初始化的变量，但是函数返回的err其实复用了之前的err：

```go
var err error
res,err := f()
```

[建议] 函数返回值大于 3 个时，建议通过 struct 进行包装。

[建议] 函数参数不建议超过 3 个，大于 3 个时建议通过 struct 进行包装。



### 控制结构

[强制] 禁止使用goto。

[强制 gosimple] 当一个表达式为 bool 类型时，应该使用 expr 或 !expr 判断，禁止使用 == 或 != 与 true / false 比较。

[强制 nestif] if 嵌套深度不大于5。



### **方法**

[强制 revive] receiver 的命名要保持一致，如果你在一个方法中将接收器命名为 "c"，那么在其他方法中不要把它命名为 "cl"。

[强制] receiver 的名字要尽量简短并有意义，禁止使用 this、self 等。

```go
func (c Client) done() error {
 // ...
}
func (cl Client) call() error {
 // ...
}
```



### 注释

Go提供C风格的注释。有/**/ 的块注释和 // 的单行注释两种注释风格。注释主要有下面几个用处。

1. 注释不仅仅可以提供具体的逻辑细节，还可以提供代码背后的意图和决策。
2. 帮助澄清一些晦涩的参数或返回值的含义。一般来说，我们会尽量找到一种方法让参数或返回值的名字本身就是清晰的。但是当它是标准库的一部分时，或者在你无法更改的第三方库中，一个清晰的注释会非常有用。
3. 强调某一个重要的功能。例如，提醒开发者修改了这一处代码必须连带修改另一处代码。

总之，好的注释给我们讲解了what、how、why，方便后续的代码维护。

[强制] 无用注释直接删除，无用的代码不应该注释而应该直接删除。即使日后需要，我们也可以通过Git快速找到。

[强制] 使用行注释而不是尾注释，注释一律写在所描述内容的上一行。

[强制] 统一使用中文注释，中英文字符之间严格使用空格分隔。

```go
// 从 Redis 中批量读取属性，对于没有读取到的 id ， 记录到一个数组里面，准备从 DB 中读取
```

[强制] 注释不需要额外的格式，例如星号横幅。

[强制] 包、函数、方法和类型的注释说明都是一个完整的句子，以被描述的对象为主语开头。Go源码中都是这样的。

示例如下：

```go
// queueForIdleConn queues w to receive the next idle connection for w.cm.
// As an optimization hint to the caller, queueForIdleConn reports whether
// it successfully delivered an already-idle connection.
func (t *Transport) queueForIdleConn(w *wantConn) (delivered bool)
```

[强制] Go语言提供了[文档注释工具go doc](https://tip.golang.org/doc/comment)，可以生成注释和导出函数的文档。文档注释的写法可以参考文稿中的链接。

[强制 godot] 注释最后应该以句号结尾。

[建议] 当某个部分等待完成时，可用 `TODO:` 开头的注释来提醒维护人员。

[建议] 大部分情况下使用行注释。块注释主要用在包的注释上，不过块注释在表达式中或禁用大量代码时很有用。

[建议] 当某个部分存在已知问题需要修复或改进时，可用 `FIXME:` 开头的注释来提醒维护人员。

[建议] 需要特别说明某个问题时，可用 `NOTE:` 开头的注释。



### 结构体

[强制] 不要将 Context 成员添加到 Struct 类型中。



### Commit 规范

**commit message格式**

```text
<type>(<scope>): <subject>
```

[强制] **type(必须)**

用于说明git commit的类别，只允许使用下面的标识。

feat：新功能（feature）。

fix/to：修复bug，可以是QA发现的BUG，也可以是研发自己发现的BUG。

- fix：产生diff并自动修复此问题。适合于一次提交直接修复问题
- to：只产生diff不自动修复此问题。适合于多次提交。最终修复问题提交时使用fix

docs：文档（documentation）。

style：格式（不影响代码运行的变动）。

refactor：重构（即不是新增功能，也不是修改bug的代码变动）。

perf：优化相关，比如提升性能、体验。

test：增加测试。

chore：构建过程或辅助工具的变动。

revert：回滚到上一个版本。

merge：代码合并。

sync：同步主线或分支的Bug。

[建议] **scope(可选)**

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

例如在Angular，可以是location，browser，compile，compile，rootScope， ngHref，ngClick，ngView等。如果你的修改影响了不止一个scope，你可以使用*代替。

[强制] **subject(必须)**

subject是commit目的的简短描述，不超过50个字符。

建议使用中文（感觉中国人用中文描述问题能更清楚一些）。

- 结尾不加句号或其他标点符号。
- 根据以上规范git commit message将是如下的格式：

```text
fix(DAO):用户查询缺少username属性 
feat(Controller):用户查询接口开发
```



> 这样规范 git commit 有以下优点

- 便于开发者对提交历史进行追溯，了解发生了什么情况。
- 一旦约束了commit message，意味着我们将慎重的进行每一次提交，不能再一股脑的把各种各样的改动都放在一个git commit里面，这样一来整个代码改动的历史也将更加清晰。
- 格式化的commit message才可以用于自动化输出Change log。



## 高效

[强制] Map在初始化时需要指定长度`make(map[T1]T2, hint)`。

[强制] Slice在初始化时需要指定长度和容量`make([]T, length, capacity)`。

我们来看下下面这段程序，它的目的是往切片中循环添加元素。

```go
func createSlice(n int) (slice []string) {
   for i := 0; i < n; i++ {
      slice = append(slice, "I", "love", "go")
   }
   return slice
}
```

从功能上来看，这段代码没有问题。但是，这种写法忽略了一个事实，如下图所示，往切片中添加数据时，切片会自动扩容，Go运行时会创建新的内存空间并执行拷贝。

![image-20221124144357089](https://img-1307890592.cos.ap-chengdu.myqcloud.com/typroa/image-20221124144357089.png)

自动扩容显然是有成本的。在循环操作中执行这样的代码会放大性能损失，减慢程序的运行速度。性能损失的对比可参考[这篇文章](https://github.com/uber-go/guide/blob/master/style.md#prefer-specifying-container-capacity)。我们可以改写一下上面这段程序，在初始化时指定合适的切片容量：

```go
func createSlice(n int) []string {
   slice := make([]string, 0, n*3)
   for i := 0; i < n; i++ {
      slice = append(slice, "I", "love", "go")
   }
   return slice
}
```

这段代码在一开始就指定了需要的容量，最大程度避免了内存的浪费。同时，运行时不需要再执行自动扩容操作，加速了程序的运行。

[强制]  time.After()在某些情况下会发生泄露，替换为使用Timer。

[强制] 数字与字符串转换时，[使用strconv，而不是fmt](https://gist.github.com/evalphobia/caee1602969a640a4530)。

[强制] 读写磁盘时，使用[读写buffer](https://www.instana.com/blog/practical-golang-benchmarks/#file-i-o)。

[建议] 谨慎使用Slice的截断操作和append操作，除非你知道下面的代码输出什么：

```go
x := []int{1, 2, 3, 4}
y := x[:2]
fmt.Println(cap(x), cap(y))
y = append(y, 30)
fmt.Println("x:", x)
fmt.Println("y:", y)
```

[建议] 任何书写的协程，都需要明确协程什么时候退出。

[建议] 热点代码中，内存分配复用内存可以使用 sync.Pool [提速](https://www.instana.com/blog/practical-golang-benchmarks/#object-creation)。

[建议] 将频繁的字符串拼接操作（+=），替换为**StringBuffer 或 StringBuilder。**

[建议] 使用正则表达式重复匹配时，利用Compile提前编译[提速](https://www.instana.com/blog/practical-golang-benchmarks/#regular-expressions)。

[建议]  当程序严重依赖Map时，Map的Key使用int而不是string将[提速](https://www.instana.com/blog/practical-golang-benchmarks/#map-access)。

[建议]  多读少写的场景，使用读写锁而不是写锁将提速。



## 健壮性

[强制] 除非出现不可恢复的程序错误，否则不要使用 panic 来处理常规错误，使用 error 和多返回值。

[强制] 永远只在 main 函数和 init 函数中调用 log.Fatal() 方法。

[强制] 永远先关闭 写channel 再关闭 读channel，否则对已关闭 写channel 进行写入时会引发 panic 。

[强制 [revive](https://revive.run/r#error-strings)] 错误信息不应该首字母大写（除专有名词和缩写词外），也不应该以标点符号结束。因为错误信息通常在其他上下文中被打印。

[强制 [errcheck](https://golangci-lint.run/usage/linters/#errcheck)] 不要使用 _ 变量来丢弃 error。如果函数返回 error，应该强制检查。

[强制] 在 Release模式 使用 context 的时候需要设定超时时间。

[建议] 在处理错误时，如果我们逐层返回相同的错误，那么在最后日志打印时，我们并不知道代码中间的执行路径。例如找不到文件时打印的`No such file or directory`，这会减慢我们排查问题的速度。因此，在中间处理err时，需要使用fmt.Errorf 或[第三方包](https://godoc.org/github.com/pkg/errors)给错误添加额外的上下文信息。像下面这个例子，在fmt.Errorf中，除了实际报错的信息，还加上了授权错误信息`authenticate failed` ：

```go
func AuthenticateRequest(r *Request) error {
        err := authenticate(r.User)
        if err != nil {
                return fmt.Errorf("authenticate failed: %v", err)
        }
        return nil
}
```

当有多个错误需要处理时，可以考虑将fmt.Errorf放入defer中：

```go
func DoSomeThings(val1 int, val2 string) (_ string, err error) {
    defer func() {
        if err != nil {
            err = fmt.Errorf("in DoSomeThings: %w", err)
        }
    }()
    val3, err := doThing1(val1)
    if err != nil {
        return "", err
    }
    val4, err := doThing2(val2)
    if err != nil {
        return "", err
    }
    return doThing3(val3, val4)
}
```

[强制] 利用recover捕获panic时，需要由defer函数直接调用。

例如，下面例子中的panic是可以被捕获的：

```go
package main

import "fmt"

func printRecover() {
    r := recover()
    fmt.Println("Recovered:", r)
}

func main() {
    defer printRecover()

    panic("OMG!")
}
```

但是下面这个例子中的panic却不能被捕获：

```go
package main

import "fmt"

func printRecover() {
	r := recover()
	fmt.Println("Recovered:", r)
}

func main() {
	defer func() {
		printRecover()
	}()

	panic("OMG!")
}
```

[强制] 不用重复使用recover，只需要在每一个协程的最上层函数拦截即可。recover只能够捕获当前协程，而不能跨协程捕获panic，下例中的panic就是无法被捕获的。

```go
package main

import "fmt"

func printRecover() {
	r := recover()
	fmt.Println("Recovered:", r)
}

func main() {
	defer printRecover()
	go func() {
		panic("OMG!")
	}()
	// ...
}
```

[强制] 有些特殊的错误是recover不住的，例如Map的并发读写冲突。这种错误可以通过race工具来检查。



## 扩展性

[建议] 利用接口实现扩展性。接口特别适用于访问外部组件的情况，例如访问数据库、访问下游服务。另外，接口可以方便我们进行功能测试。关于接口的最佳实践，需要单独论述。

[建议] 使用功能选项模式(options 模式)对一些公共API的构造函数进行扩展，大量第三方库例如gomicro、zap等都使用了这种策略。

```go
db.Open(addr, db.DefaultCache, zap.NewNop())
可以替换为=>
db.Open(
	addr,
	db.WithCache(false),
	db.WithLogger(log),
)
```



## 内部实践

### Gin

#### 项目启动

[强制] 在大型的项目开发中，采用优雅服务启停设计，采用 httpServer.ListenAndServer() 方法进行web服务的启动，而不是 Gin 框架的方法，web服务需额外启动一个 goroutine 来运行，通过信号监听的方式控制结束程序。

```golang
func main() {
    r := gin.Default()
    srv := &http.Server{
        Addr:    ":80",
        Handler: r,
    }

    // 优雅的启停
    go func() {
        log.Printf("web server running in %s \n", srv.Addr)
        if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalln(err)
        }
    }()

    quit := make(chan os.Signal)
    // SIGINT 用户发送 INTR 字符(Ctrl + C)触发 kill -2
    // SIGTERM 结束程序 (可以被捕获、阻塞或忽略)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit
    log.Println("Shutting Down project Web server...")

    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()
    if err := srv.Shutdown(ctx); err != nil {
        log.Fatalln("Web server shutdown, cause by : ", err)
    }
    select {
    case <-ctx.Done():
        log.Println("wait timeout...")
    }
    log.Println("Web Server stop success...")
}
```
> 以上代码可进一步封装，以保证 main 的代码结构清晰



#### 返回值格式

[强制] json格式的返回值，code部分统一采用 net/http 包中的 http.StatusOK，obj 则为业务对象的统一格式封装。

```golang
ctx.JSON(http.StatusOK, result.Success(list))
ctx.JSON(http.StatusOK, result.Fail(http.StatusBadRequest, "参数错误"))
```

[强制] 业务对象中的统一字段为：code、msg、data，其中code为错误代码，由具体的服务实现方定义，需要保证全局唯一，如果没有错误则固定为 200 ，msg为错误信息，由具体的服务实现方定义，需要简洁的描述错误的原因，错误可统一定义为全局变量，当在api层产生参数错误、数据校验失败等请求值错误，则使用 http.StatusBadRequest 作为错误码， data 则为前端所需要的业务对象，推荐使用在api服务中封装返回值结构体实例。

对应结构体实例：

```golang
type BusinessCode int
type Result struct {
	Code BusinessCode `json:"code"`
	Msg  string       `json:"msg"`
	Data any          `json:"data"`
}
```

[强制] 在给前端返回 json 后必须 return 。



### grpc

#### proto文件与命名规范

[强制] proto文件中的 service、rpc Function、message 以大驼峰的方式命名。

[推荐] 尽可能的将 proto 文件中 service 命名为 xxxService 、 RPC 方法的入参命名为 XxxRequest，出参命名为 XxxResponse。

[强制] 将引用的生成 rpc 包重命名为 xxxRpc 以避免module中的本地模块产生冲突。

```go
import(
	departmentRpc "github.com/Kirov7/project-grpc/project/department"   
)
```

[推荐] 编译 proto 文件时推荐不要直接生成到会被其他模块引用的包下，建议生成到指定目录后手动迁移文件。



#### 注册grpc服务

[建议] 优雅的启动 grpcServer 示例，其中 RegisterGrpc() 返回的 grpc.Server 可供上层调用 grpcSever.Stop() 等，可对 grpcServer 生命周期进行管控。

```golang
type gRPCConfig struct {
	Addr         string
	RegisterFunc func(server *grpc.Server)
}

func RegisterGrpc() *grpc.Server {
   	c := gRPCConfig{
    	Addr: config.AppConf.GrpcConfig.Addr,
      	RegisterFunc: func(g *grpc.Server) {
        	login.RegisterLoginServiceServer(g, loginServiceV1.NewLoginService())
      },
   	}
   	//cacheInterceptor := interceptor.NewCacheInterceptor()
   	//s := grpc.NewServer(cacheInterceptor.CacheInterceptor())
   	s := grpc.NewServer()
   	c.RegisterFunc(s)
   	listen, err := net.Listen("tcp", c.Addr)
   	if err != nil {
    	log.Printf("listen port %s fail\n", c.Addr)
   	}
   	go func() {
    	log.Printf("grpc server started as %s \n", c.Addr)
      	err = s.Serve(listen)
      	if err != nil {
        	log.Printf("server started error: %s\n", err)
        	return
      	}
   	}()
    return s
}
```



### 项目结构核心部分示例

此示例结合了部分DDD领域驱动设计的思想，但并非完全按照DDD的模式进行组织，旨在优化项目组织结构的同时，尽量避免过于复杂抽象的概念。

#### main.go 

进行各类初始化操作，如初始化http服务、路由注册、初始化grpc客户端、grpc服务注册等。

#### config 包

内有 config.go 和 config.yaml 文件，config.go 中拥有包含配置的全局变量，**推荐使用 viper 进行配置读取**。

#### internal 包

internal包 是 golang 中特殊的一个包，它对外部module不可见，可用来存放数据操作等敏感内容。

可包含domain、repository、dao、rpc、data等包

- domain：领域服务，负责表达业务概念，业务状态信息以及业务规则，通过调用 repository 、rpc 或其他 domain 进行数据操作与数据整合，实现完整的某一领域模块的业务逻辑。在进行单元测试时可以绕过 service 直接对 domain 进行细粒度的测试，简化测试流程。
- repository：仓库，负责封装数据的查询、创建、更新、删除等逻辑的接口，屏蔽底层实现，供使用者调用

- dao：数据操作层，实现 repository 的接口，封装对MySQL、Redis等数据库操作的具体实现的。
- rpc：rpc客户端，对其他rpc服务进行远程调用的客户端。
- data：数据表的直接映射，并包含对于数据传输模型的转换。

#### pkg 包

pkg包 包含了服务具体实现与其所依赖的预定义的常量与全局变量。

可包含model、service等包

- model：预定义的常量与全局变量，便于在业务中重用，简化操作逻辑。可包含预定于的 rediskey、业务所需的枚举值、预定义的错误等。
- service：业务层，接受客户端传输的数据，对请求数据进行校验与转换，通过调用 domain 层进行业务操作，整合调用的结果值，返回给客户端。



## 工具

要人工来保证团队成员遵守了上述的编程规范并不是一件容易的事情。因此，我们有许多静态的和动态的代码分析工具帮助团队识别代码规范的错误，甚至可以发现一些代码的bug。

### golangci-lint

golangci-lint 是当前大多数公司采用的静态代码分析工具，词语Linter 指的是一种分析源代码以此标记编程错误、代码缺陷、风格错误的工具。

而golangci-lint是集合多种Linter的工具。要查看支持的 Linter 列表以及启用/禁用了哪些Linter，可以使用下面的命令：

```
golangci-lint help linters
```

Go语言定义了实现Linter的API，它还提供了golint工具，用于集成了几种常见的Linter。在[源码](https://cs.opensource.google/go/x/tools/+/refs/tags/v0.1.11:go/analysis/passes/unreachable/unreachable.go)中，我们可以查看怎么在标准库中实现典型的Linter。

Linter的实现原理是静态扫描代码的AST（抽象语法树），Linter的标准化意味着我们可以灵活实现自己的Linters。不过golangci-lint里面其实已经集成了包括golint在内的总多Linter，并且有灵活的配置能力。所以在自己写Linter之前，建议先了解golangci-lint现有的能力。

在大型项目中刚开始使用golang-lint会出现大量的错误，这种情况下我们只希望扫描增量的代码。如下所示，可以通过在[golangci-lint配置文件](https://golangci-lint.run/usage/configuration/)中调整new-from-rev参数，配置以当前基准分支为基础实现增量扫描

```yaml
linters:
 enable-all: true
issues:
 new-from-rev: master
```

### Pre-Commit

在代码通过Git Commit提交到代码仓库之前，git 提供了一种pre-commit的hook能力，用于执行一些前置脚本。在脚本中加入检查的代码，就可以在本地拦截住一些不符合规范的代码，避免频繁触发CI或者浪费时间。pre-commit的配置和使用方法，可以参考[TiDB](https://github.com/pingcap/tidb/blob/master/hooks/pre-commit)。

### 并发检测 race

Go 1.1 提供了强大的检查工具race来排查数据争用问题。race 可以用在多个Go指令中，一旦检测器在程序中找到数据争用，就会打印报告。这份报告包含发生race冲突的协程栈，以及此时正在运行的协程栈。可以在编译时和运行时执行race，方法如下：

```shell
$ go test -race mypkg
$ go run -race mysrc.go
$ go build -race mycmd
$ go install -race mypkg
```

在下面这个例子中， 运行中加入race检查后直接报错。从报错后输出的栈帧信息中，我们能看出具体发生并发冲突的位置。

```shell
» go run -race 2_race.go
==================
WARNING: DATA RACE
Read at 0x00000115c1f8 by goroutine 7:
	main.add()
		bookcode/concurrence_control/2_race.go:5 +0x3a
Previous write at 0x00000115c1f8 by goroutine 6:
	main.add()
		bookcode/concurrence_control/2_race.go:5 +0x56
```

第四行Read at 表明读取发生在2_race.go 文件的第5行，而第七行Previous write 表明前一个写入也发生在2_race.go 文件的第5行。这样我们就可以非常快速地定位数据争用问题了。

竞争检测的成本因程序而异。对于典型的程序，内存使用量可能增加 5~10 倍，执行时间会增加2~20倍。同时，竞争检测器会为当前每个defer和recover语句额外分配8字节。在Goroutine退出前，这些额外分配的字节不会被回收。这意味着，如果有一个长期运行的Goroutine，而且定期有defer 和recover调用，那么程序内存的使用量可能无限增长。（这些内存分配不会显示到 runtime.ReadMemStats或runtime / pprof 的输出。）

### 覆盖率

一般我们会使用代码覆盖率来判断代码书写的质量，识别无效代码。go tool cover 是go语言提供的识别代码覆盖率的工具。