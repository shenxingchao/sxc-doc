# Rust

## 起步

[标准库](https://doc.rust-lang.org/std/index.html)

[类库](https://crates.io/)

[example](https://doc.rust-lang.org/rust-by-example/index.html)

### 安装

[windows](https://www.rust-lang.org/tools/install)

查看是否安装正确

```
rustc --version
```

更新

```
rustup update
```

还需要安装 [Visual Studio](https://visualstudio.microsoft.com/zh-hans/downloads/),并选择下面的选项

```
“使用 C++ 进行桌面开发”
Windows 10 或 11 SDK
```

编辑器直接使用vscode 安装好rust-analyzer、Rust Syntax、Code Runner扩展

### 创建项目

cargo是构建系统和包管理器，cargo会随rust一起安装

```powershell
cargo new demo
```

### 构建

构建运行的是target/debug/demo.exe文件

```
cargo run //编译并生成可执行文件
cargo check //只编译
cargo build //构建debug项目
cargo build --release //构建发布项目，生成release文件
```

### 运行

```powershell
cd src
rustc main.rs
```

或者直接点三角形运行 会生成main.exe 就可以直接运行了,这个只是运行单个rs文件,打包还得上面的构件

### 编写基本程序

用一个猜数字演示

```rs
//导包 use path::to::item
use rand::Rng;
use std::{cmp::Ordering, io};

fn main() {
    println!("请输入:");
    // 生成范围随机数
    let answer_number = rand::thread_rng().gen_range(1..=100);
    println!("答案 {answer_number}");
    loop {
        // let 定义变量
        // mut表示是可变变量 不加则是不可变
        // 调用String的静态方法new()，初始化字符串
        // 可以省略导包 变 std::io::stdin()
        let mut number = String::new();
        io::stdin()
            .read_line(&mut number) // & number 引用指向上面的number 因为引用也是不可变的所以要加上mut使其可变
            .expect("读取错误"); // 对于输入结果Result的错误处理

        // 调用宏! 打印
        println!("输入了 {number}");
        //String类型转换number u32 有符号 i32无符号
        //match xxx {}表达式 用于匹配结果
        let number: u32 = match number.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
        //调用比较函数
        match number.cmp(&answer_number) {
            Ordering::Less => println!("太小了"),
            Ordering::Greater => println!("太大了"),
            Ordering::Equal => {
                println!("答对了");
                break;
            }
        }
    }
}
```

### 生成文档

```
cargo doc --open
```

## 概念

### 变量

```rs
fn main() {
    let a = 1; //不可变变量
    let mut b = 2; //可变变量
    print!("{b}");
    b = 3;
    print!("{b}");
}
```

### 作用域

作用域内不会覆盖作用域外的值,作用域可用作表达式最后一行作为返回值返回不加分号，且{}末尾需要加分号
```rs
fn main() {
    let a = 1;
    let a = a + 1;
    let b = {
        let a = a + 2;
        println!("{a}"); //4
        a //表达式的最后一行作为返回值
    };
    print!("{a},{b}") //2,4
}
```

### 数据类型

#### 基本类型

```rs
fn main() {
    //u表示无符号 i表示有符号
    //整形
    let a: u8 = 255; //2^8-1
    let b: u16 = 65535; //2^16-1
    let c: u32 = 4294967295; //2^32-1
    let d: i64 = 9223372036854775807; //2^63-1
    let e: u128 = 340282366920938463463374607431768211455; //2^128-1
    let z: isize = 9223372036854775807;//取决于系统类型,64位系统就是i64
    println!("{a}{b}{c}{d}{e}{z}");
    //浮点数
    let f: f32 = 3.14;
    let g: f64 = 3.14;
    println!("{f}{g}");
    //布尔
    let h = true;
    let i:bool = false;
    if h {
        println!("{h}{i}")
    }
    //字符
    let j = 'a';
    let k:char = 'b';
    let l = '😻';
    println!("{j}{k}{l}");
}
```

#### 复合类型

```rs
fn main() {
    //元组 类型需一一对应
    let tup: (i32, i32, i32) = (-1, 1, -1);
    let (x, y, z) = tup;
    println!("{x},{y},{z}");
    let a: i32 = tup.0;
    println!("{a}");
    //数组 类型必须全部一致
    let b = [-1, 1, -1];
    let c: [i32; 3] = [-1, 1, -1];//中括号指定类型和长度
    let d:i32 = b[0] + c[0];
    println!("{d}");
}
```

#### 类型转换

```rs
fn main() {
    let str = "18";
    //string转int
    let num: u8 = str.parse().expect("转换错误");
    println!("{}", num);
    //int转String
    let str = &num.to_string();
    println!("{}", str);
    //int转float
    let fnum: f32 = num as f32;
    println!("{}", fnum);
    //float转int
    let num: u8 = fnum as u8;
    println!("{}", num);
}
```

### 常量

```rs
fn main() {
    const PI:f32 = 3.1415926;
    print!("{PI}");
}
```

### 函数

```rs
fn main() {
    add(1, 2); //1,2
    say(String::from("hello fn")); //hello fn
}

//->i32 返回类型
fn add(x: i32, y: i32) -> i32 {
    println!("{x},{y}");
    x + y //等同于return x + y;
}

fn say(str: String) {
    println!("{str}");
}
```

### 条件表达式

```rs
fn main() {
    if (3 > 1 || 3 > 2) && (1 < 3 || 1 < 2) {
        println!("true");//true
    } else {
        println!("false")
    }

    //if + {}作为条件表达式 返回值
    let a:isize = if 3 > 1 { 1 } else { 2 };
    println!("{a}"); //1
}
```

### 循环表达式

loop

```rs
fn main() {
    let mut a = 0;
    let res = loop {
        a = a + 1;
        if a < 5 {
            println!("{a}");//1234
            continue;
        }
        break a;//可直接用break返回值 也可以单纯用作循环
    };
    println!("{res}");//5
}
```

while

```rs
fn main() {
    let mut a = 0;
    while a < 5 {
        a = a + 1;
        println!("{a}")//12345
    };
}
```

遍历数组

```rs
fn main() {
    let list = [-1, 1, -1];
    let mut i = 0;
    while i < list.len() {
        println!("{}", list[i]);
        i += 1;
    }
}
```

for循环遍历数组

```rs
fn main() {
    let list = [-1, 1, -1];

    for item in list {
        println!("{}", item);
    }
}
```

需要计数遍历数组

```rs
fn main() {
    let list = [-1, 1, -1];
    for i in 0..list.len() {
        println!("{}", list[i]);
    }
}
```

### 所有权

#### 概念

> 所有权它使 Rust 能够在不需要垃圾收集器的情况下做出内存安全保证

**变量指针的移动，s2指向s1指向的堆内容**

```rs
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; //S1变量内存被释放，所有权移动到s2，所以后面不能再使用s1

    // println!("{}, world!", s1);//报错F
    println!("{}, world!", s2);
}
```

**深拷贝**

```rs
fn main() {
    let mut s1 = String::from("hello");
    let s2 = s1.clone(); //栈中创建两个指针s2 s1同时指向堆内存"hello"
    println!("{}, world!", s1); //hello, world!
    println!("{}, world!", s2); //hello, world!
    println!("{}", s1 == s2); //true
    s1.push_str(" haha"); //开辟新的堆内存"hello haha"
    println!("{}, world!", s1); //hello haha, world!
    println!("{}", s1 == s2); //false
}
```

**基本类型全是深拷贝**

```rs
fn main() {
    let x = 5;
    let y = x;
    println!("{},{}", x, y); //5,5
    println!("{}", x == y); //true
}
```

**函数所有权**

```rs
fn main() {
    let str = String::from("hello ownership!");
    let num = 5;
    fn1(str);
    fn2(num);

    //之后str不能再使用，所有权已经在函数内
    // println!("{}", str);//报错
    //num是基本类型可以继续使用
    println!("{}", num); //5

    //返回值所有权
    let str2 = fn3();
    println!("{}", str2);
}

fn fn1(mut str: String) {
    println!("{}", str); //hello ownership!
    str.push_str("!!!"); //函数内部可以继续使用
    println!("{}", str); //hello ownership!!!!
}

fn fn2(num: isize) {
    println!("{}", num); //5
}

fn fn3() -> String {
    String::from("hello wolrd!")
}
```

#### 引用

```rs
fn main() {
    let mut str = String::from("hello ownership!");
    fn1(&mut str); //使用可变引用则不转移所有权，后面可以继续使用
    println!("{}", str); //hello ownership!!!!

    let str2 = "hello world";
    let str3 = &str2;//&str2使用普通引用（借用不能改变值）指向str2在栈里的指针
    println!("{},{}", str2, str3); //hello world,hello world

    // println!("{}", str2 == str3);//引用后比较报错误...
}

fn fn1(str: &mut String) {
    println!("{}", str); //hello ownership!
    str.push_str("!!!");
    println!("{}", str); //hello ownership!!!!
}
```

!> 可变引用有一个很大的限制：如果你有一个对一个值的可变引用，你就不能有对该值的其他引用。

#### 切片

..操作类似于一个范围，包含开头不包含结尾

```rs
fn main() {
    let s = String::from("hello world");

    let hello = &s[0..5];//包含0不包含5
    let world = &s[6..11];
    println!("{} {}", hello, world); //hello world
}
```

> &str 这是字符串切片的类型

### 结构体

类似于声明一个类 只有属性

#### 普通结构体

```rs
//类似User类
struct User {
    name: String,
    age: u8,
}
fn main() {
    let mut user = User {
        name: String::from("张三"),
        age: 18,
    };
    user.name = String::from("李四");

    //..语法混入
    let user2 = User {
        name: String::from("王五"),
        ..user
    };
    println!("{}{}{}{}", user.name, user.age, user2.name, user2.age); //李四18王五18
}
```

#### 元组结构体

```rs
//无属性名
struct Color(u8, u8, u8);
fn main() {
    let color = Color(255, 255, 255);
    println!("{}", color.0);
}
```

#### 打印结构体

```rs
#[derive(Debug)] //加上才能使用{:?}打印结构体
struct Rect {
    width: u32,
    height: u32,
}
fn main() {
    let rect = Rect {
        width: 30,
        height: 50,
    };
    println!("{}{}", rect.width, rect.height); //3050
    println!("{:?}", rect); //Rect { width: 30, height: 50 }
    println!("{:#?}", rect); // Rect {
                             //     width: 30,
                             //     height: 50,
                             // }
}
```

#### 类实现

类似于成员方法 需要实现结构体struct/特征trait(类/接口)

```rs
#[derive(Debug)] //加上才能使用{:?}打印结构体
struct Rect {
    width: u32,
    height: u32,
}

//类实现
impl Rect {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rect {
        width: 30,
        height: 50,
    };
    println!("{}",rect.area());
}
```


#### 枚举

```rs
#[derive(Debug)]
enum Color {
    Red(u8),
    Green(u8),
    Blue(u8),
}

#[derive(Debug)]
struct Car {
    color: Color,
}

fn main() {
    let car = Car {
        color: Color::Blue(255),
    };
    println!("{:#?}{:?}", car, car.color);
    print!(
        "{:?}{:?}{:?}",
        Color::Red(255),
        Color::Green(255),
        Color::Blue(255)
    );
}
```

枚举使用泛型

```rs
#[allow(dead_code)] //禁止未使用变量报错
#[derive(Debug)]
enum Color<T> {
    None,
    Red,
    Green,
    Blue,
    Alpha(T),
}

fn main() {
    println!("{:#?}", Color::Alpha(0.5));
}
```

### 匹配控制

类似有返回值的switch,必须列出所有匹配，否则报错

```rs
#[allow(dead_code)] //禁止未使用变量报错
enum Color {
    Red,
    Green,
    Blue,
    Alpha(f32),
}

fn main() {
    println!("{}", get_color(Color::Green));
    println!("{}", get_color(Color::Alpha(0.5)));
    println!("{}", get_other(Color::Blue));
}

fn get_color(color: Color) -> String {
    match color {
        Color::Red => String::from("red"),
        Color::Green => {
            println!("匹配到绿色");
            String::from("green") //返回最后一行
        }
        Color::Blue => String::from("blue"),
        Color::Alpha(alpha) => {
            println!("匹配到透明度");
            String::from(alpha.to_string())
        }
    }
}

fn get_other(color: Color) -> String {
    match color {
        Color::Red => String::from("red"),
        //匹配其他结果
        _ => String::from("未知"),
    }
}
```

### iflet流程控制

相当于只匹配一个结果的match

```rs
#[allow(dead_code)] //禁止未使用变量报错
enum Color {
    Red,
    Green,
    Blue,
    Alpha(f32),
}

fn main() {
    let str = "hello world";

    if let "hello world" = str {
        println!("匹配到了");
    } else {
        println!("没匹配到");
    }

    //等价于
    if str == "hello world" {
        println!("匹配到了");
    } else {
        println!("没匹配到");
    }

    //这种可以用来匹配枚举获取枚举的值
    let color = Color::Alpha(0.5);

    if let Color::Alpha(alpha) = color {
        print!("{}", alpha);
    } else {
        println!("没匹配到");
    }
}
```

### 包

不加pub就是私有

**src\common.rs**

```rs
//pub 公开一个模块（结构体 方法 枚举 都可以公开 还能具体到每个属性） mod modlue模块缩写
pub mod common {
    pub mod util {
        #[allow(dead_code)]
        pub fn console(str: &String) {
            println!("{}", str);
            super::super::private_fn(); //调用并列父级的其他方法
        }
    }
}

//pub use 称为再导出
pub use common::util;

pub fn test(str: &String) {
    crate::common::util::console(str); //绝对路径 crate根目录
    common::util::console(str); //相对路径
    util::console(str);
}

fn private_fn() {}
```

**src\main.rs**

```rs
#[allow(dead_code)] //禁止未使用变量报错
mod common; //引入模块 类似于命名空间
use common::util;//使用use 用于简化common::util::console -> util::console

fn main() {
    common::util::console(&String::from("mod - hello package"));
    common::test(&String::from("mod - test"));
    util::console(&String::from("use - test"))
}
```

### 集合

#### Vector

```rs
#[allow(dead_code)] //禁止未使用变量报错

fn main() {
    //初始化 并添加值
    let mut v: Vec<isize> = Vec::new();
    v.push(1);
    v.push(2);
    println!("{:#?}", v);
    //初始化并赋值1
    let v: Vec<isize> = [1, 2, 3].to_vec();
    println!("{:#?}", v);
    //初始化并赋值2
    let mut v: Vec<isize> = vec![1, 2, 3];
    println!("{:#?}", v);
    //指定索引读取
    println!("{}", &v[0]);
    //方法读取 返回的是枚举Option::Some
    println!("{:?}", v.get(0)); //Some(1)

    //使用Option 可以匹配上面Some返回的值
    let third: Option<&isize> = v.get(2);
    match third {
        Some(third) => println!("{}", third),
        None => println!("没有匹配到"),
    }

    //遍历
    for &mut item in &mut v {
        println!("{}", &item + 1);
    }
}
```

#### 字符串

```rs
use std::ops::Add;

#[allow(dead_code)] //禁止未使用变量报错
#[allow(unused_variables)] //允许局部变量不使用

fn main() {
    //初始化
    let str = String::new();
    let str = String::from("hello world");
    let str = "hello world".to_string();
    println!("{}", str);

    //添加字符串
    let mut str = String::from("hello");
    str.push_str(" world");
    //添加字符
    str.push('!');

    //拼接字符串1 使用+
    let str1 = String::from("hello");
    let str2 = String::from(" world");
    let str = str1 + &str2; //str1 不能再使用 str2可以继续
    println!("{}", str);
    //拼接字符串2 使用+
    let str = "hello".to_string() + " world";
    println!("{}", str);
    //拼接字符串3 使用format!
    let str1 = String::from("hello");
    let str2 = String::from(" world");
    let str = format!("{}{}", str1, str2);
    println!("{}", str);
    //拼接字符串4 使用add函数
    let mut str = String::from("hello world");
    str = str.add("!");
    println!("{}", str);

    //字符串切片
    let str = "hello world";
    let hello = &str[0..5];
    println!("{}", str);

    //迭代字符串
    for ch in "hello world".chars() {
        println!("{}", ch);
    }
}
```

#### HashMap

```rs
use std::collections::HashMap;

#[allow(dead_code)] //禁止未使用变量报错

fn main() {
    //创建hashmap
    let mut person = HashMap::new();
    person.insert("name", "张三");
    person.insert("age", "18");
    //更新
    person.insert("age", "20");
    println!("{:#?}", person);

    //访问
    println!("{}", person["name"]); //张三
    println!("{:?}", person.get("name")); //Some("张三")

    //遍历
    for (key, value) in &person {
        println!("{}:{}", key, value);
    }
    
    //键不存在则插入
    person.entry("age").or_insert("30");
}
```

### 错误处理

**以文件错误来演示**

#### match处理错误

```rs
#[allow(dead_code)] //禁止未使用变量报错
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    //打开文件
    let greeting_file_result = File::open("hello.txt");
    //匹配文件结果
    let _greeting_file = match greeting_file_result {
        Ok(file) => file, //返回文件
        //匹配错误类型
        Err(error) => match error.kind() {
            //文件不存在,创建文件
            ErrorKind::NotFound => match File::create("hello.txt") {
                //文件创建成功
                Ok(fc) => fc,
                //文件创建失败
                Err(e) => panic!("错误: {:?}", e),
            },
            other_error => {
                //其他错误
                panic!("错误: {:?}", other_error);
            }
        },
    };
    println!("{:?}", _greeting_file); //File { handle: 0x90, path: "\\\\?\\D:\\sxc\\rust\\demo\\hello.txt" }
}
```

#### unwrap_or_else处理错误

```rs
#[allow(dead_code)] //禁止未使用变量报错
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    //unwrap_or_else来替代match处理错误
    let _greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        //打开文件失败
        if error.kind() == ErrorKind::NotFound {
            //文件不存在,创建文件
            File::create("hello.txt").unwrap_or_else(|error| {
                //文件创建失败
                panic!("错误: {:?}", error);
            })
        } else {
            //其他错误
            panic!("错误: {:?}", error);
        }
    });
    println!("{:?}", _greeting_file); //File { handle: 0x90, path: "\\\\?\\D:\\sxc\\rust\\demo\\hello.txt" }
}
```

#### unwrap直接抛出错误

```rs
#[allow(dead_code)] //禁止未使用变量报错
use std::fs::File;
fn main() {
    //unwrap 直接抛出错误
    let _greeting_file = File::open("hello.txt").unwrap();
    println!("{:?}", _greeting_file);
}
```

#### expect直接抛出并给出提示

```rs
#[allow(dead_code)] //禁止未使用变量报错
use std::fs::File;
fn main() {
    let _greeting_file = File::open("hello.txt").expect("缺少文件hello.txt");
    println!("{:?}", _greeting_file);
}
```

#### 使用?传递错误

```rs
#[allow(dead_code)] //禁止未使用变量报错
use std::fs::File;
use std::{
    fs,
    io::{self, Read},
};
fn main() -> Result<(), io::Error> {
    println!("{:?}", read_string()); //Ok("hello world")

    //简单方法读取字符串使用fs模块
    let str = fs::read_to_string("hello.txt")?;
    println!("{}", str); //hello world

    //maini函数用问号必须返回
    let mut str = String::new();
    File::open("hello.txt")?.read_to_string(&mut str)?;
    println!("{}", str); //hello world
    Ok(())
}

fn read_string() -> Result<String, io::Error> {
    let mut str = String::new();
    File::open("hello.txt")?.read_to_string(&mut str)?;
    Ok(str)
}
```

### 泛型

**实例：找出集合中的最大值**

```rs
#[allow(dead_code)] //禁止未使用变量报错

fn main() {
    let list: Vec<i32> = [11, 222, 33, 44, 555, 6].to_vec();
    println!("max is {}", get_max(&list)); //max is 555
    let list: Vec<char> = ['a', 'c', 'b'].to_vec();
    println!("max is {}", get_max(&list)); //max is c
}

//找到集合中的最大值 PartialOrd特征(接口 实现了类型的比较 所以这里可以实现各种类型的比较) 返回值也需要加& (修改后返回引用类型，避免了参数只能使用实现了Copy特征类型的问题，且不会额外分配内存)
fn get_max<T: PartialOrd>(list: &[T]) -> &T {
    let mut max = &list[0];
    for item in list {
        if item > max {
            max = item;
        }
    }
    max
}
```

### 特征trait

特征类似于接口

#### 定义

animal.rs

```rs
//动物类
pub struct Animal {
    pub weight: f32,
}

//狗狗接口
pub trait Dog {
    fn say(&self, str: String) -> String;
}

//动物类实现狗狗接口说话方法
impl Dog for Animal {
    fn say(&self, str: String) -> String {
        format!("Dog say({} g):{}", self.weight, str)
    }
}
```

main.rs

```rs
#[allow(dead_code)] //禁止未使用变量报错
mod animal;
use animal::{Animal, Dog};

fn main() {
    println!(
        "{}",
        Animal { weight: 10.0 }.say(String::from("hello trait"))
    );
}
```

#### 接口类型

```rs
#[allow(dead_code)] //禁止未使用变量报错
mod animal;
use animal::{Animal, Dog};

fn main() {
    println!("{}", dog_say(&say()));
    println!("{}", dog_say2(&say()));
}

//函数返回一个实现接口的对象
fn say() -> impl Dog {
    Animal { weight: 10.0 }
}

//下面这张的简写
fn dog_say(dog: &impl Dog) -> String {
    dog.say(String::from("hello"))
}

//泛型使用接口类型
fn dog_say2<T: Dog>(dog: &T) -> String {
    dog.say(String::from("hello"))
}
```

#### 实现Display以更好方式输出

animal.rs

```rs
use std::fmt::Display;

//skip

//实现Display接口
impl Display for Animal {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "{:?}", self.weight)
    }
}
```

main.rs

```rs
#[allow(dead_code)] //禁止未使用变量报错
mod animal;

use std::fmt::Display;

use animal::{Animal, Dog};

fn main() {
    println!("{}", dog_say(&say()));
    println!("{}", dog_say2(&say()));
}

//函数返回一个实现接口的对象
fn say() -> impl Dog + Display {
    Animal { weight: 10.0 }
}

//下面这张的简写
fn dog_say(dog: &(impl Dog + Display)) -> String {
    dog.say(String::from("hello"))
}

//泛型使用接口类型 还有一例可参考泛型
fn dog_say2<T: Dog + Display>(dog: &T) -> String {
    dog.say(String::from("hello"))
}
```

### 生命周期

声明周期就是这个变量有没有被内存回收 使用& 借用符号来保证变量不被回收;每个引用都有生命周期

#### 简单的生命周期

```rs
fn main() {
    let r;                // ---------+-- 'a  |r的声明周期开始
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |x的声明周期开始
        r = &x;           //  |       |
    }                     // -+       |x的声明周期结束
                          //          |
    println!("r: {}", r); //          |r的声明周期结束  所以这里引用了x 可是x已经从内存中移除，所以会报错
}       
```

#### 泛型引用

```rs
#[allow(dead_code)] //禁止未使用变量报错

fn main() {
    let string1 = String::from("hello");
    let string2 = "world!";

    let result = longest(&string1, &string2);
    println!("长的字符串是{}", result); //长的字符串是world!
}

//<'t> 泛型生命周期引用 随便取名，加了这个就可以返回任何类型的引用,否则只能返回一个引用
fn longest<'t>(x: &'t str, y: &'t str) -> &'t str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

### 测试

#### 基本写法

src/lib.rs

```rs
pub fn add_two(a: i32) -> i32 {
    a + 2
}

#[cfg(test)]
pub mod tests {
    use super::*; //可以使用外部的任何函数

    #[test]
    pub fn pass() {
        assert!(4 == add_two(2), "自定义错误内容"); //普通断言
    }

    #[test]
    pub fn eq_pass() {
        assert_eq!(4, add_two(2)); //相等断言
    }

    #[test]
    pub fn not_eq_pass() {
        assert_ne!(3, add_two(2)); //不等断言
    }

    #[test]
    pub fn custom_error_message() -> Result<(), String> {
        if true {
            Ok(())
        } else {
            Err(String::from("测试不通过"))//返回ERR 而不是 panic
        }
    }

    #[test]
    #[should_panic]
    pub fn should_panic_fn() {
        panic!("出现panic终止运行,则测试通过");
    }
}
```

#### 运行

```
cargo test
cargo test -- --show-output //查看print输出值
```

#### 编写测试文件夹

修改src/lib.rs

```rs
pub fn add_two(a: i32) -> i32 {
    a + 2
}
```

创建/tests/test.rs

```rs
use demo::{self, add_two};

#[test]
pub fn pass() {
    assert!(4 == add_two(2), "自定义错误内容"); //普通断言
}
```

之后运行```cargo test 或者cargo test --test test```