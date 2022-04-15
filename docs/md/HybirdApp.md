# HybirdApp

## uniapp

### Native.js示例汇总

<p align="left" style="color:#777777;">发布日期：2021-01-26</p>

工作时用到了低功耗蓝牙开发BLE蓝牙  
[示例地址](https://ask.dcloud.net.cn/article/114)

* * *

## react native

### 环境搭建

<p align="left" style="color:#777777;">发布日期：2021-02-19</p>

#### 一、java jdk安装和环境变量配置

[jdk1.8.0下载地址](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html#license-lightbox)  
环境变量配置

| 系统变量名称    | 值                                                   |
|:--------- | ---------------------------------------------------:|
| JAVA_HOME | D:\tool\jdk1.8.0                                    |
| Path      | %JAVA_HOME%\bin                                     |
| Path      | %JAVA_HOME%\jre\bin                                 |
| CLASSPATH | .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar; |

检查是否配置成功

```
java -version
```

输出java version "1.8.0_191"

#### 二、安装android studio

[下载地址](https://developer.android.google.cn/studio/)  

- 选择Custom选项安装
- 确保选中
  - Android SDK
  - Android SDK Platform
- SDK Manager - SDK Platforms - Show Package Details 
  - Android SDK Platform 29
- SDK Manager - SDK Tools - Show Package Details
  - Android SDK Build-Tools
    - 29.0.2版本
  - NDK (Side by side) - Show Package Details
    - 20.1.5948944版本

?> 安装sdk和编译工具，模拟器不需要安装，用第三方

环境变量配置

| 系统变量名称       | 值                             |
|:------------ | -----------------------------:|
| ANDROID_HOME | D:\tool\androidsdk            |
| Path         | %ANDROID_HOME%\platform-tools |
| Path         | %ANDROID_HOME%\tools          |
| Path         | %ANDROID_HOME%\tools\bin      |

#### 三、安装夜神模拟器

[下载地址](https://www.yeshen.com/)

!> 备份原有模拟器的nox_adb.exe 为nox_adb_old.exe，需要将androidsdk\platform-tools 下的adb.exe 改名为nox_adb.exe复制到夜神模拟器的adb文件夹，因为sdk版本不一样会跑不起来。 

#### 四、cli创建应用

npm install -g  react-native-cli  
react-native init

#### 五、运行

- 开启模拟器
- abd连接模拟器
  - adb connect 127.0.0.1:62001
- 查看设备连接状态
  - adb devices
    - 显示：127.0.0.1:62001 device 表示已连接

运行前先下载安装gradle，手动下载那个连接，然后配置url

- 打开 android\gradle\wrapper\gradle-wrapper.properties
  - 找到 distributionUrl 下载到本地
- 替换distributionUrl 地址为本地
  - distributionUrl=file\:///D\:/tool/gradle/gradle-6.2-all.zip

```npm
npm run android
```

#### 六、调试工具

```npm
npm config set electron_mirror https://cdn.npm.taobao.org/dist/electron/  
npm install -g electron
npm install -g react-devtools
npx react-devtools
```

```
adb reverse tcp:8097 tcp:8097
```

!> 安装调式工具react-devtools 时 一定要先安装electron 再安装 react-devtools

!> 启动不成功 adb reverse tcp:8097 tcp:8097

?> 打开开发菜单 在cli.js start窗口按d就可以了 或者 adb shell input keyevent 82

其他工具  
更好看 官方
https://fbflipper.com 布局 日志 网络
功能更强大 第三方 用这个
https://github.com/jhen0409/react-native-debugger 布局 日志 网络 redux props

### react native elements

<p align="left" style="color:#777777;">发布日期：2021-02-19</p>

#### 安装

- Step1
  
  ```
  npm i react-native-elements --save
  ```

- Step2
  
  ```
  npm i --save react-native-vector-icons
  react-native link react-native-vector-icons
  ```

- Step3
  
  ```
  npm i --save react-native-safe-area-context
  react-native link react-native-safe-area-context
  ```

!>  //使用 yarn add 速度快 不管怎么样都link一下不会错

#### 基本使用案例

```javascript
//导入按钮和主题容器
import {Button, ThemeProvider} from 'react-native-elements';
//导入安全区域容器
import {SafeAreaProvider} from 'react-native-safe-area-context';

//自定义主题方法   实际开发主题样式放到style文件夹下导出 主要是定义颜色
const theme = {
  Button: {
    raised: false,
    titleStyle: { //这个titleStyle的类型是Text style (object)     继承自rn组件的Text的style
      color: '#ffffff',
      fontSize: 50,
      fontWeight: 'normal',
      textTransform: 'lowercase',
      writingDirection: 'ltr',
      textShadowColor: 'red',
      textShadowOffset: {width: 10, height: 10},
    },
    containerStyle: {
      marginTop: 10,
      marginLeft: 10,
      marginRight: 10,
    },
  },
};

//根组件
const App = () => {
  return (
    <SafeAreaProvider>
      <ThemeProvider theme={theme}>
        <Button title="My Button" />
        <Button title="My 2nd Button" buttonStyle={{backgroundColor: 'red'}} />
        <Button title="My 3nd Button" disabled={true} />
      </ThemeProvider>
    </SafeAreaProvider>
  );
};
export default App;
```

### react navigation路由导航器 底部tabbar

<p align="left" style="color:#777777;">发布日期：2021-02-21</p>

#### 文档地址

https://reactnavigation.org/docs

#### 安装

- Step1
  
  ```
  yarn add @react-navigation/native //报错的话把yarn-error.log 删掉 重来
  ```

- Step2
  
  ```
  yarn add react-native-reanimated react-native-gesture-handler react-native-screens  @react-native-community/masked-view react-native-safe-area-context //react-native-safe-area-context如果在上面的ui框架已经安装  就不需要了
  ```

#### 使用

- 必须全局导入react-native-gesture-handler
  
  ```
  import 'react-native-gesture-handler';
  ```

#### 堆栈导航

- 安装
  
  ```
  yarn add @react-navigation/stack
  ```

- 创建
  
  ```javascript
  //App.js
  import 'react-native-gesture-handler';
  import React from 'react';
  import {View, Text, Button, StatusBar} from 'react-native';
  import {NavigationContainer} from '@react-navigation/native';
  import {createStackNavigator} from '@react-navigation/stack';
  
  //定义一个主屏幕
  let HomeScreen = ({navigation, route}) => {
    React.useEffect(() => {
      if (route.params?.post) {
        //订阅操作。如果有post参数回传，那么执行一些代码
      }
    }, [route.params?.post]);
    return (
      <View style={{flex: 1, alignItems: 'center', justifyContent: 'center'}}>
        <StatusBar
          backgroundColor="#f60"
          barStyle="light-content"
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>首页{route.params && <Text>{route.params.post}</Text>}</Text>
        <Button
          title="跳转到详情页"
          onPress={() => navigation.navigate('Details', {id: 99})} //路由跳转传参
        />
      </View>
    );
  };
  //定义一个详情页
  let DetailsScreen = ({navigation, route}) => {
    const {id} = route.params; //获取路由参数
    return (
      <View style={{flex: 1, alignItems: 'center', justifyContent: 'center'}}>
        <StatusBar
          backgroundColor="#ccc"
          barStyle="dark-content"  //字没变黑的话是安卓版本太低了
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>详情页id:{id}</Text>
        <Button
          title="再次跳转到详情页"
          onPress={() => navigation.push('Details', {id: 99})}
        />
        <Button title="返回首页" onPress={() => navigation.navigate('Home')} />
        <Button
          title="返回首页并带参数"
          onPress={() => navigation.navigate('Home', {post: '我是返回带的参数'})}
        />
        <Button title="返回上一页" onPress={() => navigation.goBack()} />
        <Button
          title="返回堆栈的第一个屏幕"
          onPress={() => navigation.popToTop()}
        />
      </View>
    );
  };
  
  const Stack = createStackNavigator();
  
  let App = () => {
    return (
      <NavigationContainer>
        {/* 初始化为主屏幕 */}
        <Stack.Navigator
          initialRouteName="Home"
          screenOptions={{
            //跨屏幕共享样式
            headerStyle: {
              backgroundColor: '#f4511e',
            },
            headerTintColor: '#fff',
            headerTitleStyle: {
              fontWeight: 'bold',
            },
          }}>
          <Stack.Screen
            name="Home"
            component={HomeScreen}
            options={{
              title: '标题名称',
              headerShown: false, //隐藏导航栏 就能用沉浸式导航啦
            }}
          />
          <Stack.Screen
            name="Details"
            component={DetailsScreen}
            // options={{title: '详情页'}}
            options={({route}) => ({title: route.params.id})}
          />
        </Stack.Navigator>
      </NavigationContainer>
    );
  };
  
  export default App;
  ```

- 创建底部导航
  
  ```
  yarn add @react-navigation/bottom-tabs
  ```
  
  使用  
  
  ```javascript
  //App.js
  import 'react-native-gesture-handler';
  import React from 'react';
  import {View, Text, Button, StatusBar} from 'react-native';
  import {NavigationContainer} from '@react-navigation/native';
  import {createStackNavigator} from '@react-navigation/stack';
  import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';
  
  const Stack = createStackNavigator();
  const Tab = createBottomTabNavigator();
  
  //定义一个发现页
  let FindScreen = ({navigation, route}) => {
    return (
      <View
        style={{
          flex: 1,
          alignItems: 'center',
          justifyContent: 'center',
          backgroundColor: 'blue',
        }}>
        <StatusBar
          backgroundColor="transparent"
          barStyle="dark-content" //字没变黑的话是安卓版本太低了
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>发现页</Text>
      </View>
    );
  };
  
  //定义一个详情页
  let DetailsScreen = ({navigation, route}) => {
    const {id} = route.params; //获取路由参数
    return (
      <View style={{flex: 1, alignItems: 'center', justifyContent: 'center'}}>
        <StatusBar
          backgroundColor="transparent"
          barStyle="dark-content" //字没变黑的话是安卓版本太低了
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>详情页id:{id}</Text>
        <Button
          title="再次跳转到详情页"
          onPress={() => navigation.push('DetailsScreen', {id: 99})}
        />
        <Button
          title="返回首页"
          onPress={() => navigation.navigate('IndexScreen')}
        />
        <Button
          title="返回首页并带参数"
          onPress={() =>
            navigation.navigate('IndexScreen', {post: '我是返回带的参数'})
          }
        />
        <Button title="返回上一页" onPress={() => navigation.goBack()} />
        <Button
          title="返回堆栈的第一个屏幕"
          onPress={() => navigation.popToTop()}
        />
        <Button
          title="跳转到发现页"
          onPress={() =>
            navigation.navigate('发现页', {
              params: {user: 'findle'},
            })
          } // 跳转到其他屏幕 第一个是屏幕name 第二个是参数  多级嵌套 params: {screen: '发现页详情'}, 就可以了
        />
      </View>
    );
  };
  
  //定义一个首页
  let IndexScreen = ({navigation, route}) => {
    React.useEffect(() => {
      if (route.params?.post) {
        //订阅操作。如果有post参数回传，那么执行一些代码
      }
    }, [route.params?.post]);
    return (
      <View
        style={{
          flex: 1,
          alignItems: 'center',
          justifyContent: 'center',
          backgroundColor: '#f60',
        }}>
        <StatusBar
          backgroundColor="transparent"
          barStyle="light-content"
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>首页{route.params && <Text>{route.params.post}</Text>}</Text>
        <Button
          title="跳转到详情页"
          onPress={() =>
            navigation.navigate('DetailsScreen', {
              id: 99,
            })
          } //路由跳转传参
        />
        <Button
          title="跳转到我的"
          onPress={() =>
            navigation.navigate('我的', {
              params: {user: 'jane'},
            })
          } //路由跳转传参
        />
      </View>
    );
  };
  
  //定义一个首页堆栈导航
  let IndexStack = () => {
    return (
      <Stack.Navigator
        initialRouteName="IndexScreen"
        screenOptions={{
          //跨屏幕共享样式
          headerStyle: {
            backgroundColor: '#f4511e',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}>
        <Stack.Screen
          name="IndexScreen"
          component={IndexScreen}
          options={{
            headerShown: false, //隐藏导航栏 就能用沉浸式导航啦
          }}
        />
        <Stack.Screen name="DetailsScreen" component={DetailsScreen} />
      </Stack.Navigator>
    );
  };
  
  //定义一个我的
  let MyScreen = ({navigation, route}) => {
    return (
      <View
        style={{
          flex: 1,
          alignItems: 'center',
          justifyContent: 'center',
          backgroundColor: '#f60f60',
        }}>
        <StatusBar
          backgroundColor="transparent"
          barStyle="light-content"
          animated={true}
          hidden={false}
          translucent={true}
        />
        <Text>我的{route.params && <Text>{route.params.post}</Text>}</Text>
      </View>
    );
  };
  
  //定义一个主屏幕
  let HomeScreen = ({navigation, route}) => {
    return (
      <Tab.Navigator keyboardHidesTabBar={true}>
        <Tab.Screen name="首页" component={IndexStack} />
        <Tab.Screen name="我的" component={MyScreen} />
      </Tab.Navigator>
    );
  };
  
  //APP堆栈导航
  let App = () => {
    return (
      <NavigationContainer>
        <Stack.Navigator initialRouteName="HomeScreen">
          <Stack.Screen
            name="HomeScreen"
            component={HomeScreen}
            options={{
              headerShown: false, //隐藏导航栏 就能用沉浸式导航啦
            }}
          />
          <Stack.Screen
            name="发现页"
            component={FindScreen}
            options={{
              headerShown: false, //隐藏导航栏 就能用沉浸式导航啦
            }}
          />
        </Stack.Navigator>
      </NavigationContainer>
    );
  };
  
  export default App;
  //嵌套  堆栈导航->tab导航->堆栈导航->实际页面
  ```

### react-native-scrollable-tab-view

<p align="left" style="color:#777777;">发布日期：2021-02-21</p>

#### 滚动tab

- 安装
  
  ```
  yarn add @react-native-community/viewpager
  react-native link @react-native-community/viewpager 
  //上面的是依赖库
  yarn add react-native-scrollable-tab-view
  ```

- 使用
  
  ```javascript
  import ScrollableTabView, {
    ScrollableTabBar
  } from 'react-native-scrollable-tab-view'
  
  <ScrollableTabView
    style={{
      backgroundColor: ThemeColor.white,
      height: 1000
    }}
    initialPage={0} //初始化第一个tab
    renderTabBar={() => <ScrollableTabBar />}
    tabBarPosition="top" //顶部
    locked={false} //锁定拖动 默认否
    tabBarUnderlineStyle={{ backgroundColor: ThemeColor.primary }} //下划线颜色
    tabBarActiveTextColor={ThemeColor.primary}
    tabBarInactiveTextColor={ThemeColor.h2}
    tabBarTextStyle={{ fontSize: 16 }}
    onScroll={position => {
      // console.log('滑动时的位置：' + position)
    }}
    onChangeTab={(key, ref) => {
      // console.log(key)//在这里处理点击显示哪个tab key 就是tabitem的key
    }}
  >
    <ScrollView tabLabel="选项卡1">
      <Text>选项卡1内容</Text>
    </ScrollView>
    <ScrollView tabLabel="选项卡2">
      <Text>选项卡2内容</Text>
    </ScrollView>
    <ScrollView tabLabel="选项卡3">
      <Text>选项卡3内容</Text>
    </ScrollView>
    <ScrollView tabLabel="选项卡4">
      <Text>选项卡4内容</Text>
    </ScrollView>
    <ScrollView tabLabel="选项卡5">
      <Text>选项卡5内容</Text>
    </ScrollView>
  </ScrollableTabView>
  ```

!> 假如这里报getNode()函数的问题，去react-native-scrollable-tab-view/index.js 搜索那个函数，把他删了就行了，高版本的rn>0.62.0不需要，低版本的话又需要

#### 吸顶tab 嵌套scrollview

- 安装
  
  ```
  yarn add react-native-head-tab-view
  yarn add react-native-scrollable-tab-view-collapsible-header //基于上面那个插件
  ```

- 使用
  
  ```javascript
  //定义一个首页堆栈导航
  import React, { useState } from 'react'
  //导入基础组件
  import {
    View,
    Text,
    StatusBar,
    ScrollView,
    ActivityIndicator,
    Dimensions
  } from 'react-native'
  import SafeAreaView from 'react-native-safe-area-view'
  //导入吸顶导航嵌套滚动
  import { HScrollView } from 'react-native-head-tab-view'
  import { CollapsibleHeaderTabView } from 'react-native-scrollable-tab-view-collapsible-header'
  //导入UI组件
  import { Image } from 'react-native-elements'
  //导入自定义组件
  import AutoHeightImage from '../../components/AutoHeightImage'
  //导入主题
  import { theme, ThemeColor } from '../../../styles/theme'
  
  //定义一个首页
  export default IndexScreen = ({ navigation, route, props }) => {
    const [isRefreshing, setIsRefreshing] = useState(false)
    const [showActivityIndicator, setShowActivityIndicator] = useState(false)
    return (
      <SafeAreaView
        style={{
          flex: 1
        }}
      >
        <CollapsibleHeaderTabView
          headerHeight={120}
          renderScrollHeader={() => (
            <View>
              <StatusBar
                backgroundColor="transparent"
                barStyle="light-content"
                animated={true}
                hidden={false}
                translucent={true}
              />
              <AutoHeightImage
                style={{ height: 120 }} //必须设高度 不然吸顶会失效
                source={{ uri: 'http://www.ay1.cc/img?w=720&h=180&c=f60f60' }}
                resizeMode="cover" //先设contain 再设cover 保证高度和图片差不多都能正好显示
              />
            </View>
          )}
          style={{
            backgroundColor: ThemeColor.white
          }}
          initialPage={0} //初始化第一个tab
          tabBarPosition="top" //顶部
          locked={false} //锁定拖动 默认否
          tabBarUnderlineStyle={{
            backgroundColor: ThemeColor.primary
          }} //下划线颜色
          tabBarActiveTextColor={ThemeColor.primary}
          tabBarInactiveTextColor={ThemeColor.h2}
          tabBarTextStyle={{ fontSize: 16 }}
          onScroll={position => {
            // console.log('滑动时的位置：' + position)
          }}
          onChangeTab={(key, ref) => {
            // console.log(key)//在这里处理点击显示哪个tab key 就是tabitem的key
          }}
          //整个页面下拉刷新
          // isRefreshing={isRefreshing}
          // onStartRefresh={() => {
          //   setIsRefreshing(true)
          //   console.log('开始刷新')
          //   setTimeout(() => {
          //     console.log('刷新结束')
          //     setIsRefreshing(false)
          //   }, 2000)
          // }}
        >
          {/* 选项卡标签 */}
          <HScrollView
            index={0}
            tabLabel="推荐"
            style={{
              backgroundColor: 'yellow'
            }}
            //标签页下拉刷新
            isRefreshing={isRefreshing}
            onStartRefresh={() => {
              // console.log('开始刷新')
              setIsRefreshing(true)
              setShowActivityIndicator(true)
              setTimeout(() => {
                // console.log('刷新结束')
                setIsRefreshing(false)
                setShowActivityIndicator(false)
              }, 1500)
            }}
          >
            {showActivityIndicator && (
              <View
                style={{
                  position: 'relative',
                  justifyContent: 'center'
                }}
              >
                <ActivityIndicator
                  style={{
                    position: 'absolute',
                    top: 0,
                    left: 0,
                    right: 0
                  }}
                  size="large"
                  color={ThemeColor.primary}
                />
              </View>
            )}
            <Text style={{ height: 500 }}>推荐列表1</Text>
            <Text style={{ height: 500 }}>推荐列表2</Text>
            <Text style={{ height: 500 }}>推荐列表3</Text>
            <Text>推荐列表4</Text>
            <Text style={{ width: '100%', height: 100, backgroundColor: 'red' }}>
              xxx
            </Text>
          </HScrollView>
          <HScrollView
            index={1}
            tabLabel="最新"
            style={{
              backgroundColor: 'red'
            }}
          >
            <Text>最新列表</Text>
          </HScrollView>
          <HScrollView
            index={2}
            tabLabel="热门"
            style={{
              backgroundColor: 'blue'
            }}
          >
            <Text>热门列表</Text>
          </HScrollView>
        </CollapsibleHeaderTabView>
      </SafeAreaView>
    )
  }
  ```

如果想要tab可以滚动，用下面的替换CollapsibleHeaderTabView，这个目前我测试下还是有卡顿，等完善再用吧

```javascript
import {SlideTabView} from 'react-native-tab-view-collapsible-header' 
import {ScrollableTabBar} from 'react-native-scrollable-tab-view'
const SScrollView = HPageViewHoc(ScrollView, { slideAnimated: true })
```

然后用自定义渲染tabbar

```javascript
renderTabBar={() => <ScrollableTabBar />}
```

标签页的HScrollView 也不能用了  要用SScrollView

## dart

!> dart是flutter应用的编写语言

### 安装

不需要安装，安装下面的flutter SDK已经带了Dart SDK了

### 变量类型

```dart
  var varStr = 'hello wolrd!';
  String str = '字符串';
  int num = 12306;
  double floatNum = 0.998;
  bool flag = true;
  //打印多个变量
  print("$varStr\n$str\n$num\n$floatNum\n$flag\n");
```

### 常量的两种写法

```dart
  const double PI = 3.1415926;
  final double pi;
  pi = 3.1415926;
  //final是运行时常量 const定义下面这个会报错 const只能直接赋值 final就可以定义为一个函数生成的值
  //final可以先定义再赋值 const只能直接赋值
  final DateTime curTime = new DateTime.now();
  print("$PI\n$pi\n$curTime\n");
```

### 数组

```dart
  //1创建方式1
  var list = ['张三', '李四'];
  print(list[0]);
  print(list[0].length);
  //2创建方式2
  var lists = <int>[1, 2];
  print(lists);
  //3添加数据
  lists.add(4);
  //4删除数据
  lists.remove(1);
  lists.removeAt(0);
  print(lists);
  //5 创建方式3
  List<int> listss = <int>[1, 2];
  print(listss);
  //6创建方式4 创建固定长度的数组 length 不可变 被的创建方式长度都可以变
  var listsss = List<String>.filled(2, "");
  print(listsss);
```

### 字典

```dart
  //创建方式1
  var set = {'id': 1, "name": '张三'};
  print(set);
  //创建方式2
  var sett = new Map();
  sett['id'] = 1;
  sett['name'] = '张三';
  print(set);
```

### 判断变量类型

```dart
  bool flag = true;
  print(flag is bool);
```

### 类型转换

```dart
  int num = 123456;
  //parse(String类型)
  print(num.toString());
  print(int.parse(num.toString()));
  print(double.parse(num.toString()));
```

### if判断

```dart
  bool flag = true;
  //这里的==两边的必须类型一致
  if (flag == true) {
    print('真');
  } else {
    print('假');
  }
```

### 运算符

```dart
  //没列举的都和js一样
  bool flag = true;
  //1取整
  print(9 ~/ 2);
  //2判空赋值运算符
  var x; //注意这里的x不能设为int,int的话必须初始化值,var可以只声明变量而不初始化
  x ??= 3;
  print(x);
  //3三元运算符
  x = flag == true ? 4 : 5;
  print(x);
```

?> 什么swith for while do...while... try catch都和js一样就不列了

### 单例模式

```dart
class Manager{
  // 工厂模式
  factory Manager() => _getInstance();
  static Manager get instance => _getInstance();
  static Manager? _instance;

  //音频播放器
  late AudioPlayer audioPlayer;

  Manager._internal() {
    //全局AudioPlayer对象只有一个实例
    audioPlayer = AudioPlayer();
  }

  static Manager _getInstance() {
    _instance ??= Manager._internal();
    return _instance as Manager;
  }
}
// 无论如何初始化，取到的都是同一个对象
Manager manager = new Manager();
Manager manager2 = Manager.instance;
```

## flutter

### 常用

[flutter pub 中文网](https://pub.flutter-io.cn/)

[flutter实战第二版](https://book.flutterchina.club/)

[flutter老孟](http://laomengit.com/flutter/widgets/widgets_structure.html)

### 安装

参考地址 https://flutter.cn/docs/get-started/install/windows  
创建项目 https://flutter.cn/docs/get-started/test-drive?tab=vscode#create-app

### vscode配置

1. 安装flutter插件 安装过程会自动安装dart插件

2. 检查安装是否完成
   
   ```
   flutter doctor
   ```

3. 出现 
   
   ```
   Android toolchain - develop for Android devices (Android SDK version 30.0.3)
    X cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
    X Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/docs/get-started/install/windows#android-setup for more details.
   ```
   
   安装SDK cmdline-tools 即可

4. 下图为安装正确
   
   ```
   Doctor summary (to see all details, run flutter doctor -v):
   [√] Flutter (Channel stable, 2.5.1, on Microsoft Windows [Version 10.0.17763.1577], locale zh-CN)
   [√] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
   [√] Chrome - develop for the web
   [√] Android Studio (version 3.5)
   [√] Connected device (2 available)
   
   • No issues found!
   ```

### vscode包装组件代码

ctrl+shift+r 或者右键重构 如 Wrap With Row

### 关于嵌套

同样可以用上面包装组件的方法  选择一段代码->右键重构->Extract Widget->生成组件代码块

### 示例程序

```dart
// 导入material扁平化主题
import 'package:flutter/material.dart';

//主方法
void main() {
  runApp(const MyApp());
}

//快速输入stl生成下面这个无状态组件类
class MyApp extends StatelessWidget {
  //构造方法 类似于python __init__方法 或者php里的 __constrct
  const MyApp({Key? key}) : super(key: key);

  //应用程序的根节点
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // 应用的主题
        // 运行状态下按r重新加载
        // 主题色
        primarySwatch: Colors.green,
      ),
      home: const MyHomePage(title: '头部标题'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);
  // 主页类 他有一个_MyHomePageState 对象
  // 状态的配置在_MyHomePageState build方法中
  // 子类的变量标记为常量 只能分配一次值 必须初始化
  final String title;

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  // 定义一个计数器
  int _counter = 0;

  // 定义一个计数器增加方法
  void _incrementCounter() {
    setState(() {
      //设置计时器状态,实时更新显示的值
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // 每次调用setState时都会重新运行这个方法
    return Scaffold(
      appBar: AppBar(
        // 从MyHomePage对象中获取值 并使用它来设置appbar标题
        title: Text(widget.title),
      ),
      body: Center(
        // 居中布局控件, 只有一个子节点
        child: Column(
          //列布局控件 shift+ctrl+p  ->Toggle Debug Paint 查看控件边界
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              '按钮点击次数5',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      // 右下角浮动按钮
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ), 
    );
  }
}
```

### adb连接到夜神模拟器

```powershell
adb disconnect 127.0.0.1:62001 # 断开连接
adb connect 127.0.0.1:62001
```

打开闪退就下载最新的夜神模拟器 重新复制一下nox_adb.exe

### 运行

打开模拟器，在项目根目录按F5运行 或运行 flutter run命令

?> 解决Flutter编译一直显示Running Gradle task 'assembleDebug'  
D:\flutter\packages\flutter_tools\gradle\flutter.gradle  

```java
//第一处修改
buildscript {
    repositories {
        // 注释
        // google() 
        // mavenCentral()
        // 新增
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }

    }
    dependencies {
        classpath 'com.android.tools.build:gradle:4.1.0'
    }
}

// 第二处修改
// 注释
// private static final String DEFAULT_MAVEN_HOST = "https://storage.googleapis.com";
// 新增
private static final String DEFAULT_MAVEN_HOST = "https://storage.flutter-io.cn";

// 第三处修改
rootProject.allprojects {
      repositories {
          maven {
              url repository
          }
          //新增
          maven { url 'https://maven.aliyun.com/repository/google' }
          maven { url 'https://maven.aliyun.com/repository/jcenter' }
          maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
      }
  }
```

D:\项目根目录\android\build.gradle  

```java
// 注释两处
// google()
// mavenCentral()
//新增两处
maven { url 'https://maven.aliyun.com/repository/google' }
maven { url 'https://maven.aliyun.com/repository/jcenter' }
maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
```

### 官网无限列表程序

```dart
import 'package:flutter/material.dart';

// 主函数（main）使用了 (=>) 符号，这是 Dart 中单行函数或方法的简写。
void main() => runApp(const MyApp());

// 该应用程序继承了 StatelessWidget，这将会使应用本身也成为一个 widget。在 Flutter 中，几乎所有都是 widget，包括对齐 (alignment)、填充 (padding) 和布局 (layout)
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // 一个 widget 的主要工作是提供一个 build() 方法来描述如何根据其他较低级别的 widgets 来显示自己。
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'APP标题',
      theme:  ThemeData(
          primaryColor: Colors.red,
          textTheme: const TextTheme(
              bodyText1: TextStyle(color: Colors.green),
              bodyText2: TextStyle(color: Colors.blue)),
          dividerTheme: const DividerThemeData(color: Colors.grey),
          ),
      home: const ListWidget(),
    );
  }
}

//输入stful 生成下面的代码  一个widget类和state类
//widget类
class ListWidget extends StatefulWidget {
  const ListWidget({Key? key}) : super(key: key);

  @override
  _ListWidgetState createState() => _ListWidgetState();
}

//state类
class _ListWidgetState extends State<ListWidget> {
  //列表字体样式
  final TextStyle style = const TextStyle(fontSize: 18,color: Colors.blue);
  //保存数字的列表
  final Set<String> saveList = <String>{};

  //生成一行
  Widget _buildRow(String text) {
    //是否已经添加到保存列表
    final bool alreadySaved = saveList.contains(text);
    return ListTile(
      title: Text(
        text,
        style: style,
      ),
      //添加Icon
      trailing: Icon(
        alreadySaved ? Icons.favorite : Icons.favorite_border,
        color: alreadySaved ? Colors.green : null,
      ),
      onTap: () {
        //点击事件
        setState(() {
          if (alreadySaved) {
            saveList.remove(text);
          } else {
            saveList.add(text);
          }
        });
      },
    );
  }

  //生成列表
  Widget _buildList() {
    return ListView.builder(
        padding: const EdgeInsets.all(16),
        itemBuilder: (context, i) {
          // i是 0， 2， 4 ， 6， 8 ···
          //如果是奇数 则添加分割线
          if (i.isOdd) return const Divider();
          //行号
          final index = i ~/ 2;
          return _buildRow(index.toString());
        });
  }

  //每次设置状态都会调用下面这个build
  @override
  Widget build(BuildContext context) {
    //Scaffold 是 Material 库中提供的一个 widget，它提供了默认的导航栏、标题和包含主屏幕 widget 树的 body 属性。 widget 树可以很复杂。
    return Scaffold(
      appBar: AppBar(
        title: const Text('appbar标题'),
        actions: [IconButton(onPressed: _routeTo, icon: const Icon(Icons.list))],
      ),
      body: Center(
        child: _buildList(),
      ),
    );
  }

  void _routeTo() {
    //直接push(content,route)也是可以的
    Navigator.of(context).push(
      MaterialPageRoute<void>(
        builder: (BuildContext context) {
          return Scaffold(
            appBar: AppBar(
              title: const Text('新页面'),
            ),
            body: const Center(child: Text("新页面文字 ")),
          );
        },
      ),
    );
  }
}
```

### 使用pub获取依赖

```
flutter pub get
```

!> vscode中安装插件后每次保存配置文件 pubspec.yaml 会自动获取依赖

### 公共头部

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}
```

### 使用MaterialApp和设置主题

```dart
// 导入material扁平化主题
import 'package:flutter/material.dart';

//主方法
void main() => runApp(const MyApp());

//快速输入stl生成下面这个无状态组件类
class MyApp extends StatelessWidget {
  //构造方法 类似于python __init__方法 或者php里的 __constrct
  const MyApp({Key? key}) : super(key: key);

  //实现抽象方法 build 返回一个组件
  @override
  Widget build(BuildContext context) {
    //使用 主题组件
    return  MaterialApp(
      title: 'app标题',
      theme: ThemeData(primarySwatch: Colors.green),
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: const Text('状态栏标题'),
        ),
        body: const HomePage(),
      )
    );
  }
}

//主页
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //Ceter组件
    return const Center(
      child: Text(
        'hello',
        //文字方向
        textDirection: TextDirection.ltr,
        style: TextStyle(
          fontSize: 100,
          color: Color.fromARGB(255, 255, 144, 255), //Colors.red or Color.fromRGBO(255, 144, 255, 1)
        ),
      ),
    );
  }
}
```

### Container容器组件

container默认就是全屏的容器

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //定义一个Container组件
    return Container(
      //宽度
      width: 200,
      //高度
      height: 200,
      //外边距
      margin:
          const EdgeInsets.fromLTRB(20, 40, 20, 40), //const EdgeInsets.all(10),
      //内边距
      padding: const EdgeInsets.fromLTRB(2, 4, 2,
          4), //const EdgeInsets.only(left: 20,top: 40,right: 20,bottom: 40),//const EdgeInsets.all(10),
      //变形 平移
      transform: Matrix4.translationValues(10, 0, 0),
      //内容对齐方式 这里无效是因为下面用了FractionallySizedBox 去掉就能看效果
      alignment: Alignment.bottomCenter,
      //背景颜色 和整个decoration 只能设置一个
      // color: Colors.green,
      //定义样式
      decoration: BoxDecoration(
          //背景颜色
          color: Colors.green,
          //边框
          border: Border.all(color: Colors.grey, width: 1),
          //边框圆角 see from https://api.flutter-io.cn/flutter/painting/BoxDecoration-class.html
          borderRadius: BorderRadius.circular(20)), //BorderRadius.circular(20),
      // 下面这种单独设置每条边
      // borderRadius: const BorderRadius.only(
      //     bottomLeft: Radius.circular(10.0),
      //     bottomRight: Radius.circular(10.0))),
      //子组件内容 100% 撑满
      child: Row(
        //对齐方式
        mainAxisAlignment: MainAxisAlignment.end,
        //子元素
        children: const [Text('hello Container')],
      ),
    );
  }
}
```

### FractionallySizedBox宽度高度百分比组件

```dart
FractionallySizedBox (
  widthFactor: 1/3,
  heightFactor: 1/3,
  child:Text("123"),
)
```

### Text文本组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        width: 300,
        height: 300,
        decoration: BoxDecoration(
            color: Colors.green,
            border: Border.all(color: Colors.grey, width: 1)),
        //定义一个Text组件
        child: const Text(
          //定义标题
          'hello world hello world hello world hello world hello world hello world',
          //定义对齐
          textAlign: TextAlign.left,
          //溢出省略号
          overflow: TextOverflow.ellipsis,
          //最大行
          maxLines: 2,
          //缩放字体
          textScaleFactor: 2,
          //定义文字样式
          style: TextStyle(
            //定义字体大小
            fontSize: 24,
            //定义文字颜色
            color: Color.fromRGBO(255, 255, 255, 1),//Colors.white,
            //加粗
            fontWeight: FontWeight.bold,
            //倾斜
            fontStyle: FontStyle.italic,
            //下划线
            decoration: TextDecoration.underline,
            //下划线颜色
            decorationColor: Colors.yellow,
            //下划线风格虚线
            decorationStyle: TextDecorationStyle.dashed,
            //字间距
            letterSpacing: 5
          ),
        ),
      ),
    );
  }
}
```

### Image图片组件

#### 引入网络图片

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        width: 200,
        height: 200,
        decoration: const BoxDecoration(
          color: Colors.yellow,
        ),
        //引用网络图片
        child: Image.network(
          'https://v3.cn.vuejs.org/logo.png',
          alignment: Alignment.topRight,
          //图片适应父组件方式  cover:等比缩放水平垂直直到2者都填满父组件 其他的没啥用了
          fit: BoxFit.cover,
          //加滤镜
          color: Colors.blue,
          colorBlendMode: BlendMode.srcIn,
        ),
      ),
    );
  }
}
```

