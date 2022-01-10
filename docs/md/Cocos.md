# Cocos
## 精灵和图集
1. 精灵就是单张图片
2. 图集（atlas）就是一组图片

## Particle 粒子系统使用说明
1. 点击切换 preview 属性，可以切换该粒子在场景中的显隐（不会影响运行时的显示）
2. PlayOnLoad 属性决定是否在游戏运行时自动播放该粒子
3. AutoRemoveOnFinish 属性决定该粒子是否播放完就会自动销毁
4. 点击 File 属性里引用的资源，可以看到在 Assets 面板里高亮了外部工具制作好并导入的粒子资源
5. 可以拖拽另一个粒子资源到 File 属性，预览另外粒子的效果

### 使用自定义属性制作粒子
1. 点击开启 Custom 属性，可以看到下面的粒子参数都被激活并可以修改了
2. 根据用户需要调整每一个参数，直到达成希望的效果
3. 也可以去http://www.effecthub.com/制作还能保存作品   

### 相关方法
```javascript
    let myParticle = this.particle;
    if (myParticle.particleCount > 0) { //判断粒子数量的
        myParticle.stopSystem(); // 关闭粒子系统
    } else {
        myParticle.resetSystem(); // 重置粒子系统
    }
```

## 动画
### 属性
打开动画编辑器左下角添加属性，然后可以对精灵的属性添加动画效果，相当于css对属性进行css3动画变换一个意思
### 模式
Loop 循环播放  
PingPong 循环并反向播放(来回移动)  

## 碰撞系统
### 添加碰撞盒
组件BoxCollider

### 设置碰撞分组
1. 项目设置分组管理添加分组
2. 为需要碰撞的精灵的分组属性添加分组  在属性检查器第一栏
3. 项目设置分组管理底部为需要碰撞检测的分组打上勾
   
### 开启碰撞系统
```javascript
//获取碰撞系统管理器
let manager = cc.director.getCollisionManager();
//开启
manager.enabled = true;
//开启调试碰撞
manager.enabledDebugDraw = true;
//显示碰撞检测的包围盒
manager.enabledDrawBoundingBox = true;
```

### 碰撞的物体挂上碰撞检测到的脚本
```javascript
/**
 * 当碰撞产生的时候调用 碰撞刚发生
 * @param  {Collider} other 产生碰撞的另一个碰撞组件
 * @param  {Collider} self  产生碰撞的自身的碰撞组件
 */
onCollisionEnter: function (other, self) {
    console.log('on collision enter');

    // 碰撞系统会计算出碰撞组件在世界坐标系下的相关的值，并放到 world 这个属性里面
    var world = self.world;

    // 碰撞组件的 aabb 碰撞框
    var aabb = world.aabb;

    // 节点碰撞前上一帧 aabb 碰撞框的位置
    var preAabb = world.preAabb;

    // 碰撞框的世界矩阵
    var t = world.transform;

    // 以下属性为圆形碰撞组件特有属性
    var r = world.radius;
    var p = world.position;

    // 以下属性为 矩形 和 多边形 碰撞组件特有属性
    var ps = world.points;
},

/**
 * 当碰撞产生后，碰撞结束前的情况下，每次计算碰撞结果后调用 一直在碰撞
 * @param  {Collider} other 产生碰撞的另一个碰撞组件
 * @param  {Collider} self  产生碰撞的自身的碰撞组件
 */
onCollisionStay: function (other, self) {
    console.log('on collision stay');
},

/**
 * 当碰撞结束后调用 上面的和这个基本没用吧
 * @param  {Collider} other 产生碰撞的另一个碰撞组件
 * @param  {Collider} self  产生碰撞的自身的碰撞组件
 */
onCollisionExit: function (other, self) {
    console.log('on collision exit');
}
```

### 判断一个点是否在多边形碰撞组件范围内（判断点击的位置是否在某个物体上）
这个多变形组件PolygonCollider的好处是任意形状点击genrate points可以自动生成多边形的范围
```javascript
cc.director.getCollisionManager().enabled = true;
cc.director.getCollisionManager().enabledDebugDraw = true;

this.node.on(cc.Node.EventType.TOUCH_START, function (touch, event) {
    var touchLoc = touch.getLocation();
    //pointInPolygon  翻译：点在多边形里面  this.collider 这个就是多边形的碰撞体
    if (cc.Intersection.pointInPolygon(touchLoc, this.collider.world.points)) {
        //在里面
    }
    else {
        //不在里面
    }
}, this);
```


## 物理系统
!> 添加一个物理组件->Collider->Box的时候相当于添加了一个刚体和一个物理碰撞体2个组件,如果需要物理属性，如重力加速度，速度等那就得用这个，如果不需要物理属性判断2个物体碰撞则用碰撞组件即可

