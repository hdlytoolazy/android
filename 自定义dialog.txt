自定义的dialog首先必须继承系统的dialog（好像是废话）

public MultipleDialog(Activity context, int themeResId)
构造方法中第一个参数上下文，没什么好说的；第二个参数主题的id，如果有想用的主题，无论是原生还是自定义的都可以，如果不想用可以给4（如果给0的会有系统给默认赋一个id，但为什么是4我也不清楚）；

在构造方法中，常见的还有调整显示的位置、大小、透明度等等，这些必须要在show方法之前完成，否则是无效的；
        setCancelable(true);  是否能被消失
        setCanceledOnTouchOutside(false); 是否点击dialog以外区域能被消失

        WindowManager windowManager = context.getWindowManager();
        Display display = windowManager.getDefaultDisplay();
        Point point = new Point();
        display.getSize(point);

        Window dialogWindow = getWindow(); 可以用这个来改变dialog的位置
        WindowManager.LayoutParams lp = dialogWindow.getAttributes();
        dialogWindow.setGravity(Gravity.CENTER);
        dialogWindow.addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);

        requestWindowFeature(Window.FEATURE_NO_TITLE); dialog无title模式

        lp.x = 0;  新位置X坐标
        lp.y = 0;  新位置Y坐标
        lp.width = (int)(point.x  0.8);  宽度
        lp.height = (int)(point.y  0.25);  高度
        lp.alpha = 1.0f;  自身透明度
        lp.dimAmount = 0.5f; activity透明度

         getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_RESIZE 
                WindowManager.LayoutParams.SOFT_INPUT_STATE_HIDDEN); 若dialog中输入框被软键盘挡住，则如此设置
        dialogWindow.setAttributes(lp);
        dialogWindow.setBackgroundDrawableResource(R.drawable.dialog_forward_shape); 注意，如果需要自定义的边界，不仅要在xml文件中引用，还要在这里引用，否则可能会达不到效果

        正常情况完事了，再说下不正常的情况。UI的哥们把dialog弄成圆角的了，我想了想：卧槽，有毛区别。诶，最终还是得改。 
        把圆角的shape（上面的R.drawable.dialog_forward_shape）放到布局上，以为就完事了，结果好么，上面没问题，下面就是不行。
        布局最下面两个横向march_parent的按钮，我把按钮也给了半圆角的shape还是不行，最后没招了，直接把包裹按钮的layout硬是padding
        出一个半径的距离出来，总算好了，真tm难受。