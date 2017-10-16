moveTo(x,y)
不会进行绘制，只用于移动画笔至(x,y)。
结合以下方法进行使用，用来改变初始画笔位置。

lineTo(x,y)
从当前画笔的位置画直线到(x,y)坐标

以上两个方法足以画出任何各边为直线的图形。
下面的方法主要用于实现含有曲边的图形

quadTo(x1,y1,x2,y2)
(x1,y1) 为控制点，(x2,y2)为结束点。
二阶贝塞尔曲线。

cubicTo(x1,y1,x2,y2,x3,y3)
(x1,y1) 为控制点，(x2,y2)为控制点，(x3,y3) 为结束点。
三阶贝塞尔曲线。

arcTo(ovalRectF, startAngle, sweepAngle)
ovalRectF为椭圆的矩形，startAngle 为开始角度，sweepAngle 为结束角度。
注：当RectF为正方形时，即为1/4圆弧


贝塞尔曲线：https://www.2cto.com/kf/201701/591316.html