#### 引入本地图片

首先要设置图片文件夹和图片路径

1. 项目根目录新建images/2.0x;images/3.0x;images/3.0x 文件夹 分别把图片放入三个文件夹以及images目录

2. 配置文件pubspec.yaml
   
   ```html
   flutter:
     assets:
       - images/
   ```
   
    这样配置可以匹配所有图片了,不用一个一个加的
   3.引入本地图片
   
   ```dart
   class HomePage extends StatelessWidget {
     const HomePage({Key? key}) : super(key: key);
   
     @override
     Widget build(BuildContext context) {
       return Center(
         child: Container(
           width: 200,
           height: 200,
           decoration: const BoxDecoration(
             color: Colors.yellow,
           ),
           //引用本地图片
           child: Image.asset('images/logo.png',fit: BoxFit.cover,),
         ),
       );
     }
   }
   ```

#### 图片圆角

第一种 采用设置背景图片方式（不推荐）

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        width: 200,
        height: 200,
        decoration:  BoxDecoration(
          color: Colors.yellow,
          //使用圆形图片第一种
          borderRadius: BorderRadius.circular(200),
          image: const DecorationImage(
              image: NetworkImage('https://v3.cn.vuejs.org/logo.png'),
              fit: BoxFit.cover)
        ),
      ),
    );
  }
}
```

第二种 采用裁剪组件（推荐）
这个直接就是圆形了

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      // 使用圆形图片第二种 推荐这种 相当于一个Imgaeview组件
      child: ClipOval(
        child: Image.network(
          'https://v3.cn.vuejs.org/logo.png',
          alignment: Alignment.center,
          //图片适应父组件方式  cover:等比缩放水平垂直直到2者都填满父组件 其他的没啥用了
          fit: BoxFit.cover,
          width: 100,
          height: 100,
        ),
      ),
    );
  }
}
```

