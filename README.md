# CS148 自学实验

来源为Stanford CS148 Fall2026 [官方作业页](https://web.stanford.edu/class/cs148/assignments.html)。这是edidada自建的自学仓库，**不是官方GitHub starter的fork**。官方.blend下载地址、完整SHA256与手册版本见各实验报告。

Python实现统一维护在[ai_agent_scripts/py](https://github.com/edidada/ai_agent_scripts/tree/main/py)，课程工作流使用固定来源及哈希的本任务工具附件；集中脚本库访问权限独立管理。所有Blender、构建和课程实验只在GitHub Actions运行。

## 实验入口

| 实验 | 独立分支 | 工作流 | 报告 |
|---|---|---|---|
| HW3 光线追踪 | [edidada/hw3-ray-tracing](https://github.com/edidada/cs148-labs/tree/edidada/hw3-ray-tracing) | [hw3-ray-tracing.yml](https://github.com/edidada/cs148-labs/blob/edidada/hw3-ray-tracing/.github/workflows/hw3-ray-tracing.yml) | [实验报告](https://github.com/edidada/cs148-labs/blob/edidada/hw3-ray-tracing/reports/hw3-ray-tracing.md) |
| HW5 全局光照 | [edidada/hw5-global-illumination](https://github.com/edidada/cs148-labs/tree/edidada/hw5-global-illumination) | [hw5-global-illumination.yml](https://github.com/edidada/cs148-labs/blob/edidada/hw5-global-illumination/.github/workflows/hw5-global-illumination.yml) | [实验报告](https://github.com/edidada/cs148-labs/blob/edidada/hw5-global-illumination/reports/hw5-global-illumination.md) |
| HW6 高级渲染 | [edidada/hw6-advanced-rendering](https://github.com/edidada/cs148-labs/tree/edidada/hw6-advanced-rendering) | [hw6-advanced-rendering.yml](https://github.com/edidada/cs148-labs/blob/edidada/hw6-advanced-rendering/.github/workflows/hw6-advanced-rendering.yml) | [实验报告](https://github.com/edidada/cs148-labs/blob/edidada/hw6-advanced-rendering/reports/hw6-advanced-rendering.md) |
| HW7 几何建模 | [edidada/hw7-geometric-modeling](https://github.com/edidada/cs148-labs/tree/edidada/hw7-geometric-modeling) | [hw7-geometric-modeling.yml](https://github.com/edidada/cs148-labs/blob/edidada/hw7-geometric-modeling/.github/workflows/hw7-geometric-modeling.yml) | [实验报告与草图](https://github.com/edidada/cs148-labs/blob/edidada/hw7-geometric-modeling/reports/hw7-geometric-modeling.md) |
| HW8 纹理 | [edidada/hw8-texturing](https://github.com/edidada/cs148-labs/tree/edidada/hw8-texturing) | [hw8-texturing.yml](https://github.com/edidada/cs148-labs/blob/edidada/hw8-texturing/.github/workflows/hw8-texturing.yml) | [纹理报告与实测限制](https://github.com/edidada/cs148-labs/blob/edidada/hw8-texturing/reports/hw8-texturing.md) |

工作分支保存独立实现和证据，main提供索引。HW3需七个960×540检查点，HW5需四组480×480、深度3、1/4/16样本渲染；成功加载场景不等于实验完成。实际提交、Actions运行号、工件核验与未完成列表以[跨机台账todo.md](https://github.com/edidada/langnote/blob/master/todo.md)为准。HW3/HW5/HW6的明确自学范围已完成最终HEAD工件与图像审查。HW7修正版五图与11项完整工件哈希已复核，最终报告提交838a5ef已通过CI并复核该HEAD五图及11项工件哈希；HW8两套UV/PBR材质、四图及20项完整工件哈希已复核，最终报告提交ed90813已通过CI并复核该HEAD四图及20项工件哈希。噪声、木材高光和织物比例限制见报告。HW1、HW2、HW4及最终项目仍未完成；现场演示/问答无法由自学自动检查代替。

复现示例：先检出相应分支，再从Actions页手动执行对应workflow。工件可按报告运行号下载：`gh run download RUN_ID --repo edidada/cs148-labs`。每阶段包含实际图像、线性数组、完成.blend、配置、哈希和日志。不得将一个阶段成功或报告前的旧绿灯当成最终提交完成。
