# C4 · 每兆瓦 Token：从框架变量变成厂商口径

> 建立时间：2026-10-16 ｜ **C 线（跨线观察线 4）** ｜ 变量：**§1 结算框架变量 1（每兆瓦 token 数）**
> **研究标准**：`00-课题地图与研究路径.md` §5（唯一研究标准）。本文只收**可回溯到来源的口径**，每条事实一行：`数字 | 口径（测试条件）| 来源分级 | 链接`。
> **来源分级**：【官方】= 厂商新闻稿/官方博客/官网 spec 页/财报/系统卡/技术报告/keynote；【一手三方】= 独立基准方（SemiAnalysis、MLPerf、Epoch、IDC）；【产业媒体】= 转述，需交叉；【待验证】= 聚合站/个人博客/社区帖，未经一手确认。
> **总纪律**：本文所有数字**几乎全部是厂商自报或厂商供数**。**跨厂商数值不可换算，只并列**——这不是谨慎，这是本线的核心结论（§6）。

---

## 〇、怎么用这条线（先读这页）

**主问题**：当"每兆瓦能产出多少 token"从本课题的分析框架变量，变成厂商发布会上的头条规格之后——**哪些数字可比、哪些不可比、应该怎么并列、这对结算框架意味着什么。**

**分析单位 = 一条厂商自报口径**（四元组：**厂商 × 产品 × 指标定义 × 测试条件**）。不用"谁最强"框架（那是 A 线的公司单位），也不用产业层框架（那是 B 线）——同一家公司在这个单位下会因为**口径不同**分裂成多条记录（NVIDIA 一家就有 7 条不同分母的口径）。

**一句话结论**：

> **框架语言已经成为行业营销语言——四个互不通气的阵营（微软、NVIDIA、华为数字能源、中国信通院）在 2026 年同时把"每瓦/每兆瓦 token"当作核心叙事；但与此同时，没有任何两家厂商的这个数字可以直接比较。变量 1 第一次有了厂商填报，也第一次有了"跨厂商换算"这个新的造假空间。**

---

## 一、核心结论（五条）

1. **★ 框架语言已成行业营销语言（本线最强外部证据）**。2026-10-12 微软 Maia 300 官方头条 = "40% more tokens per megawatt"；NVIDIA 官方博客直接以 **tok/s/MW** 为叙事单位（"2.8M tokens/s/MW"）；华为数字能源把「让每一瓦特产出更多 Token」做成解决方案名；中国信通院把"词元经济"写进官方报告。**四个阵营、四种语言体系、同一个分母**——结算框架变量 1 不再是我们的分析工具，而是行业的公共度量诉求。
2. **★ 但自报口径全部不可跨厂商比较**。分母至少有四种：**封装 TDP**（Jalapeño 700 W）、**all-in 设施功耗**（InferenceX 的 B200 ≈2.17 kW/GPU）、**相似单用户延迟约束**（AWS）、**预测云租价**（AMD）。换分母直接改变结论：Jalapeño 的 700 W 与 1.18 kW 差 1.7×；GB300 的 1.4 kW 与 2.55 kW 差 1.8×。→ **立为第八类口径错误「自报 ≠ 可比」**（§6）。
3. **★ 唯一可复算的第三方基准只有一家**：SemiAnalysis **InferenceX / InferenceMAX**（all-in utility MW 分母 + 具名模型 + 公开可下载的 run 日志）。**MLPerf 功耗测量是可选层、近乎零提交**（v5.1 仅 2 个、v6.0 零个），J/token 指标还在社区讨论（issue #2558）。→ 变量 1 的度量生态是"**一家有数据、一家有审计地位但空转**"的双轨结构。
4. **★ 中国侧没有"每兆瓦 token"的官方指标，但话语已经落地**。政策能效分母是 **FLOPS**（433 号文"单位算力能效"= 总耗电量÷理论峰值 FLOPS，年度降幅 ≥5% 考核）与**元**（"单位能耗产出效益"），PUE 是设施层主指标（24 分权重）；GB 40879 修订将新增的"算力能效"仍是 FLOPS 口径。**唯一可推算 tokens/MW 的一手材料在华为**（Atlas 950 官方 19.6M tok/s 推理 + 100 kW/柜 → 推算约 1.53M tok/s/MW，假设见 §5.2）。
5. **★ 中国侧第一个候选口径错误实例已出现**：每百万 token 能耗 **0.1–0.3 kWh**（信通院敖立引述）vs **15–20 kWh**（电力机构，"高强度推理"口径）——**差 50–100 倍**，源于"是否高强度推理/是否含训练与基础设施"边界不清（§5.3）。

