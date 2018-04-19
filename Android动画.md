-----
title:Android动画
-----

### 一、动画类别
	三种动画如下：
		帧动画：相当于相同间隔时间播放一系列的图片资源，如200等人肉眼无法识别。
		视图动画：相当于把view做移动变换，平移、伸缩、翻转和透明度渐变，但操作的view焦点还在原处。
		属性动画：类似于视图动画，区别在于视图焦点随运动变化。

### 二、动画使用
	1、帧动画使用
		(1) res/drawable文件夹里面新建一个xxx.xml;
		(2) 添加animation根节点，item小节点
			<animation-list android:id="@+id/selected" android:oneshot="false">
    			<item android:drawable="@drawable/wheel0" android:duration="50" />
    			<item android:drawable="@drawable/wheel1" android:duration="50" />
    			<item android:drawable="@drawable/wheel2" android:duration="50" />
    			<item android:drawable="@drawable/wheel3" android:duration="50" />
    			<item android:drawable="@drawable/wheel4" android:duration="50" />
    			<item android:drawable="@drawable/wheel5" android:duration="50" />
 			</animation-list>
 		(3) 代码调用
 			ImageView img = (ImageView)findViewById(R.id.spinning_wheel_image);
 			img.setBackgroundResource(R.drawable.xxx);
 			AnimationDrawable frameAnimation = (AnimationDrawable) img.getBackground();
 			frameAnimation.start();
 			当然还可以通过代码来添加图片资源
 			frameAnimation.addFrame(Drawable frame,int duration)

 	2、视图动画
 		常见的有三种：
 			平移动画：
 				(1) 在res/anim下新建一个translate.xml,内容类似于下面：
 					<?xml version="1.0" encoding="utf-8"?>
					<translate xmlns:android="http://schemas.android.com/apk/res/android"
          				android:duration="3000"
           				android:fromXDelta="0"
           				android:fromYDelta="0"
           				android:toXDelta="100%p"
           				android:toYDelta="100%p">
					</translate>
				(2) 代码调用
					Animation translate = AnimationUtils.loadAnimation(getContext(),R.anim.translate);
					imageView.startAnimation(translate);
					亦或不用xml,直接持有TranslateAnimation对象
					TranslateAnimation translate = new TranslateAnimation(fromX,toX,fromY,toY);
					translate.setDuration(long duration);

 			缩放动画：
 				(1) 在res/anim下新建一个scale.xml,内容如下：
 					<?xml version="1.0" encoding="utf-8"?>
					<scale xmlns:android="http://schemas.android.com/apk/res/android"
       					android:duration="3000"
       					android:fromXScale="1"
       					android:fromYScale="1"
       					android:toYScale="2"
       					android:toXScale="2"
       					android:pivotX="50%"
       					android:pivotY="0%">
					</scale>
				(2) 代码调用
					Animation scale = AnimationUtils.loadAnimation(getContext(),R.anim.scale);
					imageView.startAnimation(scale);
					亦或不用xml,直接持有ScaleAnimation对象
					ScaleAnimation scale = new ScaleAnimation(float fromX,float toX,float fromY,float toY,int pivotXType,float pivotXValue,int pivotType,float pivotYValue);
					scale.setDuration(long duration);

 			旋转动画：
 				(1) 在res/anim下新建一个rotate.xml，内容如下：
 					<?xml version="1.0" encoding="utf-8"?>
					<rotate xmlns:android="http://schemas.android.com/apk/res/android"
        				android:duration="3000"
        				android:fromDegrees="0"
        				android:toDegrees="360"
        				android:pivotX="50%"
        				android:pivotY="50%">
					</rotate>
				(2) 代码调用
					Animation rotate = AnimationUtils.loadAnimation(getContext(),R.anim.rotate);
					imageView.startAnimation(rotate);
					亦或不用xml，直接持有RotateAnimation对象
					RotateAnimation rotate = new RotateAnimation(float formDegrees,float toDegrees,int pivotXType,float pivotXValue,int pivotYType,float pivotYValue);
					rotate.setDuration(long duration);

 			渐变动画：
 				(1) 在res/anim下面新建一个alpha.xml,内容如下：
 					<?xml version="1.0" encoding="utf-8"?>
					<alpha xmlns:android="http://schemas.android.com/apk/res/android"
       					android:startOffset="1000"
       					android:fromAlpha="1.0"
       					android:toAlpha="0.0"
       					android:duration="2000">
					</alpha>
				(2) 代码调用
					Animation alpha = AnimationUtils.loadAnimation(getContext(),R.anim.alpha);
					imageView.startAnimation(alpha);
					亦或不用xml,直接持有AlphaAnimation对象
					AlphaAnimation alpha = new AlphaAnimation(float fromAlpha,float toAlpha);
					alpha.setDuration(long duration);

	3、属性动画
		(1) 一个基类，两个子类，其中一个子类又有两个次级子类，可能统共4个类需要了解，如下：
			Animator --> AnimatorSet
			         --> ValueAnimator --> ObjectAnimator
					              	   --> TimeAnimator

		(2) AnimatorSet 
			类似于一个动画结合，统筹一系列动画的执行。常见的写法：
			AnimatorSet bouncer = new AnimatorSet();
			bouncer.play(anim1).before(anim2);
			bouncer.play(anim1).with(anim3);
			bouncer.play(anim1).with(anim4);
			bouncer.play(anim1).after(anim5);
			bouncer.start();
			注：animX指代各种属性动画的对象

		(3) ValueAnimator
			两个缺省值，Interpolator和TypeEvaluator,其他方法如：
				ofArgb(int... values) ---设置颜色变化范围值
				ofFloat(float... values) ---设置float类型的范围变化值
				ofInt(int... values) ---设置int类型的范围变化值
				ofObject(TypeEvaluator evaluator,Object... values) ---设置Object类型的范围变化值

			例如下面的使用方法：
				ValueAnimator mAnimator = ValueAnimator.ofInt(100, 0);
				mAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
					@Override
					public void onAnimationUpdate(ValueAnimator updateAnimation){
						view.doXXX();
					}
				});
				mAnimator.setDuration(6000);
				mAnimator.setTarget(view);
				mAnimator.start();
		(4) ObjectAnimator
			类似于ValueAnimator;
			ObjectAnimator animator = ObjectAnimator.ofFloat(view,"translationX",100f);
			animator.setDuration(100);
			animator.start();