### 开启物理系统
```javascript
//开启
cc.director.getPhysicsManager().enabled = true;
//设置重力加速度 下降640世界单位/秒
cc.director.getPhysicsManager().gravity = cc.v2(0, -640);
```

### 判断在屏幕上点击碰撞体
```javascript
//判断是否点击了碰撞体
this.node.on(
    "touchstart",
    function (e) {
    var collider = cc.director.getPhysicsManager().testPoint(e.getLocation());
    //如果点击碰撞体，此处会返回点击的碰撞体，如果点击了多个碰撞体，那么会随机返回一个
    console.log(collider)
    },
    this
);
```

### 设置刚体RigidBody(有质量的物体)属性
刚体的类型是 cc.RigidBody this.collider节点的的类型
```javascript
//开启碰撞监听 开启后碰撞才能触发回调函数
this.collider.enabledContactListener = true;
//设置刚体速度
this.collider.linearVelocity = cc.Vec2(600, 0);
//设置线性速度与空气的摩擦系数
this.collider.linearDamping = 2;
// 设置旋转速度
this.collider.angularVelocity = 600;
// 设置旋转与空气的摩擦系数
this.collider.angularDamping = velocity;
```

### 刚体类型
1.cc.RigidBodyType.Static

静态刚体，零质量，零速度，即不会受到重力或速度影响，但是可以设置他的位置来进行移动。

2.cc.RigidBodyType.Dynamic

动态刚体，有质量，可以设置速度，会受到重力影响。

3.cc.RigidBodyType.Kinematic

运动刚体，零质量，可以设置速度，不会受到重力的影响，但是可以设置速度来进行移动。

4.cc.RigidBodyType.Animated

动画刚体，在上面已经提到过，从 Kinematic 衍生的类型，主要用于刚体与动画编辑结合使用。

### 碰撞体的回调函数
直接挂在到碰撞体上
```javascript
cc.Class({
    extends: cc.Component,

    // 只在两个碰撞体开始接触时被调用一次
    onBeginContact: function (contact, selfCollider, otherCollider) {
    },

    // 只在两个碰撞体结束接触时被调用一次
    onEndContact: function (contact, selfCollider, otherCollider) {
    },

    // 每次将要处理碰撞体接触逻辑时被调用
    onPreSolve: function (contact, selfCollider, otherCollider) {
    },

    // 每次处理完碰撞体接触逻辑时被调用
    onPostSolve: function (contact, selfCollider, otherCollider) {
    }
});
```

### 相机
1.相机层级属性Depth 越大表示越迟渲染，ZINDEX在上面
2.cullingMask表示拍摄哪个分组

#### 主角跟随相机移动
核心代码  
camera.js
```javascript
cc.Class({
  extends: cc.Component,

  properties: {
    //玩家
    player: {
      default: null,
      type: cc.Node,
    },
    //地图
    map: {
      default: null,
      type: cc.Node,
    },
  },

  onLoad() {
    let _this = this;
    //计算摄像机边界 地图左下角在世界坐标的左下角
    _this._camera_min_x = cc.view.getVisibleSize().width / 2;
    _this._camera_max_x = _this.map.width - cc.view.getVisibleSize().width / 2;
    _this._camera_min_y = cc.view.getVisibleSize().height / 2;
    _this._camera_max_y = _this.map.height - cc.view.getVisibleSize().height / 2;
  },

  start() {},

  update(dt) {
    let _this = this;
    //转成世界坐标去判断
    let w_p = _this.player.convertToWorldSpaceAR(cc.v2(0, 0));

    //下面这个判断不要就是会看到地图外的世界
    if (w_p.x > _this._camera_max_x) {
      w_p.x = _this._camera_max_x;
    }
    if (w_p.x < _this._camera_min_x) {
      w_p.x = _this._camera_min_x;
    }
    if (w_p.y > _this._camera_max_y) {
      w_p.y = _this._camera_max_y;
    }
    if (w_p.y < _this._camera_min_y) {
      w_p.y = _this._camera_min_y;
    }

    let c_p = _this.node.parent.convertToNodeSpaceAR(w_p);
    _this.node.setPosition(c_p);
  },
});
```
player.js
```javascript
onLoad() {
    let _this = this;
    //this.bg是点击的范围
    this.bg.on(
        "touchstart",
        function (e) {
        let pos = _this.camera
            .getComponent(cc.Camera)
            .getScreenToWorldPoint(e.getLocation(), cc.v2(0, 0));
        //console.log(pos); //getScreenToWorldPoint可用替换getCameraToWorldPoint这个也一样
        _this.node.setPosition(_this.node.parent.convertToNodeSpaceAR(pos));
        },
        this
    );
},
```
!> getScreenToWorldPoint这个很关键，很多教程里面都没写，屏幕坐标组转世界坐标