这个是可以自定义圆角

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ClipRRect(
        borderRadius: BorderRadius.circular(20),
        child: Image.network(
          'https://v3.cn.vuejs.org/logo.png',
          alignment: Alignment.center,
          //图片适应父组件方式  cover:等比缩放水平垂直直到2者都填满父组件 其他的没啥用了
          fit: BoxFit.cover,
          width: 100,
          height: 100,
        ),
      ),
    );
  }
}
```

### SingleChildScrollView滚动视图

```dart
Scrollbar(
  child: SingleChildScrollView(
    padding: const EdgeInsets.all(10),
    child:const Text("123"),
  )
)
```

### ListView列表组件

#### ListTile 左侧图标中间标题右侧图标组件

嵌套在ListView当中的列表项组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return ListView(
      //内边距
      padding: const EdgeInsets.all(10),
      //垂直列表 水平列表有滚动条哦
      scrollDirection: Axis.vertical,
      //children可以放任意的组件
      children: [
        ListTile(
          //标题
          title: const Text('列表标题列表标题列表标题列表标题列表标'),
          //副标题
          subtitle: const Text('副标题副标题副标题副标题副标'),
          //右侧icon
          trailing: const Icon(Icons.more_vert),
          //左侧icon 网络图片
          leading: Image.network('https://v3.cn.vuejs.org/logo.png'),
          //左侧icon 图标
          // leading: Icon(
          //   Icons.badge,
          //   color: Colors.blue,
          //   size: 50,
          // ),
          onTap: () => {},
        )
      ],
    );
  }
}
```

