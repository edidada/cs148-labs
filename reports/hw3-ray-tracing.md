# HW3 光线追踪（实现已接入，待真实运行验收）

依据[官方手册](https://web.stanford.edu/class/cs148/hws/hw3/cs148-hw3.pdf)，本自学仓库补全官方Blender工程的六项TODO：有限距离阴影、距离平方衰减的Lambert/Blinn-Phong、遮挡时环境项、递归镜面反射、Schlick Fresnel、内外折射率与全反射判定。保留官方相机/场景/渲染循环，不借用HW5图片。

集中实现来源edidada/ai_agent_scripts提交c3d2c73419dced38ca7670ecfcbeca92e2ab655c的py/run_cs148_hw3.py。官方现代版资产SHA256为68edaa67bc3e50acfe83a4781f2e751ea34264b383b84da969cfcc4358835da7。工作流对资产哈希、独立DNA解析与实际Blender Text逐字比较，固定bpy4.4.0及NumPy2.2.6。

数值夹具验证反射方向、Schlick正视/掠射/斜射、法向折射及全反射、光源以外遮挡忽略、实际逆平方直接RGB、阴影环境项、递归深度与偏移两侧。六种受控破坏必须失败。夹具只验证解析约束，不能替代真实场景图像。

独立.github/workflows/hw3-ray-tracing.yml按检查点1、2a、2b、3、4、5、6生成960x540、100%比例、深度3的真实场景PNG、线性RGBA数组、完成.blend、配置/哈希及日志。各点逐步启用后续功能；2a是仅漫反射，2b启用高光。每阶段单独上传工件，脚本不在本地执行课程实验。

当前仅脚本/替换模板AST静态检查通过，实际API兼容、数值/负例、七张完整图像、人工核图与最终提交Actions均待验。工作流成功前不得登记完成。现场演示/课程问答不由自学自动检查代替。16位/色彩变换以实际图像元数据为准，不宣称物理无偏或完成其他作业。
