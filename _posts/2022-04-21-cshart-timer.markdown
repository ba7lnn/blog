---
layout: post
title:  "C# Timer 定时器"
date:   2022-04-21 12:27:32 +0800
categories: USDX
---

应用场景


公司旧项目需修改原有的数据备份功能，原功能为在实时数据入库后进行数据备份，备份方法为将实时数据转化为二进制数组后按照当天日期进行入库，之后每次有实时数据入库都需要将历史数据表中的二进制数据字段查询后在尾部添加新二进制数据，再更新入库。然而这个系统为数据监控系统，每秒都会有将近百条记录入库，所以上述原功能会导致运行迟缓，消耗资源。
现改为每日零时进行一次性的数据备份。


区别：

1、System.Threading.Timer、System.Windows.Forms.Timer和System.Timers.Timer区别

相关文档：

* <https://docs.microsoft.com/zh-tw/dotnet/api/system.windows.forms.timer?view=netcore-3.1>
* <https://docs.microsoft.com/zh-tw/dotnet/api/system.timers.timer?view=netcore-3.1>
* <https://docs.microsoft.com/zh-tw/dotnet/api/system.threading.timer?view=netcore-3.1>

System.Windows.Forms.Timer是基于UI的

System.Timers.Timer是基于服务

System.Threading.Timer是基于线程

System.Timers.Timer和System.Threading.Timer是多线程的，时间到了，就会执行，之前的任务没有执行完成也不影响，因为还会开个新线程继续执行新的任务。

System.Windows.Forms.Timer是单线程的，只有之前的任务执行完成了，才会执行下次任务，这样上一次任务处理超过时间，下一次任务执行就会延时执行。

2、System.Threading.Timer使用示例代码

Timer构造函数参数说明：

**Callback：**一个 TimerCallback 委托，表示要执行的方法。

**State：**一个包含回调方法要使用的信息的对象，或者为空引用。

**dueTime：**调用 callback 之前延迟的时间量（以毫秒为单位）。指定 Timeout.Infinite 以防止计时器开始计时。指定零 (0) 以立即启动计时器。

**Period：**调用 callback 的时间间隔（以毫秒为单位）。指定 Timeout.Infinite 可以禁用定期终止。

1）创建System.Threading.Timer实例

```
//Timeout.Infinite为-1，创建实例后，不执行计时器，如为0则立即执行
System.Threading.Timer threadTimer = new System.Threading.Timer(callback: new TimerCallback(TimerUp), null, Timeout.Infinite, 1000);
```

2）启动System.Threading.Timer实例

```
//1秒后执行一次，然后第5秒一次
threadTimer.Change(1000, 5000);
```

3）停止System.Threading.Timer实例

```
threadTimer.Change(-1, -1);
threadTimer.Dispose();
```

定时任务可以有两种方式实现：

第一种是使用定时间隔为一秒的计时器System.Timers.Timer，一直循环判断当前时间是否为期望执行时间；
第二种是使用计时器System.Threading.Timer，当前时间为期望执行时间时触发定时任务。
一、System.Timers.Timer

定义一个System.Timers.Timer对象，然后绑定Elapsed事件，通过Start()方法来启动计时，通过Stop()方法或者Enable=false停止计时。AutoReset属性设置是否重复计时（设置为false只执行一次，设置为true可以多次执行）。Elapsed事件绑定相当于另开了一个线程，也就是说在Elapsed绑定的事件里不能访问其它线程里的控件（需要定义委托，通过Invoke调用委托访问其它线程里面的控件）。

**实例：**

```
//定义Timer类
System.Timers.Timer timer;
private void FormMain_Load(object sender, EventArgs e)
{
     InitTimer();
}
/// <summary>
/// 初始化Timer控件
/// </summary>
private void InitTimer()
{
     //设置定时间隔(毫秒为单位)
     int interval = 1000;
     timer = new System.Timers.Timer(interval);
     //设置执行一次（false）还是一直执行(true)
     timer.AutoReset = true;
     //设置是否执行System.Timers.Timer.Elapsed事件
     timer.Enabled = true;
     //绑定Elapsed事件
     timer.Elapsed += new System.Timers.ElapsedEventHandler(Timer_Elapsed);
}

private void Timer_Elapsed(object sender, System.Timers.ElapsedEventArgs e)
{
    // 得到 hour minute second  如果等于某个值就开始执行
    int intHour = e.SignalTime.Hour;
    int intMinute = e.SignalTime.Minute;
    int intSecond = e.SignalTime.Second;
    // 定制时间,在00：00：00 的时候执行
    int iHour = 00;
    int iMinute = 00;
    int iSecond = 00;
    // 设置 每天的00：00：00开始执行程序
    if (intHour == iHour && intMinute == iMinute && intSecond == iSecond)
    {
       //调用数据备份方法
    }
}
```

二、System.Threading.Timer

定义该类时，通过构造函数进行初始化：

写法：

var timer = new System.Threading.Timer(new TimerCallback(A), B, C, D);
其中：

A表示要执行的方法,可以带参数也可以不带参数
B表示要给这A方法传递的参数，如果A方法不带参数，B可以为空
C表示这个方法调用之前等待的时间
D表示这个方法多久调用一次
也可以直接写重载构造函数：

var timer = new System.Threading.Timer(new TimerCallback(A));
其他设置可用 ：

timer.Change(D, C);

**实例：**

```
       private void FormMain_Load(object sender, EventArgs e)
        {
            setTaskAtFixedTime();
        }
        /// <summary>
        /// 设置定时器在零点执行任务
        /// </summary>
        private void setTaskAtFixedTime()
        {
            DateTime now = DateTime.Now;
            DateTime zeroOClock = DateTime.Today.AddHours(0.0); //凌晨00:00:00
            if (now > zeroOClock)
            {
                zeroOClock = zeroOClock.AddDays(1.0);
            }
            int msUntilFour = (int)((zeroOClock - now).TotalMilliseconds);

            var t = new System.Threading.Timer(doAt0AM);
            t.Change(msUntilFour, Timeout.Infinite);
        }

        /// <summary>
        /// 
        /// </summary>
        /// <param name="state"></param>
        private void doAt0AM(object state)
        {
            //调用数据备份功能
            
            //再次设定
            setTaskAtFixedTime();
        }
```


 这篇文章是抄自51CTO, 网址发下：


Copy From: <https://blog.51cto.com/yataigp/3585200>