#### 使用map映射请求返回数据(推荐)

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //声明一个列表
    List list = [
      {'id': 1, "name": '张三'},
      {'id': 2, "name": '李四'},
      {'id': 3, "name": '王五'},
      {'id': 4, "name": '小六'},
      {'id': 5, "name": '老七'},
    ];
    //使用map映射一个widget List
    return ListView(children: list.map((item) => Text(item['name'])).toList());
  }
}
```

#### 使用ListView.builders创建(推荐)

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //声明一个列表
    List list = [
      {'id': 1, "name": '张三'},
      {'id': 2, "name": '李四'},
      {'id': 3, "name": '王五'},
      {'id': 4, "name": '小六'},
      {'id': 5, "name": '老七'},
    ];

    //使用ListView.builder 创建listView
    return ListView.builder(
        //list长度必填
        itemCount: list.length,
        //创建回调函数
        itemBuilder: (context, index) =>
            ListTile(title: Text(list[index]['name'])));
  }
}
```

### CustomScrollView可下拉刷新的滚动视图

以下滚动皆可添加下拉刷新，pull-to-refresh  
普通列表滚动

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomScrollView(
      // 不要滚动特效，不然会整页滚动
      // physics: const PageScrollPhysics(),
      slivers: <Widget>[
        //SliverList 列表 还有其他的SliverFixedExtentList SliverGrid等
        SliverList(
            delegate: SliverChildListDelegate([
          const Center(child: Text("1")),
          //使用...扩展符放入
          ...[1, 2, 3].map((e) => const Text('e')).toList()
        ]))
      ],
    );
  }
}
```

抖音视频滚动

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CustomScrollView(
      physics: const PageScrollPhysics(),
      slivers: <Widget>[
        SliverFillViewport(
            delegate: SliverChildListDelegate(const [
          Center(child: Text("第一页")),
          Center(child: Text("第二页")),
          Center(child: Text("第三页")),
          Center(child: Text("第四页"))
        ]))
      ],
    );
  }
}
```

### GirdView网格布局

#### GirdView.count 创建网格布局(推荐)

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //声明一个列表
    List list = [
      {'id': 1, "name": '张三'},
      {'id': 2, "name": '李四'},
      {'id': 3, "name": '王五'},
      {'id': 4, "name": '小六'},
      {'id': 5, "name": '小七'},
    ];

    //使用GirdView.count 创建网格布局
    return GridView.count(
      //2列
      crossAxisCount: 2,
      //水平间距
      crossAxisSpacing: 10,
      //垂直间距
      mainAxisSpacing: 10,
      //内边距
      padding: const EdgeInsets.all(10),
      //子元素宽高比
      childAspectRatio: 0.8,
      //子元素
      children: list
          .map((item) => Container(
                decoration: const BoxDecoration(
                  //背景颜色
                  color: Colors.green,
                ),
                child: Text(item['name']),
              ))
          .toList(),
    );
  }
}
```

#### 使用GirdView.builder 创建网格布局(不推荐，麻烦)

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //声明一个列表
    List list = [
      {'id': 1, "name": '张三'},
      {'id': 2, "name": '李四'},
      {'id': 3, "name": '王五'},
      {'id': 4, "name": '小六'},
      {'id': 5, "name": '小七'},
    ];

    //使用GirdView.builder 创建网格布局
    return GridView.builder(
      //数组长度 必须
      itemCount: list.length,
      //创建回调函数
      itemBuilder: (context, index) => Container(
        decoration: const BoxDecoration(
          //背景颜色
          color: Colors.green,
        ),
        child: Text(list[index]['name']),
      ),
      padding: const EdgeInsets.all(10),
      //配置GridView参数 同上面的GirdView.count 参数一样
      gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
        //2列
        crossAxisCount: 2,
        //水平间距
        crossAxisSpacing: 10,
        //垂直间距
        mainAxisSpacing: 10,
      ),
    );
  }
}
```

### Padding边距组件

用于没有padding属性设置的组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //使用Padding组件直接 加内边距
    return Padding(
        //设置padding值
        padding: const EdgeInsets.all(100),
        child: Container(
          width: 100,
          height: 100,
          decoration: const BoxDecoration(color: Colors.pink),
        ));
  }
}
```

### Row水平布局组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.white,
      padding: const EdgeInsets.all(10),
      //行组件 里面的元素是一列一列横着排列的 对应CSS flex水平布局
      child: Row(
        //相当于css justifly
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        //相当于css align-item
        crossAxisAlignment: CrossAxisAlignment.stretch,
        //行里面的子节点
        children: [
          Container(width: 100, height: 400, color: Colors.red),
          Container(width: 100, height: 400, color: Colors.blue),
          //flex自适应
          Expanded(
              flex: 1,
              child: Container(width: 100, height: 400, color: Colors.pink)),
        ],
      ),
    );
  }
}
```

### Column垂直布局组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.white,
      padding: const EdgeInsets.all(10),
      //列组件 里面的元素是竖着一行一行排列的 对应CSS flex垂直布局
      child: Column(
        //相当于css justifly
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        //相当于css align-item
        crossAxisAlignment: CrossAxisAlignment.stretch,
        //列里面的子节点
        children: [
          Container(width: 400, height: 100, color: Colors.red),
          Container(width: 400, height: 100, color: Colors.blue),
          Container(width: 400, height: 100, color: Colors.pink),
        ],
      ),
    );
  }
}
```

### Stack堆叠布局

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //堆叠布局
    return Stack(
      //堆叠内容对齐方式
      alignment: Alignment.centerRight,
      //子节点
      children: [
        Container(
          height: 500,
          color: Colors.red,
          padding: const EdgeInsets.all(10),
        ),
        Container(
          width: 40,
          height: 40,
          color: Colors.green,
          padding: const EdgeInsets.all(10),
        ),
        Container(
          width: 20,
          height: 20,
          color: Colors.blue,
          padding: const EdgeInsets.all(10),
        ),
      ],
    );
  }
}
```

### 定位方式

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Stack(
      //子节点
      children: [
        Container(
          height: 500,
          color: Colors.red,
        ),
        //空盒子 用作相对定位父级
        SizedBox(
          height: 500,
          child:
              //Align组件 相对布局相当于position:relative 数值设置-1 ~ 1 以组件中心为0，0设置百分比
              Align(
            alignment: const Alignment(0.2, 0.2), //Alignment.bottomCenter,
            child: Container(
              width: 50,
              height: 50,
              color: Colors.green,
            ),
          ),
        ),
        //绝对定位
        Positioned(
            right: 10,
            bottom: 10,
            child: Container(
              width: 50,
              height: 50,
              color: Colors.blue,
            )),
      ],
    );
  }
}
```

### AspectRatio宽高比组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //定义一个AspectRatio组件 设置子节点的宽高比组件 可用于海报图等
    return AspectRatio(
      aspectRatio: 3 / 1,
      child: Image.network(
        'https://placeholder.idcd.com/?w=900&h=300&bgcolor=%236c757d&fontcolor=%23d3d3d3&fontsize=20&fontfamily=1',
        fit: BoxFit.cover,
      ),
    );
  }
}
```

### Card卡片组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //声明一个列表
    List list = [
      {'id': 1, "name": '张三', "address": "XXX省XXX市XXX区XXX街道XXX楼404"},
      {'id': 2, "name": '李四', "address": "XXX省XXX市XXX区XXX街道XXX楼404"},
      {'id': 3, "name": '王五', "address": "XXX省XXX市XXX区XXX街道XXX楼404"},
      {'id': 4, "name": '小六', "address": "XXX省XXX市XXX区XXX街道XXX楼404"},
      {'id': 5, "name": '老七', "address": "XXX省XXX市XXX区XXX街道XXX楼404"},
    ];
    //使用map映射一个widget List
    return ListView(
        children: list.map((item) {
      //声明一个卡片组件
      return Card(
        //外边距
        margin: const EdgeInsets.fromLTRB(10, 10, 10, 0),
        //圆角 必须要定义下面2项才能控制4个圆角
        shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
        clipBehavior: Clip.antiAlias,
        child: Column(
          children: [
            AspectRatio(
              aspectRatio: 3 / 1,
              child: Image.network(
                'https://placeholder.idcd.com/?w=900&h=300&bgcolor=%236c757d&fontcolor=%23d3d3d3&fontsize=20&fontfamily=1',
                fit: BoxFit.cover,
              ),
            ),
            ListTile(
              //标题
              title: Text(item['name']),
              //副标题
              subtitle: Text(item['address']),
              //左侧icon 圆形头像还能用这种
              leading: const CircleAvatar(
                  backgroundImage:
                      NetworkImage('https://v3.cn.vuejs.org/logo.png')),
              onTap: () => {},
            )
          ],
        ),
      );
    }).toList());
  }
}
```

### Wrap流式布局

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(10.0),
      //flex流式布局
      child: Wrap(
        //从左到右排列
        direction: Axis.horizontal,
        //水平间距
        spacing: 10,
        //垂直间距 此值可以设置为负数 以减小上下之间的间距 不然默认的0有点大
        runSpacing: -10,
        //相当于水平方向上的 justifly-content
        alignment: WrapAlignment.start,
        //相当于垂直方向上的 align-item
        runAlignment: WrapAlignment.start,
        children: const [
          TextButtonComponent('分类1'),
          TextButtonComponent('分类22'),
          TextButtonComponent('分类333'),
          TextButtonComponent('分类4444'),
          TextButtonComponent('分类55555'),
          TextButtonComponent('分类666666'),
          TextButtonComponent('分类7777777'),
        ],
      ),
    );
  }
}

//封装一个Button组件
class TextButtonComponent extends StatelessWidget {
  //定义属性 要用final关键字 可以参数用?表示
  //如果可选参数不加问号，则必须在构造函数中初始化赋值
  final String text; //必选参数

  //声明构造函数及里面的需要传入的属性 {}内的表示可选参数
  const TextButtonComponent(
    this.text, {
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //文字按钮
    return TextButton(
      style: ButtonStyle(
        //按钮大小
        minimumSize: MaterialStateProperty.all(const Size(60, 30)),
        //内边距
        padding:
            MaterialStateProperty.all(const EdgeInsets.fromLTRB(10, 4, 10, 4)),
        //边框
        side: MaterialStateProperty.all(
            const BorderSide(color: Colors.red, width: 1)),
        //圆角
        shape: MaterialStateProperty.all(
            RoundedRectangleBorder(borderRadius: BorderRadius.circular(4))),
        //背景
        backgroundColor: MaterialStateProperty.all(Colors.transparent),
        //点击时背景
        overlayColor: MaterialStateProperty.all(Colors.red[50]),
      ),
      onPressed: () {},
      child: Text(
        text,
        style: const TextStyle(color: Colors.red, fontSize: 12),
      ),
    );
  }
}
```