---

## 二、基准案例：微软 Maia 300（框架变量第一次被厂商主动填报）

### 2.1 官方逐字表述（三处同源）

| # | 原文 | 场合 | 来源分级 |
|---|---|---|---|
| M1 | "we expect Maia 300 to deliver **40% more tokens per megawatt** compared to leading GPU-based systems. And in a full 96-accelerator configuration, it delivers up to 7x the rack-scale token throughput of Maia 200." | Scott Guthrie，Official Microsoft Blog，2026-10-12 | 【官方】 [blogs.microsoft.com](https://blogs.microsoft.com/blog/2026/10/12/microsoft-to-expand-azure-maia-system-to-multi-gigawatt-deployments) |
| M2 | "deliver 40% more tokens per watt compared to leading GPU-based systems"（per watt 变体，与 per megawatt 数学等价） | Reynold D'Sa & Rani Borkar，Azure Blog（OCP），2026-10-13 | 【官方】 [azure.microsoft.com](https://azure.microsoft.com/en-us/blog/infrastructure-for-the-frontier-era-scaling-intelligence-through-open-industry-collaboration/) |
| M3 | "Maia 300 is designed to maximize **useful tokens per watt and useful tokens per dollar**" | Borkar & Dighe，Azure 架构深挖，2026-10-12 | 【官方】 [azure.microsoft.com](https://azure.microsoft.com/en-us/blog/deep-dive-into-azure-maia-300-architecture/) |

### 2.2 口径缺口（引用 M1 必须同时注明）

1. **对照组黑箱**：只说 "leading GPU-based systems"，未点名 NVIDIA 哪代（GB200/GB300？）、未给配置。
2. **测试条件黑箱**：未披露模型（仅泛述"长上下文推理、延迟敏感对话、多步智能体任务"）、未披露精度（硬件支持 MXFP4 ≠ 40% 用 MXFP4 测得）、未披露 batch 与延迟条件。
3. **时点**：M1 是 2027 年部署前的**预期值**（"we expect"），非实测。
4. **无第三方审计**：Moor Insights CEO Patrick Moorhead："目前还没有人在微软之外测试过它们"【产业媒体·CNBC 转述】。
5. **唯一有生产负载背景的数字是 Maia 200 的 1.7× tokens/W**（Teams/Copilot/Cowork 生产流量，跑 OpenAI + MAI + 开源模型）【官方】。

### 2.3 规格（写入口径换算用）

单芯片 288 GB HBM4 / 18 TB/s / 950 W TDP / 台积电 N2P / MXFP4+sub-3-bit；机架 96 加速器 / 167 kW envelope / 27.6 TB HBM；scale-up 域 128 机架 = 12,288 加速器 superpod；2027 起数 GW 部署【官方·Guthrie 博客】。单芯片 FP8 6.9 / FP4 13.8 PFLOPS 为【产业媒体】转述（官方页未直接核到）。

---

## 三、各家自报口径总表（按测试条件分栏，全部并列不换算）

### 3.1 美国云厂与 GPU 双雄

| 厂商 | 头条数字 | 层级 | 对照组 | 基准/测试条件 | 审计 |
|---|---|---|---|---|---|
| **微软** | tokens/MW **+40%** | 机架系统 | **未命名**"leading GPU systems" | **未披露**；预期值 | 无 |
| **NVIDIA（Blackwell）** | GB300 NVL72 = **2.8M tok/s/MW**；50× vs H200；$0.12/M tokens（35×） | 机架 | HGX H200 | DeepSeek-R1；**InferenceX v2**；官方明示 "Data is sourced from NVIDIA analysis and the SemiAnalysis InferenceX v2 benchmark" | 基准第三方、数据厂商供 |
| **NVIDIA（Vera Rubin）** | 吞吐/MW 最高 **30× GB300**；token 成本最高 **45× 更低**（官网仍写 35×，**版本冲突并列**） | 机架 | GB300 NVL72 | DeepSeek V4 Pro；**AgentX**（真实 agentic coding 回放）；"early results"，跑预发布 TRT-LLM | 同上 |
| **Google** | perf/W **2× Trillium**（Ironwood）；TPU 8t perf/$ **2.7×**、8i **+80%** | 芯片代际 | Trillium（v6e） | **从未发布 tokens/s 或 tokens/MW**（官方深挖全文确认） | 无 |
| **AWS** | Bedrock 上 Trainium3 **tokens/MW >5× Trn2**（at similar latency per user）；另处 perf/W **4×** vs 另处 **+40% 能效**（**官方内部冲突并列**） | 系统 | Trn2 UltraServer | 相似单用户延迟约束；模型未披露 | 无（"All figures were AWS's own"） |
| **AMD** | Helios **tokens/$ 最高 +30%** vs Vera Rubin NVL72；MI455X token 吞吐 34×（仅限高 interactivity 点） | 机架 | Vera Rubin NVL72 | Kimi K2 Thinking 32K/8K；**建模 + 预测小时租价**；**不公布 Helios 功耗 → 无法换算 tokens/MW** | 无 |
| **Meta** | MTIA 2i 生产模型 **TCO −44% vs GPU** | 芯片+系统 | **未披露型号 GPU** | 生产 R&R 模型，同质量同 SLO（ISCA '25 论文）；MTIA 300 后**只有定性词**（"parity at competitive TCO"） | 无 |
| **OpenAI** | Jalapeño 吞吐/kW **1.5–1.9×** vs GB200/GB300 | 芯片/双机架 ≈160 kW | GB200/GB300 NVL72 | **InferenceX 公开套件**（8k/1k 单轮）；**STP vs 对手 MTP 最佳配置**；OpenAI 执行、SemiAnalysis 观察 | 第三方现场运行、数据 OpenAI 供 |

**★ 口径披露完整度排序（C4 的方法论产出）**：
**OpenAI（模型/精度/单轮长度/并发扫描/功耗归一/预测解码开关六项全交代）> NVIDIA（具名基准+模型）> AWS/AMD（有约束条件但不绑模型）> 微软（全黑箱）**。OpenAI Jalapeño 可作为后续横评的**披露标准模板**。⚠ SemiAnalysis 原话：Jalapeño 对标 Blackwell "somewhat incomplete and unfair"（应对标同代 HBM4 的 Vera Rubin）。

### 3.2 推理专用硅（五家）

| 厂商 | 口径 | 测试条件 | 来源分级 |
|---|---|---|---|
| **Groq**（已分裂为 NVIDIA 侧 LPX + 公司侧历史口径） | Vera Rubin+LPX 组合 **35× TPS/MW** vs GB200 NVL72 | 2T MoE + 400K 上下文 + 400 TPS/user；"Projected performance"；无绝对功耗 | 【官方】[nvidia.com/lpx](https://www.nvidia.com/en-us/data-center/lpx/) |
| Groq 公司现行营销 | **零 perf/watt 数字**（官网首页只有 "premier neocloud for fast inference"） | 替代数字 = 融资与合作（$650M + $350M） | 【官方】[groq.com](https://groq.com) |
| **Cerebras** | CS-4 **10× throughput/W vs CS-3**；另称 30× vs GPU（GPT-OSS-120B 4,400+ TPS/user） | 只给相对倍数，无绝对功耗分母；CS-3 官方博客 ~27 kW vs 聚合站 23 kW（**差 17% 并列**） | 【官方】[cerebras.ai](https://www.cerebras.ai/press-release/cerebras-unveils-cs-4-up-to-30-times-faster-than-gpu-based-solutions-3347356) |
| **SambaNova** | "best tokens per watt" / "highest tokens per kilowatt-hour" | **只有定性排序，无绝对值、无测试条件**；SambaRack 平均 20 kW 风冷 | 【官方】[sambanova.ai](https://sambanova.ai/products/rdu-ai-chips) |
| **Etched** | **零披露确认**：官网无任何 perf/watt、tokens/s 绝对值；只有 "best-in-class throughput, latency, cost, and power efficiency" 定性语 | 替代数字 = $800M 融资、>$1B 已签合同、机架交付 | 【官方】[etched.com](https://www.etched.com/) |
| **Taalas** | ~**17,000 tok/s/user @ ~200–250 W**（HC1，Llama 3.1 8B 权重烧进 mask ROM） | 1K/1K 单用户口径；派生 tok/s/W 是"单用户速度÷单卡功耗"，**与聚合口径严禁对比**；Karl Freund 实测 14,357（低 16%） | 【厂商口径】+【一手三方】 |

**总判读**：五家推理专用硅中四家有官方能效口径，但**全部是相对倍数或单用户速度，没有一家给出"可复算的 tokens/watt 绝对值"**；Etched 确认零披露。

---

## 四、第三方测量：一家有数据，一家有审计但空转

### 4.1 SemiAnalysis InferenceX / InferenceMAX（唯一持续的 tok/s/MW + J/token 基准）

- **指标集**：Interactivity（tok/s/user）、throughput per chip、**throughput per MW**、P99 TTFT、**$/M tokens**、**J/token**（total/input/output 分相）【一手三方·[inferencemax.semianalysis.com/about](https://inferencemax.semianalysis.com/about)】
- **分母口径**：**all-in provisioned utility MW**（含 GPU、CPU、网络、设施开销），非芯片 TDP；例：B200 all-in ≈2.17 kW/GPU【一手三方】
- **测试条件**：固定 1k/1k、1k/8k、8k/1k + 并发扫描；AgentX = 长上下文多轮 agentic coding 回放；框架 SGLang/TRT-LLM/vLLM/Dynamo/Mooncake；每个数据点可回溯到公开 GitHub Actions run【一手三方】
- **关键实测序列**：B200 跑 DeepSeek V4，Day 0 ~300,000 → 2026-06-05 ~500,000 tok/s/MW（**~1.7× 纯软件增益，功耗分母不变**）——这是"软件改变每兆瓦 token"的直接测量证据，与 `AB` §6 的 21× MTP 案例同构
- **课题已引数字的原始出处在此**：GB200 $0.28 vs GB300 $0.12/M tokens（同 DeepSeek-V4-Pro、27 tok/s/user、Dynamo 解耦栈）；GB300 MTP 关 $2.35 vs 开 $0.11（21×）
- **AgentX 2026-09-14**：Rubin NVL72 vs GB300 NVL72（DeepSeek V4 Pro 1.6T）：100 tok/s/user 时 59.4M vs 28.5M tok/s/MW（~2.1×）；**生产工作点 60–100 tok/s/user 只有 1.4–3×**；170 tok/s/user 极端点 67× throughput/TCO-$。⚠ Rubin 数为 NVIDIA 测量、SemiAnalysis 复核中
- **生态背书**：Google Cloud、Azure、Oracle、Meta + vLLM/SGLang/PyTorch 支持；开源

### 4.2 MLPerf（有审计地位，功耗层近乎空转）

- 功耗测量 = **可选层（opt-in）**，墙插座 AC + 认证功率计，报告 J/query 或 perf/watt；**不含设施冷却**；Offline 与 Server 同硬件 perf/watt 可差很大【一手三方】
- v5.1（2025-09）：**仅 2 个功耗提交**；v6.0（2026-04）：公告列 24 个提交方、**无功耗结果**【一手三方/待验证交叉】
- **J/token 指标尚未进入正式规则**：MLCommons issue #2558 仍在讨论 `energy_per_token_joules`（仅生成段）+ `prefill_energy_joules` 分相
- v6.0 性能结果（audited）：GB300 vs GB200 per-GPU 吞吐 +45% Offline / +25% Server；AMD MI355X 87-GPU Llama 2 70B 首破 1M tok/s

### 4.3 其他独立口径（均为"每查询/每任务能耗"，非持续硬件基准）

- **Epoch AI**：GPT-4o 典型查询 ~0.3 Wh；100k 输入 ~40 Wh【一手三方】
- **Google 官方**（2025-08）：中位 Gemini prompt 0.24 Wh（全栈拆分）；仅加速器 0.10 Wh；fleet PUE 1.09【官方】
- **Microsoft Research Joule**（2026-04）：中位 0.31 Wh/聊天查询，推理式 ~3.91 Wh；**B200 matched latency 比 H100 中位省 35%**【一手三方】
- **ML.ENERGY Leaderboard**：H100/B200、vLLM、batch 扫描，32B 模型实测 0.15–0.31 J/token【一手三方】
- **SPECpower/Green500**：不测 transformer/AI 推理；**没有任何既有能效标准被 AI 推理采纳为结算口径**【一手三方】

---

## 五、中国侧：政策分母是 FLOPS 和元，不是 token

### 5.1 政策侧（433 号文《2026 年度国家绿色算力设施评价指标体系》附件 1，18 项 100 分制）

四大方面：能源利用水平（42 分，权重最大）/ 碳排放（20）/ 水资源（12）/ **算力资源利用水平（24）**。

| 与"每兆瓦 token"的距离 | 指标 | 口径 |
|---|---|---|
| 最近但仍是 FLOPS 口径 | **单位算力能效（6 分）** | 总耗电量 ÷ 理论峰值 FLOPS（智算按 FP16 稠密）；**年度降幅 ≥5% 得分** |
| 元口径 | **单位能耗产出效益（2 分）** | 年度效益（元）÷ 信息设备耗电量 |
| 设施层 | PUE（24 分，权重最大） | ≤1.10 满分；>1.30 不得分 |
| 变量 2（已由 `C2` 用） | 上架率（8 分）+ 负荷利用（4 分） | 60%→80% 阶梯；负荷率 30%→60% 阶梯 |

**→ 直接回答：18 项指标中没有任何一项以 token 产出为分母。** GB 40879 修订将新增的"算力能效"（单位电能算力产出）仍是 FLOPS 口径，且尚未正式发布【官方·编制说明】。

### 5.2 华为：唯一可推算 tokens/MW 的一手材料

| 数字 | 口径 | 来源分级 |
|---|---|---|
| Atlas 950 超节点（8192 卡满配）：**推理 19.6M tok/s（FP4）/ 训练 4.91M tok/s** | 官方 keynote 口径；未注明模型/批量/时延 | 【官方】[huawei.com](https://www.huawei.com/en/news/2025/9/hc-xu-keynote-speech) |
| Atlas 950 计算柜：64×950DT、114.1 PFLOPS mxFP4、**Power Usage ~100 kW** | 官方规格页（hiascend.com，整机级） | 【官方】 |
| **推算：≈1.53M tok/s/MW（推理）/ 0.38M（训练）** | 官方 token/s ÷（128 计算柜 ×100 kW ≈12.8 MW）；**供电≠功耗、未含 32 互联柜与冷却附加——勿当作官方数字引用** | 推算 |
| Atlas 350 标卡：1561 TFLOPS mxFP4、**最大功耗 ≤600 W** → 推算 2.60 TFLOPS/W | 板级 | 推算（官方数相除） |

**"每瓦 Token"的中国官方话语**（口号级，无数字）：华为数字能源「让每一瓦特产出更多 Token」为解决方案名（MWC26 起）；HC2026 英文官网 "maximize tokens per watt and minimize costs per token will be the core competitiveness of compute factories"（侯金龙）；信通院"词元经济"入官方报告（日均词元调用量两年超千倍）。**对照 Maia 300 的 +40%：中国侧采用这个指标是设施层口号，尚未落到芯片规格数字。**

**三家芯片官网现状**：华为有功耗无每瓦算力；平头哥官网连功耗都没有（400 W 出自央视画面）；寒武纪无官方数据表（Flopper.io 明记 "Cambricon has NOT published an official datasheet"）——**思元系列全部第三方口径互相矛盾（FP16 256 vs 345 vs 500 vs 700 TFLOPS；TDP 350–750 W），在公司发布官方数据表前不能进入任何每瓦性能比较。**

### 5.3 ★ token 能耗口径大冲突（中国侧第一个候选口径错误实例）

| 数字 | 口径 | 来源分级 |
|---|---|---|
| **0.1–0.3 kWh/百万 token** | 信通院副院长敖立引述的测算（"按生成 100 万 Token 耗电"） | 【官方·发言转述】 |
| **15–20 kWh/百万 token** | 国内电力机构，"以当前主流大模型在**高强度推理任务**下的表现测算"（新华社记者手记） | 【官方媒体】 |
| 15.2 万 tokens/度电（GLM-5.0）≈23.7 µWh/token | 个人实测（CSDN）：PUE=1.2、H800/H200/昇腾 910B、FP8/INT4、满载批处理、仅推理段；深度思维链模式降至 4–5 万/度电 | 【待验证】（条件透明） |
| 信通院《通信世界》2026-7：>1T 参数模型 >1 kWh/百万 token；100B–1T <0.5 | 首个中美六模型每百万 token 能耗对比；**只有英文转述，中文原文待补** | 【一手三方·转述】 |

**→ 差 50–100 倍。差异机理：是否"高强度推理"（CoT 长度/批大小/模型规模）、是否含 prefill/训练/基础设施。在边界界定之前，两个数字只能并列，不能平均、不能取其一。**

---

## 六、★ 立第八类口径错误：「自报 ≠ 可比」（每兆瓦 token 的跨厂商换算）

**定义**：同一指标（每兆瓦/每瓦 token、每百万 token 成本）在不同厂商自报口径下，**分母与测试条件各不相同，任何跨行倍数比较都不成立**。看到"X 比 Y 每瓦多产 40% token"这类数字，先过下面五问：

| # | 检查项 | 反例（本线实证） |
|---|---|---|
| 1 | **分母是什么**？封装 TDP / all-in 设施功耗 / 相似延迟约束 / 预测租价？ | Jalapeño 700 W（package）vs 1.18 kW（all-in）差 1.7×；GB300 1.4 kW vs 2.55 kW 差 1.8× |
| 2 | **对照组命名了吗**？ | 微软完全不命名 ＜ AMD/Meta（命名竞品但用自己的建模/未披露 GPU）＜ NVIDIA/OpenAI（命名竞品+具名基准） |
| 3 | **时点是预期还是实测**？ | Maia 40% = 2027 部署前预期（"we expect"）；Vera Rubin 30× = "early results"；Helios +30% = 建模 |
| 4 | **绑模型了吗**？精度？batch？延迟目标？ | NVIDIA/OpenAI 具名（DeepSeek-R1/V4 Pro、三模型）；微软/AMD/Meta 部分数字不绑模型 |
| 5 | **软件开关对等吗**？ | Jalapeño 全程 STP（无 MTP）vs NVIDIA 侧开 MTP 最佳配置——NVIDIA 自己承认"software alone doubled Blackwell performance" |

**与七类口径错误的谱系关系**：前七类都是"**把未发生/未生效的当成已发生/已生效**"；第八类不同——**数字都是真的，但比较是假的**。它不伪装状态，它伪装可比性。这是最难防的一类，因为每个数字单独看都无懈可击。

**→ 使用纪律（回写 `00` §5.3.1）**：任何 tokens/MW、perf/W、每百万 token 成本的跨厂商比较，默认不成立；必须并列 + 注明各自分母；唯一例外是同一第三方基准同一测试条件下的横评（目前只有 InferenceX 一家在做）。

---

## 七、口径冲突实例（同一指标、不同口径差 3 倍以上，全部并列）

1. **同一硅片同一模型，MTP 开关 = 21 倍成本差**（软件口径）：GB300 NVL72 跑 DeepSeek-R1、150 tok/s/user，MTP 关 $2.35 vs 开 $0.11/M tokens【一手三方·InferenceX v2】。→ **硬件代际差（GB200→GB300 的 −57%）小于软件开关差（21×）**——每兆瓦 token 首先是软件变量，其次才是硅变量。
2. **Groq 芯片级 vs 集群级 J/token（约 100 倍翻转）**：芯片级厂商口径 1–3 J/token vs GPU 10–30 J/token；但贾扬清测算 Llama-2 70B 需 305–572 张 LPU（vs 8×H100），同等吞吐下硬件成本 ~40×、能耗成本 ~10× H100。机理：LPU 仅 230MB SRAM/卡 → 大模型需数百卡组网，互连/调度/闲置功耗进分母后能效反转。→ **芯片级能效与集群级能效是两个指标，SRAM 容量直接决定卡数乘数。**
3. **NVIDIA 厂商口径 vs SemiAnalysis 实测（方向：厂商低报）**：黄仁勋 GTC 2026 称 Rubin 3× perf/W vs Blackwell；SemiAnalysis AgentX 实测生产工作点仅 1.4–3×，极端点 7×。同一基准内部，极端工作点与生产工作点本身差 20 倍以上。⚠ 这是本线唯一一例**厂商低报**（Groq/Cerebras/Taalas 均为高报方向）。
4. **AMD"便宜 40%"把方法论差异算进了产品差异**：MI355X GLM-5 $0.22 vs B200 $0.30/M tokens——Silicon Report 明确指出两数并非同一 matched-interactivity 方法论，不能直接对比。

---

## 八、尚未解决 / 待验证清单

1. **微软 40% 的对照组身份与测试条件**——等 Ignite 2026 或 2027 部署后实测。
2. **SemiAnalysis InferenceX 原始仪表盘完整数据未直接抓取**（tok/s/MW 与 J/token 按模型×框架×并发×交互性的完整表需登录拉取后回填）——这是变量 1 的"下一关"：**是否出现第三方按统一测试条件发布的跨厂商 tokens/MW 横评**。
3. **AMD Helios 机架功耗**（AMD 称仅对合作伙伴 NDA 披露）——缺此数 AMD 的 tokens/$ 无法换算成 tokens/MW。
4. **NVIDIA 官网 35× vs 博客 45×**（Vera Rubin token 成本）版本不一致；**AWS 4× perf/W vs +40% 能效**官方内部冲突。
5. **MTIA "0.12 J/token"** 源头 = ernie55ernie.github.io 个人博客（加密货币内容站），官方从未发布任何 J/token 数字，其余数字（1.2M 颗部署、跑 Llama 4-70B）均与官方矛盾——**【待验证】，建议从课题中删除**；"TCO −42%" 官方实为 **−44%**；"性价比 2.3×" 全部检索零命中（无源）。
6. **信通院《通信世界》2026 年第 7 期中文原文**（六模型每百万 token 能耗对比）未找到，目前仅英文转述。
7. **Atlas 950/960 超节点整机功率官方数字缺失**——本文件 1.53M tok/s/MW 为推算，等华为官方整机功率或 PUE 后重算。
8. **中国侧 50–100× token 能耗冲突的边界界定**（"高强度推理"的严格定义）未解。
9. **Etched 估值 $5B（2026-07，DCD 转述）vs B6 存档 $21B** 差 4 倍以上，待核 DCD 原文。
10. **待捕获事件**：**2026 年度国家绿色算力设施名单（报送截止 2026-11-16）**——若公示各设施自评价数据，将是中国首个官方"单位算力能效/单位能耗产出效益"横截面，直接决定 C4 中国线能否从"厂商口径"进入"政策口径"。

---

## 九、本轮检索路径（§5.5；三路并行，全部用具体产物名）

**总量**：约 45 次 WebSearch + 十余次 WebFetch 全文抓取，按三路推进。

**路 1 · 美国云厂与 NVIDIA**（17 次检索）：Maia 300 announcement/tokens per megawatt 官方原文 ✅；NVIDIA Blackwell 25x/15x/Vera Rubin per watt ✅（定位 InferenceX v2 / InferenceMAX v1 / AgentX 三个具名基准）；Google Ironwood/TPU 8t 8i perf per watt ✅；AWS Trainium3 trn3 UltraServer ✅；AMD MI355X/Helios tokens per dollar ✅；Meta MTIA J/token 溯源 ✅（官方空）。**空**：Maia 300 arXiv 论文（不存在，官方"technical paper"即 Azure 博客）；MTIA 0.12 J/2.3× 官方出处（从未发布）；Google tokens/MW（从未发布）；AMD Helios 功耗（NDA）。
**路 2 · 推理专用硅与第三方**（17 次检索）：Groq LPX/Cerebras CS-4/SambaNova SN50/Etched/Taalas HC1 ✅；MLPerf power 机制与提交数 ✅；InferenceX 指标集与测试条件 ✅；Epoch/SPECpower ✅。**空/被拦**："SemiAnalysis per megawatt economics" 直接查询被敏感审批拦截（换词后命中）；Epoch 无硬件级 tokens/watt 基准；SPECpower 无 AI 推理版本；Taalas 官网未直接抓取（收购过渡期）；Groq 2024 年 J/token 官网原文未找到（仅媒体转述）。
**路 3 · 中国侧**（中英双语 24 次）：中文命中官方原文——433 号文附件 1 全文（18 项指标口径与评分规则）、hiascend 规格页（Atlas 950/850E/350 功耗与算力）、华为数字能源官网×3（每瓦 Token 口号）、HC2026 官网、t-head.cn（无功耗栏）、工信部专家解读、GB 40879 编制说明。**中文空检索（有价值）**：「每瓦特算力」无已发布国家标准；「每兆瓦算力」未见于任何官方文件；字节/阿里/腾讯/智谱/DeepSeek/月之暗面官方能耗披露为零；三家芯片官网均无 perf/W 规格栏。**英文补充**：Flopper（Cambricon 无官方 datasheet 的方法论记录）、IEEE Spectrum、BigGo（信通院《通信世界》英文转述）。

---

## 十、给后续的接口与建议跟踪

**接口**：
- → `00` §1.5 纪律 2（来源分级）：第八类口径错误已回写 §5.3.1
- → `AB` §6（单位经济闭环）：Jalapeño 与 Maia 300 的每兆瓦口径在此会师；§6.2 MLPerf 的口径边界在此厘清
- → `B2`：Maia 300 的 40% 若在 2027 部署后被第三方复核，是该层最大单一验证点
- → `B4`：每兆瓦 token 的分母（all-in utility MW）与电力层直接同构；华为"让每一瓦特产出更多 Token"是 `B4` 议题的芯片侧话语
- → `C2`：433 号文 18 项指标中 #2/#3 是变量 1 的中国政策对应物（FLOPS/元口径），#11/#12 已由 C2 使用

**建议跟踪的六个数字**：
1. **微软 40% 的对照组与测试条件披露**（Ignite 2026 / 2027 部署实测）
2. **SemiAnalysis 统一条件下的跨厂商 tokens/MW 横评**（变量 1 的下一关；目前只有单芯片对测）
3. **MLPerf J/token 指标是否进入正式规则**（issue #2558）
4. **2026-11-16 绿色算力设施名单**（中国首个官方能效横截面）
5. **AMD Helios 功耗披露**（NDA 状态；缺此数 AMD 无法进入任何每兆瓦比较）
6. **NVIDIA 35×/45× 与 AWS 4×/+40% 的官方版本统一**

---

*文件完 · 2026-10-16 · 遵循 `00-课题地图与研究路径.md` §5 唯一研究标准 · 三路原料已综合进本文件（原始检索记录见 §9）*
