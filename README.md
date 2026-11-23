# CS148 自学实验

来源为 Stanford CS148 Fall2026 [官方作业页](https://web.stanford.edu/class/cs148/assignments.html)。这是 edidada 自建的自学仓库，**不是官方 GitHub starter 的 fork**；目前没有核实到可 fork 的官方实验仓库。官方 `.blend` 下载地址与哈希须单独保留。

Python 脚本统一放在 [ai_agent_scripts/py](https://github.com/edidada/ai_agent_scripts/tree/main/py)，课程 workflow 引用固定提交，不在本仓库另放 Python 源文件。所有 Blender、构建和课程实验仅在 GitHub Actions 运行。

HW1–HW8 和最终项目均未验收完成。每个实验使用独立 `edidada/hwN-*` 分支与独立 workflow；场景导入或脚本注册成功只证明环境可用，不能替代题目要求的实现、图像和报告。

HW5 当前工作分支：`edidada/hw5-global-illumination`。该分支先验证 Blender4.4 环境、官方场景及内嵌源码读取，再实现面积光源、多重采样、低差异采样和间接漫反射。详见分支内 `reports/hw5-global-illumination.md`。
