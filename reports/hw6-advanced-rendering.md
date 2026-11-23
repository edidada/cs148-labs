# HW6 高级渲染实验报告（实施中）

官方要求：https://web.stanford.edu/class/cs148/hws/hw6/cs148-hw6.pdf 。本分支是自学实现，不是官方GitHub starter fork。

已实现TODO1的远端执行入口：使用固定SHA256官方Cornell工程、Blender4.4.0、256 samples、固定随机种子，保留工程相机/几何/材质/分辨率，保存九光照通道及combined的32位EXR、PNG、工程副本和来源。在线性空间验证光照组合，并以删除主要直射分量及颜色权重的破坏样例检查验证器。实际运行与图像尚待Actions核验，不能据源码宣称通过。

脚本源：edidada/ai_agent_scripts提交bb5315bba3f8e83799ece55389a218c1866b9ca6，py/render_cs148_passes.py；公共release仅含该任务脚本和来源清单。

未完成：运动模糊、景深、体积效果各自参考图和非Cornell场景重建、对照图、制作说明；九通道实际图像及材质解释核实；现场展示与随机问答未评估。完整范围见langnote的courses/lab-reports/CS148-HW6-scope-audit.md。本地未运行Blender，独立workflow为.github/workflows/hw6-advanced-rendering.yml。

## 参与介质重建草稿（待实测）

独立fog job使用集中脚本c735614e6386833f5c8e3bf3e59fa319d5b593e8的render_cs148_fog.py，创建非Cornell夜间路灯场景，保存有雾/无雾两份工程、640x426/128samples真实Cycles对照与线性像素差异。来源候选为Thomas Sørenes的Streetlight in fog (8187998658)，CC BY2.0：https://commons.wikimedia.org/wiki/File:Streetlight_in_fog_(8187998658).jpg 。原图下载429，未取得或逐图核对，当前仅为构图草稿，不能当作已完成参考重建。远端API、非黑图像、雾效质量与参考对应均待核验。

## 运动模糊草稿（待实测）

集中工具9cb0a27adab062317a8ffedcf383543d7db197b6的render_cs148_motion.py构造非Cornell列车/站台，24fps、第10帧、1帧快门、线性位置关键帧，分别冻结与真实Cycles时间采样；保存32位EXR/PNG/场景和曝光参数。参考获取及对应尚未完成，远端执行和实际图像待验证，不以差异数值代替合理模糊审查。

## 景深视觉审查后修正

首轮f9cb223实际图像中焦外效果过弱，不按绿灯验收。集中f180d1868e796d83c3dbeb5a69121363189b9ca0将棋子、相机和灯的位置/几何等比例缩至0.1（约10cm棋子/1m拍摄），保留65mm/f1.4；面积灯尺寸同步缩放、能量按面积缩放。新图必须实际核对中间焦点与前后模糊，数值差异不能独立证明景深正确。
