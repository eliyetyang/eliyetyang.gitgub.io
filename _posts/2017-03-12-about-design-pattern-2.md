---
title: "关于设计模式的基本原则（二）"
categorise:
  - architecture
tab:
  - design
  
---

### 三 里氏替换原则
<p>
这个原则比较好理解，一句话就是————子类可以完全替换父类。
</p>

<p>
一般的情况来说，子类继承自父类，那么自然有父类的所有功能，这也是java多态的体现。
但很多时候，由于各种原因，我们可能会重写父类的部分方法，或是对父类的部分方法（多为抽象）进行空实现，
而当代码逐渐增多，结构逐渐复杂的时候，这种实现方式，很有可能引发难以察觉的问题。
所以我们在继承的时候，一定要充分思考，当前的类是否能够完全替代使用其父类的位置，
我的重写或实现是否符合当初对该方法的定义，以此尽量减少可能发生的BUG。
</p>
<p>
例子还是洗衣机……
</p>
<p>
据说在南方部分地区，雨季空气湿度高，衣服很难晾干，而我们的洗衣机类就没办法满足他们的需求。
所以想要加上一个烘干功能～其他的功能都和原来的一样就好，那么我们继承了洗衣机，定义了一个烘干洗衣机类。
其他的功能都和原有的洗衣机一样，只有我们在甩干后要添加烘干这一操作，那门我们就重写下甩干方法吧，先super再添加烘干红能。
等等，好像有什么不对的地方，我以后那些在大北方用洗衣机怎么办，都是洗衣机阿（使用时声明相同，皆为其父类），
这些地方干燥缺水，烘干功能又异常耗电（耗费系统资源），一旦抓来的不知道是那个具体的洗衣机就糟了，
况且这是甩干方法吧，把烘干这功能添加进去不合理阿。还是这样把，原来的方法我们都使用父类的，然后单独声明一个烘干方法，
需要烘干的，就单独调用，这样在北方使用烘干洗衣机也没有什么太大的问题了，老铁没毛病，双击给自己送个666～。
</p>

```java
package com.eliyet.yang.code.blog.blogsimplecode;

import android.util.Log;

import static com.eliyet.yang.code.blog.blogsimplecode.Constant.LOG_TAG_ACTION;

/**
 * Created by eliyetyang on 17-3-14.
 */

public class DryWasher extends DefaultWasher{

    public void drying() {
        Log.i(LOG_TAG_ACTION, "单独的烘干功能。");
    }
}

```
### 四 依赖倒置原则

<p>
之前早些看资料时不是很理解这个原则，但他另一个名字就很好理解————好莱坞原则，怎么个说法呢？“不要找我们，我们会联系你！”
</p>
<p>
我是好莱坞大导演，一个演员来跟我讲他又多么擅长演绎某种角色，让我给他拍一部电影。what?我是导演他时导演？
我要拍什么电影我说的算，需要你演的时候我自然去联系你，你来定义电影算怎么会事。这就是好莱坞原则。
</p>
<p>
概念一点的说法就是————“底层应依赖于上层定义，不应该上层依赖于底层功能。”
这也是软件的结构都是从上层来设计的原因，在上层时我要定义我能做什么事情，而做这事情的时候又要用到什么功能。
底层应该根据我的需要，实现我需要的功能，随着业务的拓展，可能这种实现也会是多种的。
这样会使得软件的各个层次结构间是可拔插的，做到低耦合高复用软件结构健壮。
</p>
<p>
关于依赖倒置原则，我再说点设计外的感悟。为什么说是倒置，就好像公司年会，需要表演节目，
节目单都是根据员工们的才艺所制定的。这是一般的依赖关系，毕竟让同事们现学是不现实的。
而春晚就像是应用了依赖倒置原则，我的晚会每一步需要什么类型的节目，你们就来争取这些位置吧，这是春晚定接口，
其他人进行实现。虽然春晚这个比喻不是太妥当，但从这里也能看出成果的差别。
一定要先明白你需要什么，然后根据你需要的去争取，这才能取得更高的成果。
好比晚上做菜，不要家里有什么就考虑做什么，这样你的视野会被局限，你能作出的菜品也可想而知。
想要做的更好？那么没有的材料你去买，没有卖的你去自己种！不要消极的等待，要主动去争取。
相信最后的结果一定会不同。
</p>
<p>
稍微扯远了，回到依赖倒置原则，该原则有个很常用方法————依赖注入。
想想你过去用过的依赖注入，需要的地方直接声明，使用的时候就有这个实例了。这也是一样的，
在使用前，注入框架会根据你的生命，去寻找或创建该类的实例注入到你需要使用的地方。
</p>
<p>
好，我们继续洗衣机。
</p>
<p>
洗衣机呀洗衣机，你为什么这么沉，总这么搬动也不是长久之计，这暴脾气忍不了，搞一个外置架的接口，
这个外置架要有几个功能——固定；移动；高度调整。在洗衣机类中，加入外置架成员变量，然后添加一个带参的构造函数，
从这里指定外置架的示例。然后我们在增加一个外接设备项目，这里实现外置架接口，创建一个默认的矩形外置架。
这样我们的洗衣机又变得更加强大了～趁这个机会可以推出含有矩形外置架的A套餐，哇哈哈哈哈～～～
</p>

```java
package com.eliyet.yang.code.blog.blogsimplecode;

/**
 * Created by eliyetyang on 17-3-14.
 * 外置架接口
 */

public interface IExternalFrame {
    void lock();

    void move();

    void heightAdjust();
}
```

```java
package com.eliyet.yang.code.blog.blogsimplecode.extrenal;

import android.util.Log;

import com.eliyet.yang.code.blog.blogsimplecode.IExternalFrame;

import static com.eliyet.yang.code.blog.blogsimplecode.Constant.LOG_TAG_ACTION;

/**
 * Created by eliyetyang on 17-3-14.
 * 默认的外置架。
 */

public class DefaultFrame implements IExternalFrame {

    @Override
    public void lock() {
        Log.i(LOG_TAG_ACTION,"外置架固定！");
    }

    @Override
    public void move() {
        Log.i(LOG_TAG_ACTION, "外置架活动。");
    }

    @Override
    public void heightAdjust() {
        Log.i(LOG_TAG_ACTION, "外置架高度调整。");
    }
}
```

```java
package com.eliyet.yang.code.blog.blogsimplecode;

import android.util.Log;

import com.eliyet.yang.code.blog.blogsimplecode.extrenal.DefaultFrame;

import static com.eliyet.yang.code.blog.blogsimplecode.Constant.LOG_TAG_ACTION;

/**
 * Created by eliyetyang on 17-3-14.
 * 抽象的洗衣机基类
 * 添加默认的外置架
 */

public abstract class ABaseWasher {
    private Soak mSoak;
    private IExternalFrame mFrame;

    public ABaseWasher(Soak soak) {
        this.mSoak = soak;
        defaultInit();
    }

    public ABaseWasher() {
        mSoak = new Soak();
        defaultInit();
    }

    public void defaultInit() {
        mFrame = new DefaultFrame();
    }

    public void soak() {
        mSoak.soak();
    }

    public abstract void wash() ;

    public void spinDry() {
        Log.i(LOG_TAG_ACTION, "spin-drying");
    }

    public void move() {
        mFrame.move();
    }

    public void lock() {
        mFrame.lock();
    }

    public void heightAdjust() {
        mFrame.heightAdjust();
    }
}
```