### StatefulWidget状态组件

基本用这种就行了

```dart
//基本计时器状态组件
class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //定义状态相当于vue data属性
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        //标签组件
        Chip(
          label: Text(count.toString()),
        ),
        //按钮组件
        ElevatedButton(
            onPressed: () {
              //需要通知组件画面更新的要写在这里面
              setState(() {
                count++;
              });
            },
            child: const Text('计数器+1'))
      ],
    );
  }
}
```

### TextButton文字按钮组件

```dart
class TextButtonComponent extends StatelessWidget {
  //定义属性 要用final关键字 可以参数用?表示
  //如果可选参数不加问号，则必须在构造函数中初始化赋值
  final String text; //必选参数
  final VoidCallback onPressed;

  //声明构造函数及里面的需要传入的属性 {}内的表示可选参数
  const TextButtonComponent({
    this.text = '',
    required this.onPressed,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //文字按钮
    return TextButton(
      style: ButtonStyle(
        //按钮大小
        minimumSize: MaterialStateProperty.all(const Size(60, 30)),
        //内边距
        padding:
            MaterialStateProperty.all(const EdgeInsets.fromLTRB(10, 4, 10, 4)),
        //边框
        side: MaterialStateProperty.all(
            const BorderSide(color: Colors.red, width: 1)),
        //圆角
        shape: MaterialStateProperty.all(
            RoundedRectangleBorder(borderRadius: BorderRadius.circular(4))),
        //背景
        backgroundColor: MaterialStateProperty.all(Colors.transparent),
        //点击时背景
        overlayColor: MaterialStateProperty.all(Colors.red[50]),
      ),
      onPressed: onPressed,
      child: Text(
        text,
        style: const TextStyle(color: Colors.red, fontSize: 12),
      ),
    );
  }
}
```

### ElevatedButton按钮组件

```dart
class ElevatedButtonComponent extends StatelessWidget {
  //定义属性 要用final关键字 可以参数用?表示
  //如果可选参数不加问号，则必须在构造函数中初始化赋值
  final String text; //必选参数
  final VoidCallback onPressed;

  //声明构造函数及里面的需要传入的属性 {}内的表示可选参数
  const ElevatedButtonComponent({
    this.text = '',
    required this.onPressed,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //文字按钮
    return ElevatedButton(
      style: ButtonStyle(
        //按钮大小
        minimumSize: MaterialStateProperty.all(const Size(60, 30)),
        //内边距
        padding:
            MaterialStateProperty.all(const EdgeInsets.fromLTRB(10, 4, 10, 4)),
        //边框 实际使用去掉这个边框
        //side: MaterialStateProperty.all(
        //    const BorderSide(color: Colors.red, width: 1)),
        //圆角
        shape: MaterialStateProperty.all(
            RoundedRectangleBorder(borderRadius: BorderRadius.circular(4))),
        //背景
        backgroundColor: MaterialStateProperty.all(Colors.red),
        //点击时背景
        overlayColor: MaterialStateProperty.all(Colors.red[300]),
      ),
      onPressed: onPressed,
      child: Text(
        text,
        style: const TextStyle(color: Colors.white, fontSize: 12),
      ),
    );
  }
}
```

### Navigator路由

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        //名为"/"的路由作为应用的home(首页)
        initialRoute: "/",
        //路由数组 设置在这里面就不能被拦截了 所以不要写这里，除非你不需要自定义动画
        // routes: {
        //   '/new_page': (context) => const NewPage(''),
        //   '/': (context) => Scaffold(
        //         appBar: AppBar(
        //           title: const Text('状态栏标题'),
        //         ),
        //         body: const HomePage(),
        //       )
        // },
        //如果路由不在上面的routes里面，那么才会调用这个拦截
        //使用这个拦截可以统一路由动画
        onGenerateRoute: (settings) => Router().generateRoute(settings));
  }
}

//抽离路由代码
class Router {
  //没有传参的路由全部放这里
  final routes = {
    '/': Scaffold(
      appBar: AppBar(
        title: const Text('状态栏标题'),
      ),
      body: const HomePage(),
    ),
    '/auth': const Scaffold(
        body: Center(
      child: Text("登录后页面"),
    )),
    '/new_page': (content, {arguments}) => NewPage(arguments)
  };

  generateRoute(settings) {
    //前面可以根据settings.name 进行路由鉴权
    // 如果访问的路由页需要登录，但当前未登录，则直接返回登录页路由，
    // 引导用户登录；其它情况则正常打开路由。

    //映射路由
    var pageBuilder = routes[settings.name];
    if (pageBuilder != null) {
      if (settings.arguments != null) {
        //如果有参数
        return CupertinoPageRoute(
            builder: (context) => (pageBuilder as Function)(context,
                arguments: settings.arguments) as Widget);
      } else {
        //如果没参数
        return CupertinoPageRoute(builder: (context) => pageBuilder as Widget);
      }
    } else {
      //定义没有匹配到的路由
      return CupertinoPageRoute(
          builder: (context) => const Scaffold(
                  body: Center(
                child: Text("Page not found"),
              )));
    }
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Column(
        //点击按钮跳转到新路由
        children: [
          ElevatedButton(
            child: const Text('跳转组件传值'),
            onPressed: () {
              _routeTo();
            },
          ),
          ElevatedButton(
            child: const Text('命名路由跳转'),
            onPressed: () {
              _routeToName();
            },
          ),
          ElevatedButton(
            child: const Text('删除所有并替换'),
            onPressed: () {
              _routeToAndRemove();
            },
          ),
        ]);
  }

  //路由方法 除非要返回值 一般不用这种
  void _routeTo() async {
    //直接push(content,route)也是可以的
    var res = await Navigator.of(context).push(
      CupertinoPageRoute(
        //设置为true 变为弹窗类型路由 返回按钮变为x关闭icon
        fullscreenDialog: false,
        builder: (context) {
          return const NewPage('标题传值');
        },
      ),
    );
    //路由返回值
    // ignore: avoid_print
    print(res);
  }

  //命名路由跳转
  void _routeToName() {
    //直接push(content,route)也是可以的
    Navigator.of(context).pushNamed('/new_page', arguments: '标题传值');
  }

  //删除所有路由 返回指定路由 不会保存状态
  void _routeToAndRemove() {
    Navigator.pushAndRemoveUntil(
        context,
        CupertinoPageRoute(builder: (context) => const NewPage('标题传值')),
        (route) => false);
  }
}

//新页面组件
class NewPage extends StatelessWidget {
  final String title;
  const NewPage(this.title, {Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //获取路由参数 不使用路由拦截的才能获得,不然是null
    final args = ModalRoute.of(context)!.settings.arguments;
    // ignore: avoid_print
    // print(args);

    //返回首页
    void _routeToIndex() {
      //这种会黑屏 因为首页用了pushAndRemoveUntil
      // Navigator.popUntil(context, ModalRoute.withName('/'));
      //推荐这种不会黑屏
      Navigator.popUntil(context, (route) => route.isFirst);
    }

    return Scaffold(
      appBar: AppBar(
        title: Text(title == '' ? args.toString() : title),
      ),
      //浮动按钮返回 如果没有AppBar，可以加这个
      floatingActionButton: FloatingActionButton(
        child: const Text('返回'),
        onPressed: () {
          //返回传值方法 第二个参数是返回值 不适应用默认的返回按钮和手势返回
          Navigator.pop(context, true);
        },
      ),
      body: Column(children: [
        const Text("新页面文字 "),
        ElevatedButton(
          child: const Text('返回主页'),
          onPressed: () {
            _routeToIndex();
          },
        ),
      ]),
    );
  }
}
```

### 位于顶部的Tabbar组件

主页设置顶部tabbar 设置后就没地方设置底部啦

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    List tabs = ["新闻", "历史", "图片", "美女", "军事", "母婴", "本地"];
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        home: DefaultTabController(
          length: tabs.length,
          child: Scaffold(
            appBar: AppBar(
              title: const Text("App Name"),
              //标题样式
              titleTextStyle:
                  const TextStyle(fontSize: 14, color: Colors.white),
              //标题居中
              centerTitle: true,
              //状态栏高度
              toolbarHeight: 36,
              bottom: TabBar(
                isScrollable: true,
                tabs: tabs.map((e) => Tab(text: e)).toList(),
                onTap: (index) {
                  // ignore: avoid_print
                  print(index);
                },
              ),
            ),
            body: TabBarView(
              //构建
              children: tabs.map((e) {
                return Container(
                  alignment: Alignment.center,
                  child: Text(e, textScaleFactor: 5),
                );
              }).toList(),
            ),
          ),
        ));
  }
}
```

顶部tabbar路由页面案例

```dart
//单独页面使用Tabbar组件案例 使用直接作为一个路由页面
class TabbarScrollComponent extends StatelessWidget {
  const TabbarScrollComponent({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    List tabs = ["新闻", "历史", "图片", "美女", "军事", "母婴", "本地"];
    return DefaultTabController(
      length: tabs.length,
      child: Scaffold(
        appBar: AppBar(
          title: const Text("App Name"),
          bottom: TabBar(
            isScrollable: true,
            tabs: tabs.map((e) => Tab(text: e)).toList(),
          ),
        ),
        body: TabBarView(
          //构建
          children: tabs.map((e) {
            return Container(
              height: 1500,
              alignment: Alignment.center,
              child: Text(e, textScaleFactor: 5),
            );
          }).toList(),
        ),
      ),
    );
  }
}
```

### Drawer抽屉

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        routes: {
          '/new_page': (context) => const NewPage(),
        },
        home: Scaffold(
          appBar: AppBar(
            title: const Text('主状态栏标题'),
          ),
          body: HomePage(),
        ));
  }
}

class HomePage extends StatelessWidget {
  //声明一个列表
  final List list = [
    {'id': 1, "name": '张三'},
    {'id': 2, "name": '李四'},
    {'id': 3, "name": '王五'},
    {'id': 4, "name": '小六'},
    {'id': 5, "name": '老七'},
  ];

  HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('状态栏标题'),
      ),

      //左侧抽屉
      drawer: Drawer(
        child: Padding(
          padding: const EdgeInsets.all(40),
          child: ListView(
              children: list
                  .map((item) => ListTile(
                        title: Text(item['name']),
                        onTap: () {
                          Navigator.pop(context);
                          Navigator.pushNamed(context, '/new_page');
                        },
                      ))
                  .toList()),
        ),
      ),
      //右侧抽屉
      endDrawer: Drawer(
        child: Padding(
          padding: const EdgeInsets.all(40),
          child: Column(
            children: const [Text('右侧抽屉'), Divider(), Text('右侧抽屉')],
          ),
        ),
      ),
      body: const Text('内页'),
    );
  }
}

