# 运行项目

    下载项目后请用服务器方式打开网页即可项目

# 大鱼吃小鱼

1. 功能分析

    - 蓝色大海背景
    - 紫色海葵左右摆动
    - 海葵创建食物
    - 大鱼负责吃食物    
        吃蓝色100分   吃发横色200分
    - 小鱼跟着大鱼游戏


2. 游戏的基本代码
    - index.html
        ```
        800*600
        画布一: (前) 大鱼/小鱼/分数
        画布二: (后) 背景/海葵/食物
        ```
    - deltaTime
        ````
        游戏中不同角色通用操作切换图片[大鱼眼睛]
        睁眼和闭眼
        使用智能定时器 requestAnimationFrame 计算当前画图形需要多长时间
        12ms ~ 100ms  当前绘制图片需要 50ms ~ 100ms
        deltaTime 此变量用于保存绘制整个画面最佳间隔时间
        此值为动态变量
        ````
    
    - 绘制海葵  (绘制曲线) (贝塞尔曲线)
        ````
        p0 起点
        p2 起点
        p1 控制点
        曲线从 p0 开始向 p2 绘制曲线，p1 辅助点，如果 p1 离 p0p2 远离曲线尖如果离近曲线平滑

        海葵: 
            1. 海葵 50 条海葵
            2. 每一条起点坐标，终点坐标  控制点坐标(可通过计算得到不用保存)
            3. 摆动的幅度 5~50
            4. 创建变量保存正弦函数计算结果  -1~1
            5. 如何绘制一条贝塞尔曲线     公式 100+20*0.9
                ctx.beginPath();
                ctx.moveTo(起点坐标x, 起点坐标y);
                ctx.quadraticCurveTo(控制点x, 控制点y, 终点x, 终点y);
                ctx.stroke();
                
                贝塞尔曲线
                ctx2.quadraticCurveTo(
                    控制点x(起始坐标) 控制点y(摇杆摆动的范围)
                    this.rootx[i], canHeight - 100,
                    终点x(摇杆)     终点y(相对于画布的高度)
                    this.headx[i], this.heady[i]
                );
        ````

    - 创建游戏的食物
        ````
        食物一共 30 个其中 15 个显示还有 15 个隐藏，从海葵头顶(终点坐标)出生,向上漂浮如果食物漂浮出屏幕，当前食物隐藏

        食物池: 30食物  15个显示  15个隐藏
          - 如果当前食物被大鱼吃掉或者漂浮出屏幕: 修改状态 false 隐藏
          - 从隐藏的食物挑一个显示，保证屏幕上只显示 15 个食物
        
        1. 30 个食物 15 个显示 15 个隐藏
        2. 食物通过图片显示  蓝色居多(100)  橙色很少(200)
        3. 食物行为
            出生: 图片  14px*  cx2.drawImage(blue, 100, 100, 14, 1);  由小变大
            向上漂浮: 修改 y -= 时间差
            被大鱼吃掉: 隐藏
        ````

        - 创建游戏 鱼  mom.js
            ````
            大鱼一条 面向鼠标慢慢游动, 碰到食物吃掉
                大鱼喂小鱼
            
            1. 大鱼一条
            2. 位置 x,y
            3. 游动角度 angle
            4. 睁眼 两张图片
            5. 身体  八张图片
            6. 尾巴  八张图片
            ````
        
        - 大鱼游动
            ````
            1. 为画布绑定鼠标移动事件，在事件中欧冠获取鼠标当前的坐标
                x, y
            2. 大鱼面向鼠标游动的公式
                100鼠标  200大鱼
                200 + 100 === 距离差
                100 + 100 * 0.98 = 199
                大鱼 *= 199
            ````

        - 大鱼吃食物  碰撞检测 collsions.js
            ````
            大鱼位置与某个食物位置非常接近，表示食物被吃掉
            食物消失  alive = false  自己的数据自己修改
            ````

        - 得分/分数
            ````
            - 记录分数
            - 将分数绘制到画布1
            - 当大鱼吃到食物判断  蓝色(100)  橙色(200)
            ````

        - 得分后的光环  wave.js
            ````
            - 当大鱼吃到食物显示光环，提示玩家吃到食物
            - 大鱼吃食物光环多个
            - 数组中保存 10 个光环  半径 圆心
            ````
