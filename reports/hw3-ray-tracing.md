# HW3 光线追踪实验报告

依据[官方手册](https://web.stanford.edu/class/cs148/hws/hw3/cs148-hw3.pdf)，在官方Blender工程中实现六项TODO：有限距离阴影、逆平方Lambert/Blinn-Phong、遮挡环境项、递归镜面反射、Schlick Fresnel、内外折射率及全反射判定。保留官方相机、场景、渲染循环。本仓库为自学实现库，不是官方GitHub starter fork。

## 来源与实现

官方现代版资产SHA256：`68edaa67bc3e50acfe83a4781f2e751ea34264b383b84da969cfcc4358835da7`。实际Blender Text与独立DNA读取逐字一致。集中实现为edidada/ai_agent_scripts的`py/run_cs148_hw3.py`，固定提交`fb00feb87839e6eec6525bd0fb8d0d182e2afccf`。本任务工具ZIP SHA256：`49b34fa0b731f35b4da3be69e25b4fd26082a8fdbe3fe6fd86a8da2916839de0`，来源和逐文件哈希另存provenance.json。

阴影射线只考虑交点到光源的有限线段；直接漫反射与高光使用距离平方衰减、非负余弦。反射使用归一化方向与表面外偏移；折射根据射线内外切换IOR比率，以判别式识别全反射，透射从表面内侧偏移。递归深度为0时终止。Fresnel按手册Schlick近似及材料开关启用。各检查点逐步开启后续功能，2a仅漫反射、2b加入高光。

## 实际运行证据

[完整运行45194249570](https://github.com/edidada/cs148-labs/actions/runs/45194249570)在源提交`61c38cd0fa898888af6801e9bd5cf542c859e991`成功。Ubuntu24.04、Python3.11、Blender4.4.0、NumPy2.2.6，全部真实场景图为960×540、100%比例、深度3，未在本地执行课程实验。

| 检查点 | 耗时秒 | 核图结果 |
|---|---:|---|
| 1 | 11.30 | 原场景几何与硬阴影 |
| 2a | 15.17 | 漫反射与距离衰减 |
| 2b | 17.26 | 黄色半球等物体出现高光 |
| 3 | 17.40 | 阴影区域环境项变化 |
| 4 | 32.28 | 碗和蓝色物体出现反射 |
| 5 | 32.98 | 黑色块及地面反射随Fresnel变化 |
| 6 | 38.78 | 黑色块透射/折射后方物体 |

七张实际PNG全部下载、检查960×540头部尺寸、配置与SHA256并逐图查看；不据此宣称与手册截图逐像素相同。最终插件SHA256为`0e0a59bb1e4bedffa59a433d13007a2a2d1fc94ad84fd2d3e9d4950392edd1ea`。永久图像/元数据/核图记录在langnote的courses/lab-reports/CS148-HW3-evidence。

数值夹具检查反射方向、Schlick正视/掠射/斜射、法向折射及全反射、有限阴影线段、逆平方直接RGB[1.4,4.2,8.4]、阴影环境RGB[.025,.05,.075]、递归深度与偏移两侧。无界阴影、删除逆平方、删除环境项、反射符号反转、Schlick指数改变、TIR判定反转六项受控破坏全部被检测。解析夹具与七张真实场景渲染分别提供证据。

独立工作流`.github/workflows/hw3-ray-tracing.yml`逐阶段上传PNG、线性RGBA数组、完成.blend、配置/源码/图像哈希及日志。下载命令：`gh run download 45194249570 --repo edidada/cs148-labs`。各阶段result.json的accepted=false表示单阶段不能独立验收整个实验，不是执行失败。各完成.blend保存该检查点的功能开关，检查点6保存完整实现。

## 历史与边界

首轮6c6d6a5/run45193746936正常数值基线通过，但TIR受控负例返回None后，检查器向np.allclose传入None产生TypeError。集中脚本改为先明确断言法向入射必须透射，保留所有方向/全反射检查，不扩大捕获范围放过错误。修复后六负例和七完整渲染均成功。

Blender打开文件时提示资产由较新405.90二进制写入，已在实际4.4.0环境验证Text、场景与全部渲染；仍不宣称所有新版本私有数据都无损。音频NumPy扩展初始化警告不影响本次CPU图像检查，日志保留。现场问答/演示、其他作业及物理无偏渲染不在此自动验收范围。此报告提交后须等待最终HEAD完整Actions复验，结果登记于langnote/todo.md，不将报告前旧绿灯视为最终提交证据。