class NewPage extends StatelessWidget {
  const NewPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('新页面'),
      ),
      //浮动按钮返回 如果没有AppBar，可以加这个
      floatingActionButton: FloatingActionButton(
        child: const Text('返回'),
        onPressed: () {
          //返回传值方法 第二个参数是返回值 不适应用默认的返回按钮和手势返回
          Navigator.pop(context, true);
        },
      ),
      body: Column(children: const [Text("新页面文字 ")]),
    );
  }
}
```

### Toast提示窗

[官方文档](https://pub.flutter-io.cn/packages/fluttertoast)

### 基本app布局

main.dart

```dart
import 'package:flutter/material.dart';
import './tabbar.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        home: const TabbarComponent());
  }
}
```

appbar.dart

```dart
//appbar组件 必须实现PreferredSizeWidget接口
import 'package:flutter/material.dart';

class AppBarComponent extends StatelessWidget implements PreferredSizeWidget {
  final String title;
  const AppBarComponent(this.title, {Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //appbar组件
    return AppBar(
        //标题
        title: Text(title),
        centerTitle: true,
        titleTextStyle: const TextStyle(fontSize: 14, color: Colors.white));
  }

  @override
  // implement preferredSize 实现抽象类PreferredSizeWidget里的抽象方法
  Size get preferredSize => const Size.fromHeight(36.0);
}
```

tabbar.dart

```dart
import 'package:flutter/material.dart';

import './appbar.dart';
import './home.dart';
import './category.dart';
import './user.dart';

class TabbarComponent extends StatefulWidget {
  const TabbarComponent({Key? key}) : super(key: key);

  @override
  _TabbarComponentState createState() => _TabbarComponentState();
}

class _TabbarComponentState extends State<TabbarComponent> {
  //当前激活路由索引
  int currentIndex = 0;
  //tabbar路由列表
  final List router = [
    const HomeComponent(),
    const CategoryComponent(),
    const UserComponent()
  ];
  //tabbar路由标题列表
  final List appBarTitle = ['主页', '全部分类', '我的'];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBarComponent(appBarTitle[currentIndex]),
      body: router[currentIndex],
      //tabbar
      bottomNavigationBar: BottomNavigationBar(
        //图标大小
        iconSize: 26,
        //当前激活项
        currentIndex: currentIndex,
        //布局类型
        type: BottomNavigationBarType.fixed,
        //选中字体 默认是14 这里不要放大
        selectedFontSize: 12,
        //子节点
        items: const [
          BottomNavigationBarItem(label: '主页', icon: Icon(Icons.home)),
          BottomNavigationBarItem(label: '分类', icon: Icon(Icons.category)),
          BottomNavigationBarItem(label: '我的', icon: Icon(Icons.person)),
        ],
        //切换事件
        onTap: (int index) {
          setState(() {
            currentIndex = index;
          });
        },
      ),
    );
  }
}
```

home.dart

```dart
import 'package:flutter/material.dart';

class HomeComponent extends StatefulWidget {
  const HomeComponent({Key? key}) : super(key: key);

  @override
  _HomeComponentState createState() => _HomeComponentState();
}

class _HomeComponentState extends State<HomeComponent> {
  @override
  Widget build(BuildContext context) {
    return const Text('home');
  }
}
```

category.dart

```dart
import 'package:flutter/material.dart';

class CategoryComponent extends StatefulWidget {
  const CategoryComponent({Key? key}) : super(key: key);

  @override
  _CategoryComponentState createState() => _CategoryComponentState();
}

class _CategoryComponentState extends State<CategoryComponent> {
  @override
  Widget build(BuildContext context) {
    return const Text('category');
  }
}
```

user.dart

```dart
import 'package:flutter/material.dart';

class UserComponent extends StatefulWidget {
  const UserComponent({Key? key}) : super(key: key);

  @override
  _UserComponentState createState() => _UserComponentState();
}

class _UserComponentState extends State<UserComponent> {
  @override
  Widget build(BuildContext context) {
    return const Text('user');
  }
}
```

### 组件封装

封装一个StatelessWidget自定义组件示例，这里封装了Icon组件

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //使用封装的Icon组件
    return const IconComponent(
      Icons.home,
      //颜色可不传
      color: Colors.red
    );
  }
}

//封装一个Icon组件
class IconComponent extends StatelessWidget {
  //定义属性 要用final关键字 可以参数用?表示 
  //如果可选参数不加问号，则必须在构造函数中初始化赋值
  final IconData icon;//必选参数
  final Color?color;//可选参数

  //声明构造函数及里面的需要传入的属性 {}内的表示可选参数
  const IconComponent(
    this.icon, {
    this.color=Colors.white,
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 50,
      height: 50,
      color: Colors.green,
      alignment: Alignment.center,
      child: Icon(
        icon,
        color: color,
      ),
    );
  }
}
```

### http网络请求库Dio封装

使用 Dio 请求库  
依赖文件pubspec.yaml

```ini
dependencies:
  dio: ^4.0.0 
  fluttertoast: ^8.0.8
```

main.dart

```dart
import 'package:flutter/material.dart';
import 'package:fluttertoast/fluttertoast.dart';
import './request.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final res =
      Request.http(url: '/User/getInfo', type: 'get', data: {}).then((res) {
    if (res.data["code"] != 20000) {
      Fluttertoast.showToast(
        msg: res.data["message"],
      );
    }
    // ignore: avoid_print
    print(res);
  }).catchError((error) {
    Fluttertoast.showToast(
      msg: "请求服务器错误",
    );
  });

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

request.dart

```dart
import 'dart:io';

import 'package:dio/adapter.dart';
import 'package:dio/dio.dart';

class Request {
  //初始化请求配置
  static final BaseOptions _baseOptions = BaseOptions(
      //基础url
      baseUrl: "http://www.api.com",
      //请求数据类型
      contentType: "application/json; charset=utf-8",
      //超时时间ms
      connectTimeout: 5000);

  static http({
    url,
    type,
    data,
  }) async {
    //组装get参数
    if (type == 'get') {
      if (data != null && !data.isEmpty) {
        url += "?";
        data.forEach((key, value) {
          url += key.toString() + "=" + value.toString() + "&&";
        });
        url = url.replaceRange(url.length - 2, url.length, '');
        data = {};
      }
    }
    Dio dio = Dio(_baseOptions);
    //解决夜神模拟器无法访问本地host映射域名问题
    (dio.httpClientAdapter as DefaultHttpClientAdapter).onHttpClientCreate =
        (client) {
      //这一段是解决安卓https抓包的问题
      client.badCertificateCallback =
          (X509Certificate cert, String host, int port) {
        return Platform.isAndroid;
      };
      client.findProxy = (uri) {
        return "PROXY 192.168.1.10:80";
      };
    };
    //拦截器
    dio.interceptors.add(InterceptorsWrapper(onRequest: (options, handler) {
      //请求拦截 token 附加方法暂时不需要 options.headers['X-Token'] = 'token string';
      return handler.next(options);
    }, onResponse: (response, handler) {
      //响应拦截 这里创建不了DioError进不了reject 所以错误代码全部在then后面处理
      return handler.resolve(response);
    }, onError: (DioError e, handler) {
      //出错拦截
      return handler.reject(e);
    }));

    //返回结果
    Response response;
    //捕获异常
    try {
      response = await dio.request(url,
          data: data ?? {}, options: Options(method: type));
      return response;
    } on DioError catch (e) {
      if (e.response!.statusCode != null) {
        return e.response!.statusCode;
      } else {
        return "其他未知错误";
      }
    }
  }
}
```

### shared_preferences本地存储

相当于Localstroage  
依赖

```ini
dependencies:
  shared_preferences: ^2.0.8
```

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //定义一个语言
  String lang = 'cn';

  //初始化状态 相当于mounted
  @override
  void initState() {
    super.initState();
    _getLocalStorage();
  }

  //获取本地缓存并设置当前语言
  void _getLocalStorage() async {
    //获取localstorage 实例
    final prefs = await SharedPreferences.getInstance();
    //设置状态
    setState(() {
      //读取本地缓存
      lang = prefs.getString('lang') ?? 'cn';
      // ignore: avoid_print
      print(lang);
    });
  }

  //设置本地缓存
  void _setLocalStorage() async {
    //获取localstorage 实例
    final prefs = await SharedPreferences.getInstance();
    //设置本地缓存
    setState(() {
      prefs.setString('lang', lang == 'cn' ? 'en' : 'cn');
      lang = lang == 'cn' ? 'en' : 'cn';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        children: [
          Text(lang),
          ElevatedButton(
              onPressed: () {
                _setLocalStorage();
              },
              child: const Text("切换语言")),
          ElevatedButton(
              onPressed: () {
                _getLocalStorage();
              },
              child: const Text("获取语言"))
        ],
      ),
    );
  }
}
```

### Provider状态管理

按1，2，3，4，5的步骤创建状态

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

//1.创建一个User状态管理类 别的地方都叫Model不好理解  其实就是一个全局状态管理类 实际把这个类拿出来放到store文件夹
class UserState with ChangeNotifier {
  //比如你在这里记录用户昵称状态
  String nickname = '';

  //获取状态方法 相当于vue里的 store getter
  String get name => nickname;

  //改变用户昵称状态值 相当于vue里的 store action
  void changeNickName(String name) {
    nickname = name;
    //5.通知外部使用状态的widget组件 用户昵称状态更新了 必须使用
    notifyListeners();
  }
}

//2.MultiProvide直接用多状态管理
void main() => runApp(MultiProvider(
      //MultiProvider提供多个状态管理
      providers: [
        //ChangeNotifierProvider是通知widget组件状态更新的类 后面接受一个用户状态UserState
        ChangeNotifierProvider(create: (context) => UserState()),
      ],
      child: const MyApp(),
    ));

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
          onPressed: () {
            //4.调用UserState里面change方法去设置状态
            Provider.of<UserState>(context, listen: false)
                .changeNickName('fultter provider');
          },
          child: const Text("设置昵称"),
        ),
        //3.获取状态值 因为在2已经包裹了MultiProvider 所以他包裹下面的widget都能获取到状态了
        //使用Consumer 获取状态
        Consumer<UserState>(
          builder: (context, store, child) {
            return Text("昵称是：" + store.nickname);
          },
        )
      ],
    );
  }
}
```

### 加载Html

[flutter_html](https://pub.dev/packages/flutter_html/install)  
Flutter默认为16, 所以需要修改app/build.gradle下的minSdkVersion为19

### get强大的路由状态缓存一体框架

[get](https://pub.dev/packages/get)  
路由一章的代码可修改为下面的

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:get/get.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //1.MaterialApp前面加Get
    return GetMaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        //名为"/"的路由作为应用的home(首页)
        initialRoute: "/",
        //路由配置变更
        getPages: Routers.routes
        //路由数组
        );
  }
}

//2.抽离路由代码
class Routers {
  //没有传参的路由全部放这里
  static final routes = [
    GetPage(
        name: '/',
        page: () => Scaffold(
              appBar: AppBar(
                title: const Text('状态栏标题'),
              ),
              body: const HomePage(),
            ),
        transition: Transition.rightToLeft),
    GetPage(
        name: '/new_page',
        page: () => const NewPage(),
        transition: Transition.rightToLeft),
  ];
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Column(children: [
      //3.点击按钮跳转到新路由
      ElevatedButton(
        child: const Text('命名路由跳转'),
        onPressed: () {
          _routeToName();
        },
      ),
      //7.路由返回时带参数
      ElevatedButton(
        child: const Text('跳转点参数返回后接收'),
        onPressed: () {
          _routeToBack();
        },
      ),
    ]);
  }

  //4.命名路由跳转并传值
  void _routeToName() {
    Get.toNamed('/new_page', arguments: '标题传值');
  }

  //8.跳转点参数返回后接收
  void _routeToBack() async {
    var data = await Get.toNamed('/new_page', arguments: '标题传值');
    // ignore: avoid_print
    print(data);
  }
}

//新页面组件
class NewPage extends StatelessWidget {
  const NewPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //5.获取路由参数 简单吧
    final args = Get.arguments;
    // ignore: avoid_print
    // print(args);

    return Scaffold(
      appBar: AppBar(
        title: Text(args.toString()),
      ),
      //浮动按钮返回 如果没有AppBar，可以加这个
      floatingActionButton: FloatingActionButton(
        child: const Text('返回并携带参数'),
        onPressed: () {
          //9.返回上一页并带参数
          Get.back(result: '我是新页面返回的参数');
        },
      ),
      body: Column(children: [
        ElevatedButton(
          child: const Text('返回主页'),
          onPressed: () {
            //6.返回主页(跳转并删除栈内所有路由)
            Get.offAllNamed("/");
          },
        ),
      ]),
    );
  }
}
```

