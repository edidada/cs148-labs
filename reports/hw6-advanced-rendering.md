# HW6 高级渲染实验报告

依据[官方13页手册](https://web.stanford.edu/class/cs148/hws/hw6/cs148-hw6.pdf)，实现九个光照通道，以及运动模糊、景深、参与介质三组非Cornell参考重建和效果开关对照。仓库是自建自学实现，不是官方GitHub starter fork。现场随机问答、助教演示未评估，不据本报告声称取得课程成绩或整个课程完成。

## 实现与复现

独立分支`edidada/hw6-advanced-rendering`，独立YAML `.github/workflows/hw6-advanced-rendering.yml`。所有Blender执行在GitHub Actions；本地仅源码/工件/图像审查。Ubuntu24.04、Python3.11、Blender4.4.0、NumPy2.2.6，Blender轮子SHA256固定为50ee92155cdb8740214fa3b29d9a45ee90cda9dc15eb978860916de468b8580f。

集中脚本库`edidada/ai_agent_scripts`：TODO1工具bb5315bba3f8e83799ece55389a218c1866b9ca6的py/render_cs148_passes.py；三场景工具382a13c078a4ca2c42974538dccc528727c71f44的py/render_cs148_fog.py、render_cs148_dof.py、render_cs148_motion.py。公开release仅包含对应任务脚本及来源文件哈希，不公开整个私有脚本库。三场景ZIP SHA256为6b867966f2657852abf2a82dd9a206648755123d7f1ffd66d1053feff5583b97。

复现可手动运行本分支workflow_dispatch；完整运行会生成passes、fog、dof、motion和references五组工件。场景工件保存32位线性EXR、PNG、.blend、result.json、脚本来源及安装依赖清单。每个result记录精确提交/run和逐文件哈希，不能把早期绿灯当作修改后最终提交的验收证据。

## TODO1：九个光照通道

[官方Cornell工程](https://web.stanford.edu/class/cs148/hws/hw6/cornell_box.blend)1102612字节，SHA256 267b808a404b9be94a63c9b01d3fab007ab90ebf0eca82679dafb0c70c5b47a2。保留相机/几何/材质/480x480，CPU Cycles256samples，无降噪，固定种子20261124。保存combined和九个光照通道，另存emission/environment以核对线性组合。

首次完整数值及逐图核验run45279835140：最大绝对组合误差1.33514404296875e-05，删除主要直射分量及颜色权重两项受控破坏均被检测。后续run45282684197的passes再次成功。九图实际观察与原理：

| 通道 | 实际观察与解释 |
|---|---|
| Diffuse Direct | 两球黑，墙/箱受光灰白：球材质不提供漫反射分量。 |
| Diffuse Indirect | 红绿渗色来自墙面反弹，图中仍有噪点。 |
| Diffuse Color | 材质平涂色，不包含照明/阴影。 |
| Glossy Direct | 两球中只显现面积光源的白高光。 |
| Glossy Indirect | 球中反射房间，右球中心黑；透射贡献不属于此通道。 |
| Glossy Color | 左球反射权重大；右球灰、边缘亮，符合Fresnel角度效应。 |
| Transmission Direct | 全黑；厚玻璃需要进入和离开两次透射，不以非黑断言误判。 |
| Transmission Indirect | 右球出现背景及折射后绿墙，补足反射通道缺少的中心。 |
| Transmission Color | 只有右球提供透射权重。 |

在线性EXR上验证`Combined = sum((Direct + Indirect) * Color) + Emission + Environment`，sum遍历diffuse/glossy/transmission。不能把经过显示变换的PNG直接相加。

## TODO2：运动模糊参考与重建

参考：[Markus Spiske的地铁照片](https://www.pexels.com/photo/light-city-train-tunnel-2990658/)。原站提供1280像素缩放图，由references job完整下载，已实际查看，作者与来源TSV/文件哈希存入工件。未生成或修改参考照片；许可见[Pexels License](https://www.pexels.com/license/)。

以白车身红条、低机位、固定站台与横向运行作对应尝试。列车为简化占位方体/车窗，位置关键帧1至20从-12到12、线性插值，24fps，在第10帧以1帧快门即1/24秒曝光；对照关闭运动模糊。真实Cycles时间积分，未用后期整图模糊。

run45282684197实际图中车体沿轨道方向模糊、轨道与站台保持清晰，线性像素平均绝对差0.01659070886671543。参考是亮车窗/红边更丰富的室内车站，本图缺顶棚、标牌、瓷砖和完整车轮，光照/车身尺度不同；只能作为题目允许占位几何的重建尝试，不称照片级复现。

## TODO3：景深参考与重建

参考：[Mike van Schoonderwalt的棋子照片](https://www.pexels.com/photo/king-and-queen-chess-pieces-5502523/)，同样已下载并查看，来源与哈希随references工件保存。前排双浅色棋子清晰，后排四深色棋子模糊。

本场景以兵的简化几何代替参考王后，保留前双浅色/后四深色分组。65mm/f1.4，焦点前排棋子，物理棋子约10cm、拍摄距离约1m，640x426/128samples，与针孔基线比较。早期一米棋子/十米拍摄导致模糊太弱，虽然CI通过仍拒绝视觉验收；缩放物理尺寸后实际焦外效果明显，再按参考调整前排分组。run45282684197实际图前两棋子清晰、后四及远近棋盘模糊。

形状为兵而非王后，哑光象牙材质而非参考灰亮面，背景/视角也不同。数值差异只是效果检查，实际逐图审查是必要证据。

## TODO4：参与介质参考与重建

参考：[KoolShooters的雾夜街道](https://www.pexels.com/photo/foggy-street-during-night-time-6494669/)，已下载查看。竖构图、左侧近灯、道路消失点和两侧树影为对应依据。最初Commons路灯候选下载429，Pexels本机403；runner最终取得实际照片，并非用页面文字冒充审图。

426x640竖构图、左侧三盏递远点光源、两侧树木占位，Principled Volume密度0.07、anisotropy0.25。相同场景分别有无介质，CPU Cycles128samples，保存两份场景/EXR/PNG。run45282684197实际图存在灯周散射光晕、远处能见度下降和树影，平均绝对差0.05651366710662842。

当前画面较参考暗、色调更暖，树木为锥形占位且缺电线/房屋；雾中噪点明显。报告保留这些差异，不把黑背景三锥光束的早期演示当作最终参考重建。

## 运行与证据边界

实现提交5c537ac94ba50a4e8711f8829e89585404651c7e的[run45282684197](https://github.com/edidada/cs148-labs/actions/runs/45282684197)五job全部success，三个场景完整EXR/PNG/.blend逐文件hash已核对，实际图像已与参考比较。第一轮fog失败为headless Render Result像素缓存为空，改为读取保存的32位EXR后修复；NumPy音频兼容警告保留，未阻止真实Cycles渲染。

证据归档于langnote的courses/lab-reports/CS148-HW6-evidence：passes-45279835140、references-45282311911、reference-aligned-45282684197，含来源、原始result和PNG审图。完整EXR/.blend保留在GitHub artifact及本地完整下载目录。

本报告新增提交仍需最终HEAD全量Actions与工件复核。现场随机问答/助教评分未评估；高样本收敛、照片级材质复刻和整个CS148课程完成均不在现有证据之内。
