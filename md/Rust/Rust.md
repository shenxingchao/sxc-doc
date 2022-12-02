# Rust

## 起步
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
            .expect("读取错误"); // 对于输入结果Result的异常处理

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

所有权它使 Rust 能够在不需要垃圾收集器的情况下做出内存安全保证