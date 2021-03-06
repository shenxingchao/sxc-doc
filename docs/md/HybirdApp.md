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

| 系统变量名称 |                                                  值 |
| :----------- | --------------------------------------------------: |
| JAVA_HOME    |                                    D:\tool\jdk1.8.0 |
| Path         |                                     %JAVA_HOME%\bin |
| Path         |                                 %JAVA_HOME%\jre\bin |
| CLASSPATH    | .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar; |

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
-  SDK Manager - SDK Platforms - Show Package Details 
   -  Android SDK Platform 29
-  SDK Manager - SDK Tools - Show Package Details
   -  Android SDK Build-Tools
      -  29.0.2版本
   -  NDK (Side by side) - Show Package Details
      -  20.1.5948944版本

?> 安装sdk和编译工具，模拟器不需要安装，用第三方

环境变量配置

| 系统变量名称 |                            值 |
| :----------- | ----------------------------: |
| ANDROID_HOME |            D:\tool\androidsdk |
| Path         | %ANDROID_HOME%\platform-tools |
| Path         |          %ANDROID_HOME%\tools |
| Path         |      %ANDROID_HOME%\tools\bin |

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
    titleStyle: { //这个titleStyle的类型是Text style (object)	 继承自rn组件的Text的style
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
