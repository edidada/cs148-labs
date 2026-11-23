# HW5 全局光照实验报告

依据[官方手册](https://web.stanford.edu/class/cs148/hws/hw5/cs148-hw5.pdf)，实现面积光源、样本累积/平均、Corput像素扰动和间接漫反射。实验使用官方工程，仓库是自学实现库，不是已确认的官方GitHub starter fork。

## 实现与复现

官方现代版资产：`https://web.stanford.edu/class/cs148/hws/hw5/cs148-hw5-4_4.blend`，SHA256 `0540023e22bc01ed98be39e005ba86b837225bd14762077be8f970f5f9da9995`。实际Blender Text.as_string与独立DNA解析结果逐字一致。

集中脚本库提交`5ac95c6a17181044d6c7395d9cb011079ea500ba`的`py/run_cs148_hw5.py`完成圆盘sqrt均匀面积采样、发光单面朝向及世界变换、累积平均、2/3基Corput像素扰动、稳定切线基半球间接漫反射。保留深度终止、自相交偏移与题目指定的余弦权重。完成后的插件SHA256为`a15a93878543b8d0df96db7e7598d601133d7efe02b4f6ba81eaeb3c663c25a3`。

每个实验独立YAML；本实验使用`.github/workflows/hw5-global-illumination.yml`。GitHub Actions Ubuntu24.04、Python3.11、Blender4.4.0、NumPy2.2.6，CPU渲染，随机种子20261123。脚本包ZIP SHA256为`c99c45348aa741f3d46037cc66a6a03213b9349e31d8eaf9fbaab94714ac5bff`，包内保存来源提交与逐文件哈希。没有公开集中私有库的其他脚本，也不使用个人令牌。

## 实际结果

[完整运行45185146659](https://github.com/edidada/cs148-labs/actions/runs/45185146659)在源提交`9b321268f088a7bc52f779e92cd18f040a2d6bfd`完成并成功。四阶段均为480×480、100%比例、递归深度3：

| 阶段 | 样本 | Corput | 间接漫反射 | 耗时秒 |
|---|---:|---|---|---:|
| area-single | 1 | 否 | 否 | 26.83 |
| area-four | 4 | 否 | 否 | 104.09 |
| corput-four | 4 | 是 | 否 | 106.00 |
| indirect-sixteen | 16 | 是 | 是 | 1872.55 |

下载并核对四张PNG的尺寸、元数据及SHA256，人工查看了四张实际场景图。场景包含红绿墙、两箱体与玻璃/镜面球，面积阴影与反射可见；间接光照使暗部出现照明与颜色传递。间接阶段记录20,248,873次权重调用，其中11,239,594次正贡献，线性RGB均值为[0.1133144,0.1175478,0.0683301]。16样本仍有明显蒙特卡洛噪声，不宣称收敛或无噪声。

数值检查包括圆盘E[r²]=0.4948864（理论0.5）、发光正背面[1,0]、单位半球方向/余弦、常量入射积分、真实渲染循环的恒定辐射平均和Corput射线扰动。删除sqrt、翻转半球、删除间接余弦、删除平均除法、禁用Corput的五项受控破坏全部被检测。解析夹具与真实场景渲染分别保存。

Actions每阶段工件包含PNG、线性RGBA数组、完成后的.blend、result.json和日志。按运行号下载：`gh run download 45185146659 --repo edidada/cs148-labs`。课程台账另永久保存四张PNG与结果JSON。各阶段result.json中的accepted=false表示单阶段不能独立验收整个实验，并非执行失败。保存的completed-hw5.blend包含完整四项实现，最终indirect-sixteen版本配置对应完整效果；前面阶段图以result.json中的实际功能开关为准。

## 边界与历史

手册现场演示/问答无法由自动检查代替；本报告仅记录自学实现和公开可复现的运行证据。按题目公式实现的间接权重不额外宣称是经过PDF归一化的无偏路径追踪器。课程其他作业仍待完成。

首次45184095389因公开课程GITHUB_TOKEN无法检出私有集中脚本库而失败；改为只发布本任务脚本的固定哈希附件。环境审计45184478608通过。此次报告提交后仍需等待报告最终HEAD的完整Actions结果，最终提交与验收台账记录在langnote/todo.md。