局部的状态管理（也不是很好用）

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //1.GetMaterialApp
    return GetMaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

//2.定义状态类
class Store {
  //比如你在这里记录用户昵称状态
  final nickname = ''.obs;
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    //3.实例化store 一定要用Get.put包裹 使用Get.put()实例化你的类，使其对当下的所有子路由可用
    final Store store = Get.put(Store());
    return Column(
      children: [
        ElevatedButton(
          onPressed: () {
            //4.设置状态
            store.nickname.value = '新昵称';
          },
          child: const Text("设置昵称"),
        ),
        //5.获取状态值 Obx(() => widget)
        Obx(() => Text(store.nickname.value))
      ],
    );
  }
}
```

全局的状态管理（推荐使用）

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

void main() {
  //3.依赖注入到内存 必须 注入后就可以在后面任何地方使用了，相当于初始化
  Get.put(Store());
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    //1.GetMaterialApp
    return GetMaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

//2.定义状态类
class Store extends GetxController {
  //比如你在这里记录用户昵称状态
  String nickname = '默认昵称';


  //5.改变用户昵称状态值 相当于vue里的 store action
  void changeNickName(String nicknameChangeValue) {
    nickname = nicknameChangeValue;
    update();
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

  @override
  void initState() {
    super.initState();
    //7.init获取或设置状态值 
    // ignore: avoid_print
    print(Get.find<Store>().nickname);
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        ElevatedButton(
          onPressed: () {
            //4.设置状态
            Get.find<Store>().changeNickName('新昵称');
          },
          child: const Text("设置昵称"),
        ),
        //6.获取并显示
        GetBuilder<Store>(
            //初始化store控制器
            init: Store(),
            builder: (store) {
              return Text(store.nickname);
            })
      ],
    );
  }
}
```

本地存储一章可以改为  
使用[get_storage](https://pub.flutter-io.cn/packages/get_storage)库,需要配置  

```ini
dependencies:
  get_storage: ^2.0.3
```

```dart
import 'package:flutter/material.dart';
import 'package:get_storage/get_storage.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //定义一个语言
  String lang = 'cn';
  //初始化本地缓存
  final box = GetStorage();

  //初始化状态 相当于mounted
  @override
  void initState() {
    super.initState();
    _getLocalStorage();
  }

  //获取本地缓存并设置当前语言
  void _getLocalStorage() async {
    if (mounted) {
      //查找当前收藏的本地存储
      if (box.read('lang') != null) {
        setState(() {
          lang = box.read('lang');
        });
      }
    }
  }

  //设置本地缓存
  void _setLocalStorage() async {
    //设置本地缓存
    setState(() {
      box.write('lang', lang == 'cn' ? 'en' : 'cn');
      lang = lang == 'cn' ? 'en' : 'cn';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        children: [
          Text(lang),
          ElevatedButton(
              onPressed: () {
                _setLocalStorage();
              },
              child: const Text("切换语言")),
          ElevatedButton(
              onPressed: () {
                _getLocalStorage();
              },
              child: const Text("获取语言"))
        ],
      ),
    );
  }
}
```

### 上拉加载下拉刷新

[pull_to_refresh](https://pub.dev/packages/pull_to_refresh)

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:pull_to_refresh/pull_to_refresh.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  //声明一个列表
  List list = [];

  //定义刷新控件
  final RefreshController _refreshController =
      RefreshController(initialRefresh: false);

  @override
  void initState() {
    super.initState();
    //初始化模拟加载完成
    setState(() {
      _onRefresh();
    });
  }

  //下拉刷新方法
  void _onRefresh() async {
    //模拟请求完成
    await Future.delayed(const Duration(milliseconds: 1000));

    if (mounted) {
      setState(() {
        list.clear(); //可省略
        list = [
          {'id': 1, "name": '张三'},
          {'id': 2, "name": '李四'},
          {'id': 3, "name": '王五'},
          {'id': 4, "name": '小六'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
          {'id': 5, "name": '老七'},
        ];
      });
    }

    //下拉刷新完成
    _refreshController.refreshCompleted();
  }

  //下拉加载方法
  void _onLoading() async {
    //模拟请求完成
    await Future.delayed(const Duration(milliseconds: 1000));
    if (mounted) {
      setState(() {
        list.add({'id': 5, "name": '老牛1'});
        list.add({'id': 5, "name": '老牛2'});
        list.add({'id': 5, "name": '老牛3'});
        list.add({'id': 5, "name": '老牛4'});
        list.add({'id': 5, "name": '老牛5'});
        //下拉加载完成
      });
    }
    _refreshController.loadComplete();
  }

  @override
  Widget build(BuildContext context) {
    //刷新组件
    return SmartRefresher(
        //下拉刷新
        enablePullDown: true,
        //上拉加载
        enablePullUp: true,
        //经典header 其他[ClassicHeader],[WaterDropMaterialHeader],[MaterialClassicHeader],[WaterDropHeader],[BezierCircleHeader]
        header: const ClassicHeader(
          releaseText: "松开刷新",
          refreshingText: '刷新中...',
          completeText: '刷新完成',
          idleText: '下拉刷新',
        ),
        footer: const ClassicFooter(
          canLoadingText: '松开加载',
          loadingText: '加载中...',
          idleText: '上拉加载',
        ),
        controller: _refreshController,
        onRefresh: _onRefresh,
        onLoading: _onLoading,
        child: ListView(
            children: list
                .map((item) => ListTile(title: Text(item['name'])))
                .toList()));
  }
}
```

### 轮播图

[carousel_slider](https://pub.flutter-io.cn/packages/carousel_slider)

### animations内容动画效果组件

[animations](https://pub.flutter-io.cn/packages/animations)
点击容器 将容器放大并过渡到新页面

```dart
class HomePage extends StatelessWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return OpenContainer<bool>(
      //动画持续时间
      transitionDuration: const Duration(milliseconds: 2000),
      //动画类型
      transitionType: ContainerTransitionType.fadeThrough,
      //点击跳转的container按钮
      closedBuilder: (BuildContext _, VoidCallback openContainer) {
        return GestureDetector(
          child: Container(
              width: 100,
              height: 200,
              color: Colors.blue),
          onTap: () {
            openContainer();
          },
        );
      },
      //跳转后的页面
      openBuilder: (BuildContext context, VoidCallback _) {
        return const Text('详情页');
      },
      //关闭详情页触发
      onClosed: (bool? bool) {},
      tappable: true,
    );
  }
}
```

### 定位插件

[geolocator](https://pub.flutter-io.cn/packages/geolocator)  

```ini
dependencies:
  geolocator: '7.6.2'
  # 安卓Sdk30及以下用这个 安卓SDK31及以上（安卓12） 用最新版
```

AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.demo">
    <!-- 添加下面2个权限 -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
</manifest>
```

使用

```dart
import 'package:geolocator/geolocator.dart';
class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

  @override
  void initState() {
    super.initState();
    //获取位置
    _determinePosition().then((Position) => print(Position));
  }

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

Future _determinePosition() async {
  bool serviceEnabled;
  LocationPermission permission;

  //查看定位是否开启
  serviceEnabled = await Geolocator.isLocationServiceEnabled();
  if (!serviceEnabled) {
    return Future.error('Location services are disabled.');
  }

  //检查定位权限是否授予
  permission = await Geolocator.checkPermission();
  if (permission == LocationPermission.denied) {
    permission = await Geolocator.requestPermission();
    if (permission == LocationPermission.denied) {
      //定位权限拒绝访问
      return Future.error('Location permissions are denied');
    }
  }

  //定位权限永久拒绝
  if (permission == LocationPermission.deniedForever) {
    return Future.error(
        'Location permissions are permanently denied, we cannot request permissions.');
  }

  //获取到定位权限
  return await Geolocator.getCurrentPosition(
      desiredAccuracy: LocationAccuracy.best);
}
```

