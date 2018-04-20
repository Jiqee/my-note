------
title:Fragment生命周期
------

相较于Activity的生命周期，Fragment多了几个生命状态，常见的生命状态基本一样。

从上往下排列如下：
	onAttach(Activity) --绑定Activity
	👇
	onCreate(Bundle) --常用来做初始化操作
	👇
	onCreateView(LayoutInflater, ViewGroup, Bundle) --初始化布局组件
	👇
	onActivityCreated(Bundle) --知会Activity已完成哦那Create()操作
	👇
	onViewStateRestored(Bundle) --保存视图状态
	👇
	onStart() --是当前Fragment可视
	👇
	onResume() --提供与外部的交互
	👇
	onPause() --推出与用户的交互，即未获得焦点
	👇
	onStop() --暂停的状态
	👇
	onDestroyView() --清理视图资源
	👇
	onDestroy() --销毁Fragment状态
	👇
	onDetach() --与Activity解绑