# HW8 纹理实验：窗边茶桌（尚待远程验证）

依据[官方HW8手册](https://web.stanford.edu/class/cs148/hws/hw8/cs148-hw8.pdf)，本实验制作两个独立UV、重建两种参考纹理，并继续完善最终项目原型。现场问答未评估，当前没有课程成绩或最终项目完成声明。本仓库是自建自学实现，不是官方starter fork。

## 来源与场景

沿用HW7修正版 `8223d1c371a5f4bbe08e2012e56644bd9c71e95c`、run `45402760046` 的 `tea-table.blend`；载入前同时验证原始result的提交/运行号及.blend SHA256 `d1213f1f2fca62fc465c5cc5907441e343341a408e3dd3877bf1bca14b05debe`。HW7图像审查仍在进行，本实验使用其原型并不意味着HW7已验收。

两张真实表面参考已下载、核对提供方MD5及独立SHA256，并实际查看：

- [深棕木纹](references/wood_table_001.jpg)，来源[wood_table_001](https://polyhaven.com/a/wood_table_001)：纵向木纹与较粗的纹理带，用于桌面。
- [红白格织物](references/fabric_pattern_07.jpg)，来源[fabric_pattern_07](https://polyhaven.com/a/fabric_pattern_07)：规则格子和经纬纱线，用于书本上封面。

上述资产采用[Poly Haven CC0许可](https://polyhaven.com/license)。不使用网站截图或提供方示例渲染替代自己的Cycles图像。六张1k JPEG颜色/OpenGL法线/粗糙度贴图的精确下载URL、大小和SHA256见[固定清单](../assets/hw8-textures.json)；不能把下载贴图本身当作完成纹理重建。

## UV、节点和检查

桌面与上书封分别执行Smart UV Project，保留独立具名UV，导出每面坐标JSON和SVG布局。选择这两个物体是为了在茶桌构图中清楚展示大面积木纹与布面书封；原型里的青瓷杯继续保留原材质，不宣称已为杯子建立UV。

UV Map节点显式连接三个Image Texture的Vector。颜色使用sRGB，法线/粗糙度使用Non-Color；OpenGL法线通过Normal Map接入Principled BSDF，强度0.35，粗糙度单独接入。所有图片嵌入.blend，避免跨机丢失外部贴图。

结构检查覆盖每面非退化UV、坐标边界、节点来源和颜色空间及图片打包；两项负例实际删除桌面UV、切断Base Color输入，必须被拒绝，再恢复并重新检查。这些不能代替视觉核查。应检查棋盘格的方向/拉伸、木纹方向、织物比例、接缝、凹凸与光照表现。

## 远程实验与待完成项

独立 `hw8-texturing.yml` 调用集中 `ai_agent_scripts/py/render_cs148_texturing.py` 的固定任务包；仅在Actions使用Blender4.4.0/NumPy2.2.6 CPU Cycles。输出960×540的UV棋盘格（32 samples）、整体原型和木纹/织物局部图（64 samples），以及EXR、带贴图.blend、UV布局、真实耗时、SHA及负例记录。小型review工件与完整工件分别上传。

当前仅实现并做AST检查，尚未取得本HW8的远程运行结果。需要下载工件、逐项核hash、实际查看四幅图和两张UV布局，再写实测结果与局限。后续项目计划：修正可能的纹理拉伸/比例，完善窗外背景和桌面细节，按最终项目完整手册逐项补齐要求，提升采样并检查噪点；不预设这些已经完成，也不以本原型代替最终项目。