这个插件只能获取经纬度，要获取详细地址还得用[百度地图](https://pub.flutter-io.cn/packages/flutter_bmflocation/versions),目前他的最新版还有点问题[官方文档](https://lbsyun.baidu.com/index.php?title=flutter/loc/guide/create)  

```ini
dependencies:
  flutter_bmflocation: ^2.0.0-nullsafety.1
```

```dart
// ignore_for_file: avoid_print

import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter_bmflocation/bdmap_location_flutter_plugin.dart';
import 'package:flutter_bmflocation/flutter_baidu_location.dart';
import 'package:flutter_bmflocation/flutter_baidu_location_android_option.dart';
import 'package:flutter_bmflocation/flutter_baidu_location_ios_option.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'app',
        theme: ThemeData(primarySwatch: Colors.red),
        debugShowCheckedModeBanner: false,
        home: Scaffold(
          appBar: AppBar(
            title: const Text('状态栏标题'),
          ),
          body: const HomePage(),
        ));
  }
}

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  // 定位结果1Map形式
  Map<String, Object>? _loationResult;
  // 定位结果2对象形式
  BaiduLocation? _baiduLocation;
  //监听器
  StreamSubscription<Map<String, Object>?>? _locationListener;
  //定位插件
  late LocationFlutterPlugin _locationPlugin;

  @override
  void initState() {
    super.initState();
    //初始化插件
    _locationPlugin = LocationFlutterPlugin();

    /// 动态申请定位权限
    _locationPlugin.requestPermission();

    _locationListener = _locationPlugin.onResultCallback().listen((result) {
      setState(() {
        //PS这里面的异常处理捕捉不到，不影响使用
        _loationResult = result;
        print(_loationResult);
        // 将原生端返回的定位结果信息存储在定位结果类中
        _baiduLocation = BaiduLocation.fromMap(result);
        //输出定位信息
        if (_baiduLocation?.country != null) {
          print(_baiduLocation?.latitude);
          print(_baiduLocation?.longitude);
          print(_baiduLocation?.country);
          print(_baiduLocation?.province);
          print(_baiduLocation?.city);
          print(_baiduLocation?.district);
          print(_baiduLocation?.street);
          print(_baiduLocation?.address);
          print(_baiduLocation?.locationDetail);
          print(_baiduLocation?.poiList);
          //停止定位
          _locationListener?.cancel(); // 停止定位
        }
      });
    });
  }

  @override
  void dispose() {
    super.dispose();
    if (null != _locationListener) {
      _locationListener?.cancel(); // 停止定位
    }
  }

  /// 设置android端和ios端定位参数
  void _setLocOption() {
    /// android 端设置定位参数
    BaiduLocationAndroidOption androidOption = BaiduLocationAndroidOption();
    androidOption.setCoorType("bd09ll"); // 设置返回的位置坐标系类型
    androidOption.setIsNeedAltitude(true); // 设置是否需要返回海拔高度信息
    androidOption.setIsNeedAddres(true); // 设置是否需要返回地址信息
    androidOption.setIsNeedLocationPoiList(true); // 设置是否需要返回周边poi信息
    androidOption.setIsNeedNewVersionRgc(true); // 设置是否需要返回最新版本rgc信息
    androidOption.setIsNeedLocationDescribe(true); // 设置是否需要返回位置描述
    androidOption.setOpenGps(true); // 设置是否需要使用gps
    androidOption.setLocationMode(LocationMode.Hight_Accuracy); // 设置定位模式
    androidOption.setScanspan(3000); // 设置发起定位请求时间间隔1S
    Map androidMap = androidOption.getMap();

    /// ios 端设置定位参数
    BaiduLocationIOSOption iosOption = BaiduLocationIOSOption();
    iosOption.setIsNeedNewVersionRgc(true); // 设置是否需要返回最新版本rgc信息
    iosOption.setBMKLocationCoordinateType(
        "BMKLocationCoordinateTypeBMK09LL"); // 设置返回的位置坐标系类型
    iosOption.setActivityType("CLActivityTypeAutomotiveNavigation"); // 设置应用位置类型
    iosOption.setLocationTimeout(10); // 设置位置获取超时时间
    iosOption.setDesiredAccuracy("kCLLocationAccuracyBest"); // 设置预期精度参数
    iosOption.setReGeocodeTimeout(10); // 设置获取地址信息超时时间
    iosOption.setDistanceFilter(100); // 设置定位最小更新距离
    iosOption.setAllowsBackgroundLocationUpdates(true); // 是否允许后台定位
    iosOption.setPauseLocUpdateAutomatically(true); //  定位是否会被系统自动暂停
    Map iosMap = iosOption.getMap();

    _locationPlugin.prepareLoc(androidMap, iosMap);
  }

  /// 启动定位
  void _startLocation() {
    _setLocOption();
    _locationPlugin.startLocation();
  }

  /// 停止定位
  void _stopLocation() {
    _locationPlugin.stopLocation();
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      //按钮主动获取定位信息 实际可以弄一个弹窗 让他点访问 或者配置持续定位
      children: [
        ElevatedButton(
            onPressed: () {
              _startLocation();
            },
            child: const Text("开启定位")),
        ElevatedButton(
            onPressed: () {
              _stopLocation();
            },
            child: const Text("停止定位"))
      ],
    );
  }
}
```

### 动画

#### Tween补间动画

```dart
class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

//with SingleTickerProviderStateMixin 单个动画必须继承
class _HomePageState extends State<HomePage>
    with SingleTickerProviderStateMixin {
  //定义动画控制器
  late AnimationController _animationController;
  //定义动画对象
  late Animation<Offset> _animation;
  //定义动画位移控制变量
  Offset _position = const Offset(0.0, 0.0);
  //动画是否开始  开始则控制变量用动画值代替 否则用控制变量代替
  bool _isAnimationStart = false;

  @override
  void initState() {
    super.initState();
    _initAnimation();
  }

  @override
  void dispose() {
    //路由销毁时需要释放动画资源
    _animationController.dispose();
    super.dispose();
  }

  //初始化动画方法
  void _initAnimation() {
    //初始化动画控制器
    _animationController = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    //初始化动画对象 不然会报错动画对象不存
    _animation =
        Tween(begin: const Offset(0.0, 0.0), end: const Offset(0.0, 0.0))
            .animate(_animationController);
    //监听动画开始和结束
    _animationController.addStatusListener((AnimationStatus status) {
      if (status == AnimationStatus.forward) {
        setState(() {
          //动画开始播放啦
          _isAnimationStart = true;
        });
      }
      if (status == AnimationStatus.completed) {
        setState(() {
          //动画完成
          _isAnimationStart = false;
          //归位
          _position = const Offset(0.0, 0.0);
        });
      }
    });
  }

  // 执行动画
  // @offset begin 开始的位置
  // @offset end 结束的位置
  void _runAnimation(Offset begin, Offset end) {
    //初始化过度曲线
    var curve =
        CurvedAnimation(parent: _animationController, curve: Curves.easeInOut);
    //初始化动画对象 .animate()如果没有曲线curve 可以直接传_animationController
    _animation = Tween(begin: begin, end: end).animate(curve);
    //启动动画(..代表使用前面的返回值调用函数 reset重置动画 forward正向执行)
    _animationController
      ..reset()
      ..forward();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
        color: Colors.blue,
        child: AnimatedBuilder(
            animation: _animation,
            builder: (context, child) {
              return SizedBox(
                child: Transform.translate(
                  offset: _isAnimationStart ? _animation.value : _position,
                  child: GestureDetector(
                    child: const SizedBox(
                      child: Center(
                        child: Text(
                            '文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容文字内容'),
                      ),
                    ),
                    onPanUpdate: (details) {
                      //实时设置偏移量
                      setState(() {
                        _position += Offset(details.delta.dx, 0.0);
                      });
                    },
                    onPanEnd: (details) {
                      //设置偏移量状态
                      setState(() {
                        if (_position.dx.abs() <
                            MediaQuery.of(context).size.width / 6 * 1) {
                          _position = const Offset(0.0, 0.0);
                        } else {
                          //判断方向
                          if (_position.dx < 0) {
                            //向左拖动 执行动画
                            _runAnimation(
                                _position,
                                Offset(
                                    -MediaQuery.of(context).size.width, 0.0));
                          } else {
                            //向右拖动 执行动画
                            _runAnimation(_position,
                                Offset(MediaQuery.of(context).size.width, 0.0));
                          }
                        }
                      });
                    },
                  ),
                ),
              );
            }));
  }
}
```

### 打包安装

#### 添加启动图标

[插件](https://pub.flutter-io.cn/packages/flutter_launcher_icons)  

```ini
dev_dependencies:
  flutter_launcher_icons: "^0.9.2"

flutter_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icon/icon.png"
```

然后在项目根目录新建assets/icon/icon.png  

运行创建图标  

```powershell
flutter pub run flutter_launcher_icons:main 
```

#### 添加闪屏页

[flutter_native_splash](https://pub.flutter-io.cn/packages/flutter_native_splash)

```ini
dev_dependencies:
  flutter_native_splash: ^1.2.4

flutter_native_splash:
  #下面2个选一个
  color: "#42a5f5"
  #background_image: "assets/images/splashscreen.png"
```

然后在项目根目录新建assets/images/splashscreen.png
运行创建闪屏页 

```powershell
flutter pub run flutter_native_splash:create
```

#### 添加一下需要的权限

这里添加基本的网络权限

```
<manifest>
  <uses-permission
      android:name="android.permission.INTERNET"/>
</manifest
```

#### 创建签名文件

keytool工具会在安装android stido一起安装的  
创建命令如下

```powershell
#创建
keytool -genkey -v -keystore demo.keystore -alias demo -keyalg RSA -keysize 2048 -validity 10000
#格式转换
keytool -importkeystore -srckeystore demo.keystore -destkeystore demo.keystore -deststoretype pkcs12
```

#### 给app配置自动签名

创建文件/android/key.properties

```ini
storePassword=上一步骤中的密码
keyPassword=上一步骤中的密码
keyAlias=demo别名
storeFile=密钥库路径 需要\\双斜杠
```

修改/android/app/build.gradle 将 key.properties 文件加载到 keystoreProperties 对象中

```java
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}
//在这新增前面的代码

android {
  ...
}
```

继续修改/android/app/build.gradle

```java
signingConfigs {
  release {
      keyAlias keystoreProperties['keyAlias']
      keyPassword keystoreProperties['keyPassword']
      storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
      storePassword keystoreProperties['storePassword']
  }
}

//在这新增前面的代码 下面的signingConfig signingConfigs.release 也要替换记得
buildTypes {
    release {
        signingConfig signingConfigs.release
    }
}
```

!> 配置完后就能自动签名了，当你更改 gradle 文件后，也许需要运行一下 flutter clean。这将防止缓存的版本影响签名过程 运行完后保存下yaml配置文件重新pub get一下

#### 打包

生成release版本，生成过程中会自动签名

```powershell
flutter build apk
or
flutter build apk --split-per-abi #这种会生成三个
```

生成文件目录\build\app\outputs\apk\release\xxx.apk

#### 查看是否签名

```powershell
keytool -list -printcert -jarfile .\app-release.apk
```

### App升级方案

#### 检查版本号

安装package_info

```ini
dev_dependencies:
  package_info: ^2.0.2
```

#### 获取文件存储路径

安装path_provider

```ini
dev_dependencies: 
  path_provider: ^2.0.5
```

#### 检查读写权限

安装 permission_handler

```ini
dev_dependencies: 
  permission_handler: ^8.2.5
```

android\app\src\main\AndroidManifest.xml 添加读写和请求安装的权限

```java
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES" />
```

!> 最新版的需要设置android\app\build.gradle sdk31 不然会报错

#### 服务器上传app.json和安装包

app.json

```json
{
  "version": "1.0.2",
  "url":"http://noval.o8o8o8.com/version/app-release.apk"
}
```

#### 获取app.json信息

安装dio

```ini
dev_dependencies: 
  dio: ^4.0.0 
```

#### 打开安装文件

安装open_file

```ini
dependencies:
  open_file: ^3.2.1
```

#### 完整代码

```dart
import 'dart:io';
import 'package:package_info/package_info.dart';
import 'package:path_provider/path_provider.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:open_file/open_file.dart';
import './request.dart';

class AppUpdate {
  late String _version; //版本号
  late String savePath; //文件存储路径
  late String downloadUrl; //下载路径
  bool _permision = false; //是否有存储权限
  bool canUpdate = false; //是否可以更新


  init() async {
    //获取版本号
    await _getVersion();
    //获取文件存储路径
    await _findLocalPath();
    //检查权限
    await _checkPermission();
  }

  //获取版本号 pubspec.yaml version: 1.0.0+1  版本号+构件号 一般改前面的就行
  _getVersion() async {
    PackageInfo packageInfo = await PackageInfo.fromPlatform();
    _version = packageInfo.version;
  }

  //获取文件存储路径
  Future _findLocalPath() async {
    var directory = Platform.isAndroid
        ? await getExternalStorageDirectory()
        : await getApplicationDocumentsDirectory();
    savePath = directory!.path + "/app-release.apk";
  }

  //检测存储权限
  _checkPermission() async {
    if (Platform.isAndroid) {
      //获取状态
      var status = await Permission.storage.status;
      //如果没有授权，请求授权
      if (!status.isGranted) {
        if (await Permission.storage.request().isGranted) {
          //授权了
          _permision = true;
        } else {
          //拒绝授权
          _permision = false;
        }
      } else {
        //已经授权了
        _permision = true;
      }
    } else {
      //ios
      _permision = true;
    }
  }

  checkUpdate() async {
    //有权限
    if (_permision) {
      //获取版本信息 这里的http dio章节封装好了自己看
      await Request.http(url: '/version/app.json', type: 'get', data: {})
          .then((res) {
        //比对版本号 若有新版本则下载
        downloadUrl = res.data['url'];
        if (res.data["version"] != _version) {
          //有新版本需要更新
          canUpdate = true;
        }
      }).catchError((error) {});
    }
  }

  //安装Apk
  installApk() async {
    await OpenFile.open(savePath);
  }
}
```

调用

```dart
class HomePage extends StatefulWidget{
  const HomePage({Key? key}) : super(key: key);

  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> with WidgetsBindingObserver{
  List list = [];
  late AppUpdate appUpdate;

  //当前下载进度
  double downloadPercent = 0;

  //mounted
  @override
  void initState() {
    super.initState();
    //这句是监听绑定关键
    WidgetsBinding.instance!.addObserver(this);
    _updateApp();
  }

  //监听APP切换状态 必须with WidgetsBindingObserver 类似继承 但是不会覆盖原有的变量方法 可以with多个类
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    super.didChangeAppLifecycleState(state);
    //从后台隐藏状态切换到前台显示状态
    if (state == AppLifecycleState.resumed) {
      //如果进度下载进度为1 则安装APP
      if (downloadPercent >= 1) {
        appUpdate.installApk();
      }
    }
  }

  //检查app更新
  void _updateApp() async {
    //检查App升级
    appUpdate = AppUpdate();
    await appUpdate.init();
    await appUpdate.checkUpdate();
    if (appUpdate.canUpdate) {
      //如果可以更新 弹窗选择是否更新
      showDialog(
        context: context,
        barrierDismissible: true,
        builder: (BuildContext context) {
          return AlertDialog(
            title: const Text('检测到新版本'),
            content: const Text('是否立即更新'),
            actions: <Widget>[
              TextButton(
                child: const Text('下次在说'),
                onPressed: () {
                  Navigator.of(context).pop();
                },
              ),
              TextButton(
                child: const Text('立即更新'),
                onPressed: () async {
                  Navigator.of(context).pop();
                  //显示下载进度 这里用到了局部刷新StatefulBuilder
                  late StateSetter dialogState;
                  showDialog(
                      context: context,
                      barrierDismissible: false,
                      builder: (BuildContext context) {
                        return StatefulBuilder(builder: (context, state) {
                          dialogState = state;
                          return AlertDialog(
                            title: const Text('更新提示'),
                            content: Text("正在更新 当前进度" +
                                (downloadPercent * 100).toStringAsFixed(2) +
                                '%'),
                          );
                        });
                      });

                  //创建下载任务
                  Dio dio = Dio();
                  await dio.download(appUpdate.downloadUrl, appUpdate.savePath,
                      onReceiveProgress: (received, total) {
                    if (total != -1) {
                      //当前下载的百分比例
                      double percentValue =
                          double.parse((received / total).toStringAsFixed(2));
                      dialogState(() {
                        downloadPercent = percentValue;
                      });
                    }
                  });
                  await appUpdate.installApk();
                },
              ),
            ],
          );
        },
      );
    }
  }
}
```
