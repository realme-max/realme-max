# 你好，我是冯家锐

天津大学电子信息专业硕士研究生，关注工业三维视觉、三维点云算法、机器人焊接系统与 AI 工程化部署。

主要使用 **C++、Python、Qt、PCL、OpenCV、PyTorch、ONNX、TensorRT 与 CUDA**，具备从点云模型训练、推理优化到工业任务编排的完整能力链路。

## 重点项目

### [PTV2-WeldSeg-Deployment](https://github.com/realme-max/PTV2-WeldSeg-Deployment)

面向工业焊缝点云的 **PointTransformerV2 语义分割与工程部署项目**，已打通 PyTorch、ONNX、TensorRT、自定义 CUDA Plugin、C++17 SDK、Qt/OpenGL 软件与 Windows Release 链路。

- **点云分割**：以 PointTransformerV2 为基础，引入图卷积网络GCN改进PTV2，完成焊缝点/背景点二分类。
- **模型效果**：当前公开基准中，GCN_res 在工业焊缝点云测试集上达到 **93.63% mIoU**，焊缝类别 **F1 为 94.68%**。
- **推理性能**：完整 TensorRT 纯推理平均 **4.8209 ms**，相较 PyTorch 的 **20.9776 ms** 加速 **4.3514×**。
- **CUDA 优化**：使用 CUB 重写动态 VoxelUnique Plugin，独立算子由 28.8478 ms 降至 0.1036 ms；该 **278.38×** 仅为算子级加速。
- **C++/Qt 部署**：完成 TensorRT Runtime、点云预处理、几何后处理、WeldDetector SDK、Qt/OpenGL 可视化与可搬迁 Windows Release 包。
- **端到端表现**：C++ SDK 平均检测耗时 **28.1068 ms**，其中 CPU k=6 邻接矩阵构建占 65.87%，是当前主要性能瓶颈。

**技术栈：** PyTorch · PointTransformerV2 · GCN · ONNX · TensorRT · CUDA/CUB · C++17 · Qt/OpenGL

[查看项目与部署文档 →](https://github.com/realme-max/PTV2-WeldSeg-Deployment)

---

### [weld_agent](https://github.com/realme-max/weld_agent)

面向机器人焊接引导场景的 **多阶段 Agent 编排系统**。项目将点云检测、结果校验、异常诊断、可信评审、证据追踪、问答与报告生成组织为可复现、可审计的任务流程。

- **流程编排**：采用 State-Node-Edge 状态图设计，并提供可选 LangGraph 运行入口；通过统一任务状态、条件路由和安全门控制各阶段流转。
- **工业工具接入**：通过封装调用既有 Qt/C++ 焊缝检测 CLI、几何算法与 PointNet++ 预测工具，不重写原有核心检测算法。
- **可信 Agent**：实现 Failure Diagnosis、Review/Trust Score、Evidence Pack、只读证据工具选择、本地词法 RAG、任务记忆和实验报告等模块。
- **可追溯性**：保存任务计划、中间 JSON、工具调用、状态图轨迹、审查结果、可视化页面和最终报告，支持单任务复查与批量回放。
- **安全边界**：大模型能力默认关闭；可选 LLM ReAct 必须先读取证据。系统不发送机器人指令、不执行真实逆解或轨迹规划，人工确认也仅允许恢复软件流程。

**技术栈：** Python · LangGraph/状态图 · Qt/C++ 工具封装 · PointNet++ · Plotly · 本地 RAG · MCP 只读接口

[查看项目与运行说明 →](https://github.com/realme-max/weld_agent)

---

### [IndustrialAIServiceFramework](https://github.com/realme-max/IndustrialAIServiceFramework)

面向工业 AI 应用的 **C++17 高性能服务框架**。项目把独立点云算法封装为可通过浏览器和 HTTP 调用的异步任务服务，打通网络接入、任务调度、外部进程隔离、Artifact 管理、结果下载与 WebGL2 三维展示。

- **高性能服务运行时**：实现 Linux `epoll ET` 单 Reactor、`eventfd` 跨线程唤醒、`timerfd` 超时、`signalfd` 优雅停止、非阻塞 TCP、严格 HTTP/1.1、固定线程池与有界背压。
- **双业务独立接入**：分别通过 Adapter 接入 PTV2 焊后焊缝分割和 WeldAgent 焊前建系/焊接特征计算，使用异步 Job、状态轮询和结构化结果，两个业务不自动串联。
- **浏览器分析闭环**：提供同域 Web 页面，支持本地点云上传、任务进度查询、焊缝分割叠加、起止点/拐点/坐标轴显示及输出 Artifact 下载。
- **数据与进程边界**：使用 SHA-256、严格 manifest、canonical path、symlink/root-escape 防护和结果白名单；外部程序不经 Shell 调用，并限制超时、输出与临时目录。
- **可观测与扩展**：提供异步有界日志、`/health`、`/metrics`、诊断接口，以及静态插件和稳定 C ABI 动态插件运行时。
- **工程验证**：覆盖 Windows/Linux Debug/Release、GitHub Actions、ASan/UBSan、CTest 连续 50 次、HTTP/Job 压力与真实 AI 延迟；12 小时双业务 soak 共 720 个作业全部成功。

**技术栈：** C++17 · Linux epoll/timerfd/signalfd · HTTP/1.1 · CMake · GoogleTest/CTest · Python Benchmark · WebGL2 · TensorRT/WeldAgent Adapter

[查看框架、Web UI 与性能基准 →](https://github.com/realme-max/IndustrialAIServiceFramework)

## 能力主线

- **工业三维视觉**：点云预处理、语义分割、几何特征提取与结果验证。
- **模型工程化部署**：PyTorch 模型导出、ONNX 图检查、TensorRT 插件与推理基线验证。
- **高性能 C++ 服务**：Linux Reactor、非阻塞 TCP、HTTP/1.1、定时器、线程池、有界队列与背压。
- **工业 AI 应用平台**：Artifact 管理、异步 Job、进程隔离、算法 Adapter、浏览器上传与 WebGL2 可视化。
- **机器人焊接与 Agent**：焊前建系、焊接特征计算、状态图编排、证据优先评审与安全边界。
- **工程质量与性能**：跨平台构建、CI、Sanitizer、重复回归、压力/性能基准与长时间稳定性验证。

> 三个仓库职责清晰：PTV2-WeldSeg-Deployment 聚焦焊后点云分割与推理部署，weld_agent 聚焦焊前建系、焊接特征与可信任务编排，IndustrialAIServiceFramework 负责以统一 HTTP/Web 运行平台承载两条相互独立的业务链路。
