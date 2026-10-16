# B2 · 云厂自研 ASIC 层

> 研究线：B（芯片行业）子课题 2 ｜ 母课题：`00-课题地图与研究路径.md`
> 上游依据：`B1-商用GPU.md`（份额口径）、`B3-上游制造与封装.md`（HBM/CoWoS）、`B4-电力与散热.md`（每 GW 的电力侧）
> 完成时间：2026-09-18 ｜ 检索 11 次，全部命中
> **分析单位提醒**：本层按"商业模式"和"负载类型"分析，不按公司排名。

---

## 0. 本层要回答的问题

`B1` 留下一个未解释的现象：**出货量口径 GPU 58% / ASIC 42%（2026），但收入口径 NVIDIA 仍占 75–81%**。Morgan Stanley 归因于"商户 GPU ASP 比 ASIC 高 3–7 倍"，并估两者 2025→2028E CAGR 为 28%（$712B）vs 69%（$269B）。

**因此本层的核心命题是：这个 3–7 倍 ASP 溢价能否维持。**

若它破了，整条 B 线的份额预测都要重算。

**结论先行（分两个口径，答案相反）：**

| 口径 | 3–7 倍溢价是否成立 | 判断 |
|---|---|---|
| **自用（captive）** | ✅ 成立且会长期成立 | ASIC 的成本优势归自用者所有，**根本不进入加速器市场收入统计** |
| **对外销售（merchant）** | ❌ 基本不成立 | Google 卖 TPU 系统定价 **$35B/GW**，NVIDIA 每 GW 收入 **$40B/GW**——**只差 12.5%** |

> **这直接回答了 B1 的分叉：units 与 revenue 两条曲线的分叉不是过渡现象，是永久性的结构。** 因为绝大多数的 ASIC 永远不会被"卖"，它们只会被"用"。分母里没有它们。

---

## 1. 六条 ASIC 线的成熟度对照

### 1.1 总表

| 项目 | 代次 | 状态 | 是否对外销售 | 训练 or 推理 | 代工/设计伙伴 |
|---|---|---|---|---|---|
| **Google TPU** | v7 Ironwood（GA 2026-04）→ v8t/v8i（2026-04 预览） | **最成熟** | ✅ **两条腿都走**：GCP 租赁 + 整系统直销 | **训推一体** | Broadcom（主，协议至 2031）+ Marvell（推理）+ 联发科（8i）+ AMD（第十代之一，传） |
| **AWS Trainium** | T2（售罄）→ T3（2026 初）→ T4（2027/28） | **成熟** | ❌ **截至 2026-07 未售一颗裸芯片** | 训推一体 | Marvell（T2 chiplet）+ Alchip（T3/T4 后端，争议中）+ Annapurna |
| **Microsoft Maia** | 100（有限）→ 200（2026-01 量产）→ **300（2026-10-12 发布，2027 起数 GW 部署）** | **落后但在加速** | ❌ 仅内部 Azure，2026-08 无外部 GA | **仅推理** | Marvell |
| **Meta MTIA** | 4 代路线图：**450/Arke 2027H1 投产、500/Astrid 2027 年底进数据中心**（2026-10-12 披露） | **2026-09 量产** | ❌ 纯自用 | **仅推理 / 排序推荐** | Broadcom（含业界首款 2nm AI 加速器）+ TSMC chiplet |
| **OpenAI Jalapeño** | 第一代（2026-06 发布） | **2026 底小批量** | ❌ 纯自用 | **仅推理** | Broadcom（硅实现/网络）+ Celestica（板卡机架） |
| **Anthropic（未命名）** | 早期规划 | **未确定，可能放弃**（2026-10 与三星深度谈判：架构定义 + 2nm + HBM 集成） | — | 拟聚焦推理 | 三星 2nm + 先进封装（洽谈中） |

> 来源：【官方】Broadcom Q3 FY2026 财报电话会（Nasdaq 记录）、Microsoft/Google 官方公告；【一手三方】SemiAnalysis、The Information、Reuters、TechCrunch；【产业媒体】TechTimes、钛媒体、EE Times。

### 1.2 关键读数

**Google TPU v7 Ironwood**（【官方】Google Cloud Next，2026-04 GA）
- 单芯片 4.6 PFLOPS FP8、192GB HBM3e、7.4 TB/s 带宽
- SuperPod 9,216 颗聚合 42.5 EFLOPS
- 互联带宽 9.6 Tbps（ICI）**低于** NVIDIA NVLink 14.4 Tbps——但集群层面靠规模取胜
- 单芯片功耗接近 1 kW，迫使 Google 全面转向高压直流 + 直接液冷

**AWS Trainium3 UltraServer**
- 144 芯片/机架，约 362 MXFP8 PFLOPS ≈ Blackwell NVL72 的 ~360 PFLOPS（【官方】AWS）
- 比 T2 提升 30–40% 性价比；每兆瓦输出 token 提升 5 倍以上
- **1.4M 颗 Trainium 已部署**，其中 Project Rainier 含 50 万颗 T2
- 客户：Anthropic（>100 万颗 T2、承诺 5GW）、OpenAI（约 2GW，2027 起）、苹果（测试 T2）、Meta（Graviton）

**Microsoft Maia 200**（【官方】Microsoft，2026-01-26）
- TSMC 3nm、>1400 亿晶体管、216GB HBM3E、7 TB/s、**272MB 片上 SRAM（超过 Blackwell 的 192MB）**
- >10 PFLOPS FP4、~5 PFLOPS FP8、750W TDP
- 部署于 Iowa、Arizona，**仅内部**；服务 GPT-5.2 与 M365 Copilot 推理
- 官方声称比上一代机队 +30% 性能/美元（Nadella，FY26 Q4 电话会）

**Microsoft Maia 300**（【官方】Microsoft，2026-10-12 OCP Global Summit）
- **288 GB HBM4、18 TB/s**（带宽为 Maia 200 的 2.5×、容量 +33%、每瓦 HBM 带宽约 2×）；台积电 **2nm 多芯粒**；**96 颗/机架**（Maia 200 的两倍），机架级 Token 吞吐最高 **7×**
- **2027 年起数 GW 级部署，使 Maia 成为微软产品与服务的主要推理系统**；Maia 200 已投产（Copilot + OpenAI 模型 + MAI 模型），GitHub Copilot 与 Microsoft Foundry 年底前接入
- **官方卖点即结算框架公式三**："最大限度提高**每瓦电力、每美元投入所能产生的有效 Token 数量**"；自报每兆瓦 Token 比领先 GPU 系统 **+40%**（厂商口径、无测试条件，按 §5.3 纪律只并列不换算）
- Evercore ISI：2027Q1 Maia 约占微软 AI 芯片支出 **10%**
- **对本层的意义**：§2.2"验证深度"这条结构性原因被部分反驳——第三代直接上 2nm + 数 GW 部署；但"仅内部、不外卖"的模式判断不变（Moor Insights：尚无微软以外的企业测试过这些芯片）

**OpenAI Jalapeño**（【官方】Hot Chips 2026-08 + SemiAnalysis InferenceX）
- TSMC 3nm、reticle-size die、6 个 HBM4 堆栈 / 216 GiB / 15.4 TB/s、700W TDP（实测 ≤550W）
- InferenceX 实测：比 GB200/GB300 **高 1.5–1.9× 吞吐/千瓦**；端到端延迟低 1.7–3.6×
- **低延迟工作点宣称 8.6–104.3× 吞吐/千瓦**——这个数量级过于夸张，SemiAnalysis 自承测试条件对 Jalapeño 有利，**标为待验证**
- **未与 Vera Rubin 对比**
- 9 个月 RTL→tapeout，**用 OpenAI 自己的 AI 模型辅助设计**（→ 与 `A2-进步机制换引擎.md` 的 RSI 闭环咬合）
- 2027 Anthropic 部署 1.3GW；Broadcom 称 2028 有 >5GW 可见度

---

## 2. 为什么只有 Google 把芯片卖了出去（本层第一问）

接续卡的原问题是"为什么 Google 能对外卖 TPU 而 Microsoft 的 Maia 卖不动"。**这个提法本身要修正**——因为不止 Microsoft 卖不动，**AWS 也卖不动**（截至 2026-07 未售一颗 Trainium 裸芯片）。

**正确的问法是：为什么只有 Google 选择了卖。** 三个结构性原因：

### 2.1 负载覆盖：训推一体 vs 只做推理

- **TPU 能训练 frontier 模型**。Anthropic 用它训练 Claude，Google 用它训练 Gemini。
- **Maia 200 是 inference-first**："基本不关心双精度浮点性能"，砍掉训练不需要的功能，把晶体管预算让给低精度吞吐（【产业媒体】多家一致）。
- **只做推理的芯片，只能卖给"已经训好模型的客户"**——可寻址市场窄一个数量级。这是 Maia 卖不动的**技术原因**。
- Meta MTIA / OpenAI Jalapeño 同理，都只有推理。

### 2.2 验证深度：十年 vs 第三代

- TPU 已在 Google 内部跑了十年（Search / Ads / YouTube / Gemini），是**被自身最苛刻业务验证过的硅**。
- Maia 是**第三代才刚起步**：Maia 100 有限出货 → Maia 200 延期至 2026-01 量产 → Maia 300 传闻 2026-09。**（2026-10-15 更新：Maia 300 已于 10-12 发布，2nm + 数 GW 部署——"第三代拐点"被事实跳过，Wedbush 的批评仅对前两代成立。）**
- Wedbush 分析师 Matt Bryson 直言不讳（【产业媒体】LongPort 引述）：
  > "迄今为止，微软的 Maia 自研 AI 芯片项目一直未能达到预期，而且总体而言，**微软过去在硬件领域的执行力一直有所欠缺**。"
  > ——但他同时指出芯片开发的收益通常在**第三代（历时 4–5 年）**才开始兑现。

### 2.3 商业模式的根本差异：芯片是武器还是产品

这是我认为**最根本的一条**：

| | 自研芯片的角色 | 证据 |
|---|---|---|
| **AWS** | **云毛利率的武器** | $25B 年化收入**全部来自 AWS 算力账单**；AWS 营业利润率 **39.4% 创历史新高**（+650 bps）；Jassy 称自研芯片每年节省数百亿美元 capex |
| **Microsoft** | 同上 | Maia 200 只用于降低 Azure 推理账单；有测算称若在 50% 内部推理负载替代 NVIDIA（按 30% 节省），年省 **$30–45 亿**（【待验证】公众号测算，未获官方确认） |
| **Google** | **两条腿都要** | 既出租（GCP）**也直销整系统**（$35B/GW 总额法确认） |

**为什么 Google 是唯一一个？**

> **因为 AWS 和 Azure 不需要靠卖芯片赚钱，它们有企业云的分销霸权。Google Cloud 没有。**
> AWS 手握 **$4,960 亿 backlog**、39.4% 营业利润率——它的自研芯片只要能让自家云更赚钱就够了。
> Google Cloud 的情况不同：TPU 是它**唯一能差异化的资产**。TPU 系统外售把 GCP 增速从核心业务的 70% 出头拉到 **82%**（2026 Q2），并预计把 2027 增速拉到 **100%+**（vs 卖方一致预期 64%）。

**这一条的推论很重要**：Google 卖 TPU 不是因为"ASIC 好卖"，而是因为 **Google 在云市场的相对弱势，逼它把芯片从成本中心转成营收引擎**。其他家不做这件事，是因为它们有更好的变现路径。

---

## 3. 核心命题验证：3–7 倍 ASP 溢价（本层第二问，也是最重要的一节）

### 3.1 三个互相冲突的"每 GW 成本/售价"

| 数字 | 口径 | 来源 | 等级 |
|---|---|---|---|
| **$6–11 B/GW** | Broadcom/Marvell 设计的 **ASIC 机架** | Milk Road AI 估算（经 Edgen 转述） | 【待验证】二手估算 |
| **~$19 B/GW** | NVIDIA **GB300 机架** | 同上 | 【待验证】二手估算 |
| **~$25 B/GW** | NVIDIA **Vera Rubin 机架** | 同上 | 【待验证】二手估算 |
| **$35 B/GW** | **Google TPU 系统对外销售价**（总额法确认收入） | SemiAnalysis Tokenomics Model | 【一手三方】自建模型，非 Google 指引 |
| **$40 B/GW** | **NVIDIA 每 GW 收入**（Hopper $18B → Blackwell $25B → Rubin $40B） | `B1-商用GPU.md` 已整理 | 【二手】产业链测算 |

**这三个数无法共存于同一口径。** 我的判断（**未证实，列入待验证**）：

- **$6–11B/GW** 大概率是"芯片 + 机架 BOM/制造成本"层级
- **$25B/GW（Rubin）** 是"买方机架采购成本"
- **$35B/GW（TPU 系统）** 是"整系统售价"——含机架、网络、液冷、以及 **SPV 结构与融资成本**
- **$40B/GW（NVIDIA）** 是"NVIDIA 从每 GW 中获得的收入"——含网络、Grace CPU、软件

### 3.2 这个冲突推出本层最重要的一个判断

**Google 把 TPU 当产品卖时，定价锚定的是 NVIDIA 的价格，不是自己的成本。**

$35B/GW（TPU 售价）vs $40B/GW（NVIDIA 收入）——**只差 12.5%，远不是 3–7 倍。**

> **换句话说：ASIC 的成本优势被"自用者"拿走了；一旦变成对外销售，价差被卖方捕获为毛利。**
> Google 的 TPU 系统销售 EBIT 利润率"30% 出头"，**低于核心云业务**，但仍远高于卖芯片该有的水平。

### 3.3 所以命题的答案是分叉的

**在自用口径：3–7 倍溢价成立，而且会长期成立。**
理由不是 ASIC 造得便宜，而是——**ASIC 自用的部分根本不进入"加速器市场收入"这个统计口径**。
- AWS $25B 自研芯片收入 = 算力账单，不是芯片销售额
- Microsoft Maia = Azure 内部成本项
- Meta MTIA、OpenAI Jalapeño = 纯自用，零外部收入

**在对外销售口径：溢价基本消失。** Google 是唯一的大规模卖家，而它的定价贴着 NVIDIA 走。

**→ 结论：units 与 revenue 的分叉是永久结构，不是过渡。** 整条 B 线用"收入份额"讨论 ASIC 替代 GPU，从方法上就低估了 ASIC。

### 3.4 一个必须并列的反平衡

NVIDIA 并没有在输：【一手三方】**The Information 估 NVIDIA 近年市场份额反而上升至 74%**。且 NVIDIA 已通知大客户 **2027 年初出货的服务器涨价 >15%**（Vera Rubin / Grace Blackwell 平台）——**它仍有定价权**。
同时 NVIDIA 已设立内部 ASIC 部门进入定制硅设计领域，并于 2026-03 向 Marvell 投资 **$20 亿**。

---

## 4. Broadcom 与 Marvell：谁真正吃到这部分利润（本层第三问）

### 4.1 量级差约 20 倍

| | Broadcom | Marvell |
|---|---|---|
| 最新季度 AI/定制收入 | **AI 半导体 $16.7B**（Q3 FY2026，+221% YoY，+54% QoQ），占总营收 56% | 定制硅年化 **~$1.5B**（FY2026，约占总营收 18%） |
| 指引 | FY2026 AI **$58B**（+186%）→ FY2027 **~$115B** → FY2028 **~$230B**；两年合计交付 **~$3,500 亿** | FY2027 +20%，FY2028 翻倍至 ~$55 亿（Jefferies），FY2029 目标 **$100 亿** |
| 定制硅 co-design 份额 | **~70%**（另有 80–85% 口径；Counterpoint 估 2027 约 60%） | **~20%**（另有 10–12% 口径；Counterpoint 估 2027 约 25%） |
| 旗舰客户 | Google TPU（协议至 2031）、Meta MTIA（至 2029）、OpenAI Jalapeño、Anthropic | AWS Trainium、**Microsoft Maia**、Meta DPU |
| 网络 | Tomahawk 6（几乎覆盖所有 XPU 客户）、Tomahawk Ultra、1.6T CPO | 光互连 FY2027 +70%；收购 Celestial AI（光子互连）、XConn（$5.4 亿）、Polariton |
| 利润率 | Non-GAAP 营业利润率 **66%**，FCF 利润率 46% | 营业利润率 35.3% |

> 来源：【官方】Broadcom Q3 FY2026 财报新闻稿（broadcom.com/company/news/financial-releases/64671）+ 电话会；【一手三方】Counterpoint、Jefferies、UBS、RBC；【产业媒体】多家。

### 4.2 XPU 出货量：同比 +3.5 倍

Broadcom 官方披露：**XPU 出货量同比增长超 3.5 倍，占 Q3 AI 收入的 73%**；AI 网络收入同比 +2.5 倍以上。

**六大定制 XPU 客户**驱动（Broadcom 口径）。供给瓶颈官方点名：**数据中心土地/电力/厂房（LPS）、HBM、基板**——与 `B3`（HBM/CoWoS）和 `B4`（电力）完全咬合。

### 4.3 Marvell 的 Google 认股权证：是期权不是订单

【一手三方】2026-08-17 签署，8-19 披露：
- 向 Google 发行认股权证，最多 **58,970,907 股 @ $206.58**，头条价值 **$121.8 亿**
- 结构拆分：**仅约 136 万股按时间归属**（首年四季等额解锁，价值约 $2.81 亿）；**5,760 万股与业绩挂钩**——按 240 批解锁，每批对应 Marvell 从 FY2027 Q3 起至 FY2033 末**每确认 $5 亿合格定制产品收入**
- **全归属需 Google 在约七年内带来 ~$1,200 亿定制硅收入**；全归属将稀释现有股东 **6–7%**

> **这是一个门槛，不是承诺。** Google 的采购具有自主裁量权。认股权证本质是**利益绑定机制**，而非有保障的收入。
> 这与 `B1` 里 AMD 给 OpenAI 的认股权证是同一类工具——**本轮反复出现的"股权换采购"结构，值得单列为一条风险线。**

### 4.4 Marvell 的真实战略价值：不是第二供应商，是 NVIDIA 的桥头堡

- NVIDIA 2026-03 投资 Marvell **$20 亿**，通过 **NVLink Fusion** 押注横跨 GPU 与 ASIC 双生态
- 黄仁勋公开称 Marvell 可能成为 **$1 万亿市值**公司
- Morningstar 给 Marvell 的公允价值 **$130/股**，评级"窄护城河 + 高不确定性"，而股价交易在 **$240 中低区间**——**市场定价已远超第三方公允价值**
- 风险：AWS 客户集中；Trainium3/4 设计权争议（SemiAnalysis 报道 + Benchmark 降级称部分转给 **Alchip**，后部分撤回；RBC 反驳称 Marvell 仍是 T4 关键供应商）——**未确认，列入待验证**

**判断**：Broadcom 吃到了绝大部分（量级差 20 倍），但 Marvell 的价值在于**它同时被 NVIDIA 和所有超大规模厂商需要**。这两者不矛盾。

### 4.5 ★ 反向修正（2026-10-12 由 `B5-互连与光.md` 追加）：本层叙事的一部分已被 2026 年的事实证伪

**B2 的整条叙事是"云厂自研 ASIC = 摆脱对 NVIDIA 的依赖"。B5 从互连层带来的证据指向一个更尴尬的事实：**

| 事实 | 出处 |
|---|---|
| **AWS Trainium4 通过 UCIe 桥接芯片组接入 NVLink 6**（最多 72 颗全互连 @3.6 TB/s） | 【一手三方】 |
| **AWS 正把 Trainium4 集成进 NVIDIA 的 MGX 机架架构** | 【一手三方·NVIDIA】 |
| **NVLink Fusion 要求每个部署至少包含一件 NVIDIA 产品**（GPU/CPU/交换机/ConnectX NIC），且第三方 CPU 与第三方加速器**二选一** | 【一手三方·DigiTimes / ServeTheHome / IPNET 三家一致】 |
| **NVIDIA 网络 attach rate 接近 90%，且"即使部分部署使用自研 AI 芯片的客户，通常仍在使用 NVIDIA 的网络技术"** | 【官方·CFO Colette Kress，CES 2026】 |
| NVIDIA 另投 **MediaTek $35 亿可转债**、**Lumentum + Coherent 各 $20 亿** | 【一手三方】/【官方】 |

> **→ 修正后的表述：这不是"ASIC 替代 GPU"，而是"ASIC 在 NVIDIA 的机架里做增量"。**
> **→ 但 B2 §3 的核心结论不受影响**：Trainium4 **依然是自用的**，它只是自用在 NVIDIA 的机架里——**"自用口径的 ASP 溢价永久成立"依然成立**。**两条结论必须同时陈述，不能只说其中一条。**
> **→ 对 B2 §2「为什么只有 Google 卖芯片」的补充**：Google 之所以是唯一能外售的，可能还多了一个 B5 给的理由——**只有 Google 有 OCS（网络层）+ Raiden（传输层）+ TPU（芯片层）的完整自有栈**。AWS 在芯片层做到了，但在网络层与传输层仍在用 NVIDIA 的（MGX 机架 + NVLINK + NIXL 生态）。**这可能是"为什么 AWS 不卖 Trainium 裸芯片"的深层原因之一：你卖的不是一颗芯片，是一整套栈。**

---

## 5. "ASIC 便宜 30–50% TCO" 在哪些负载成立（本层第四问）

### 5.1 成立的

| 场景 | 证据 | 等级 |
|---|---|---|
| 成熟模型高并发推理 | Ironwood @ 100 tok/s/user：$0.181/百万 token vs B200 $0.222、B300 $0.276（**低 19% / 34%**） | 【一手三方】SemiAnalysis InferenceX，2026-09-07 |
| 同上，20 tok/s/user | Ironwood 原始吞吐 +5%，配更低小时成本 → **每美元多 50.4% token（vs B200）/ 96%（vs B300）** | 同上 |
| 图像生成推理 | **Midjourney 从 GPU 迁 TPU，月成本 $210 万 → $70 万，降 65%** | 【产业媒体】多源一致 |
| 固定训练任务 | Trainium2 比同类 GPU 性价比高 **30%**（官方），T3 再 +30–40% | 【官方】Amazon 股东信 |
| 推荐/排序 | Meta MTIA、快手凌川 SL200（覆盖快手 99.7% 直播转码） | 【官方】Meta / 【产业媒体】 |

### 5.2 不成立或显著削弱的

1. **前沿训练——没有任何定制 ASIC 取代 NVIDIA。**
   唯一例外是 Google TPU 和 Anthropic 用 Trainium，但那是**垂直整合者特例**，不是可复制的商品路径。有分析指出"Google TPU 花了 5 年才让 JAX 可训练大模型"（【待验证】CSDN 深度文）。

2. **低精度**：Ironwood **无原生 FP4 支持**，上述优势只在对比 NVIDIA 的 **FP8 模式**时成立。TPU **v8i** 才是补齐 FP4 的一代。

3. **解聚服务（disaggregated serving）**：**NVIDIA GB300 在中段延迟曲线上反而领先 Ironwood 约 30%**。Google 内部跑解聚多年，但**对外 TPU 软件尚未优化**。

4. **软件摩擦**：TPU 走 **TorchAX** 翻译层（PyTorch → JAX），增加摩擦；新框架 **TorchTPU 已于 2026-10 开源**。vLLM / SGLang 默认 NVIDIA，新开放权重模型 day-zero 支持。
   > SemiAnalysis 的判断我认同并引用：**"10 月的 TorchTPU 发布，才是 Ironwood 能否成为真正替代方案（而不是没人能部署的便宜选项）的真正考验。"**
   > **（2026-10-15 更新：考验已到来，第一轮答卷正面）**【官方·Google Developers Blog】：TorchTPU 是**原生 PyTorch 后端**（走 PyTorch PrivateUse1，无子类无包装；Debug / Strict / **Fused** 三种 eager 模式，Fused 比 Strict **+50~100%+**），静态路径走 torch.compile → Torch Dynamo → **XLA + StableHLO**；支持 DDP/FSDPv2/DTensor 与 **MPMD**（PyTorch/XLA 时代只支持纯 SPMD）；自定义 kernel 支持 Pallas/JAX（Helion 在路上）。**2026 roadmap**：公开 GitHub repo、Helion 集成、动态 shape、多队列、**vLLM 与 TorchTitan 深度集成、Pod 级线性扩展验证**。⚠ 但"无原生 FP4"这条不变（v8i 才补），TorchTPU 不改变本节第 2 条的判定。

5. **NRE 门槛**：单代先进制程芯片的设计+验证+软件适配通常需 **$1–2 亿**。前提是**单客户、大规模、稳定工作负载**；创业公司做不起。

6. **架构变化风险**：模型架构剧变时，专用设计无法快速适应（Maia 200 分析中被明确点出）。

### 5.3 一个流传很广但我标为待验证的成本对照表

| 项目 | 定制 ASIC | 同代 GPU | 差距 |
|---|---|---|---|
| 芯片采购成本 | ~$13,000 | ~$28,000 | 54% |
| 功耗/芯片 | ~0.65 kW | ~1.30 kW | 50% |
| 1 GW 建设成本 | $60–110 亿 | $190–250 亿 | 2–3× |
| 每 PFLOPS-年成本 | $3,732 | $6,341 | 41% |

> 来源：CSDN 深度文，**【待验证】**——无方法披露，且"1GW 建设成本 $190–250 亿"与第 3 节的 $25B/GW（Rubin 机架）在同一量级但与 $19B/GW（GB300）冲突。**未找到权威机构复核版本，不引用其结论，仅记录存在此说法。**

---

## 6. 份额预测的时间线（三个机构跨度一年，全部并列）

| 机构 | 交叉点判断 | 附加判断 |
|---|---|---|
| **JPMorgan** | **2027** 出货量交叉（ASIC 53%） | — |
| **Omdia**（2026-08-28 最新） | **2028** ASIC 出货 **1,050 万颗** > GPU **1,010 万颗**，首次超过 | **收入层面 GPU 领先到 2032**，差距自 2029 起收窄 |
| **Bernstein** | 最保守：ASIC 份额 10%（2025）→ 20%（2027） | — |
| **TrendForce** | ASIC 服务器占 AI 服务器出货 **27.8%（2026）→ 39.5%（2030）** | 定制 AI 芯片销售额 2026 增速 **+45%** vs GPU 出货 **+16%** |
| **DIGITIMES** | 高端 AI ASIC 出货 2025 **513 万颗** → 2026 **723 万颗**（+40%） | — |

> **三者相差整整一年，且口径不同（出货量 vs 收入 vs 服务器占比），混用即错。** 沿用 `B1` 的结论：**必须先问"分母是什么"。**

---

## 7. 中国侧：内向型自研（与 B7 的接口）

| 厂商 | 产品 | 进展 | 对外输出 |
|---|---|---|---|
| **字节跳动** | SeedChip（NPU） | 2026-03 工程样片，年内 **10 万颗**，远期 35 万颗 | ❌ **纯自用，不对外** |
| **阿里平头哥** | 真武 810E（96GB HBM2e，对标 H20）/ **M890**（2026-05，性能 +3×，144GB，800GB/s，128 卡磐久超节点） | 截至 2026-05 **累计出货 56 万片** | ✅ **60%+ 服务外部客户**，覆盖 20+ 行业 400+ 客户；**高端款仅整机销售，不单卖裸片** |
| **百度昆仑芯** | P800（7nm，345 TFLOPS FP16）/ M100（对标 H20，2026 内）/ M300（2027 初） | 多个万卡集群交付 | ✅ **外部业务比重已超过向百度内部供货**；已启动科创板上市辅导 |
| **腾讯** | 紫霄 1.0 / 2.0（与燧原联合开发） | 2026 新增自研占比目标 **30%**，年底 **10 万卡**；2025 向燧原采购 8.3 亿元 | — |
| **快手（凌川）** | SL200 视频 SoC | 近十万颗 | ✅ 部署至快手/阿里云/百度云/B站 |

**结构性差异（我认为是本层对 B7 最重要的输入）：**

> **中国的自研 ASIC 是"内向"的**——每一家都在为自己的业务场景造芯片，**几乎没有对外输出的平台化能力**。
> 而美国正在发生相反的事：**Google 把 TPU 卖给它的直接竞争对手 Anthropic**（2026 Q3–2027 Q4 期间 >20% 的 TPU 出货直接售给 Anthropic）。

**另一条值得记的**：字节正在开发与上海昕原合作的**引入 RRAM 阻变存储以绕开 HBM 瓶颈**的新一代推理芯片——这是对 `B3` 那条"HBM 是硬上限"的直接回应（华为用 LPDDR 替代 HBM3E 是另一条同构路径）。

---

## 8. 跨线接口

| 接口 | 内容 |
|---|---|
| **→ `A2-进步机制换引擎.md`** | OpenAI Jalapeño **用自家 AI 模型辅助设计**，9 个月完成 RTL→tapeout。这是 A 线"RSI 闭环"在硬件侧的第一个实物证据 |
| **→ `B3-上游制造与封装.md`** | Broadcom 官方点名供给瓶颈为 **HBM + 基板 + LPS**；三星拿下 Anthropic 2nm（Taylor 厂 2027 量产，台积电 2nm 已排至 2028–2029）——**代工竞争出现实质分流** |
| **→ `B4-电力与散热.md`** | ASIC 1GW 建设成本与电力设备交期共同决定"每 GW 打包值"；Anthropic 承诺 5GW TPU（2027）+ 10GW（2028）超过多数单一国家的增量 |
| **→ `B7-国产替代.md`** | 核心命题：中国自研 ASIC 的"内向性"是否会被打破；阿里真武 60% 外供是当前唯一反例 |
| **→ `B1-商用GPU.md`** | **修订**：B1 的 $40B/GW 与本层 $35B/GW（TPU 售价）、$25B/GW（Rubin 机架）构成三口径冲突，**不可直接比较** |

---

## 9. 尚未解决 / 待验证清单

| # | 疑点 | 为什么重要 | 优先级 |
|---|---|---|---|
| **Q1** | **$6–11B / $25B / $35B / $40B 每 GW 四个数的口径定义** | 直接决定"3–7 倍 ASP 溢价"命题成立与否。目前互相矛盾且全部是二手或自建模型 | **最高** |
| **Q2** | SemiAnalysis 的 TPU backlog **$1,500 亿**、2027 TPU 销售 **$1,200 亿**、2027 GCP 增速 **100%+** | 全部是 SemiAnalysis **自建模型**，非 Google 管理层指引。中文媒体已当作事实广泛转述 | **最高** |
| **Q3** | **Compute SPV 的 $350 亿买 1GW** 的结构与会计处理 | 总额法确认会显著放大收入；SPV 关联方关系未披露 | 高 |
| **Q4** | Marvell 在 **Trainium3/4 的实际设计份额**（Alchip 分流程度） | Amazon 与 Marvell 均未确认；Benchmark 已部分撤回降级 | 高 |
| **Q5** | **Maia 200 部署量 >10 万枚**（称来自 SEC 文件） | 我找到的来源是中国公众号转述，未见 SEC 原件；若为真是判断 Microsoft 进度的关键 | 高 |
| **Q6** | Trainium3 **TCO 低 50%**（Oplexa 估算） | 无方法披露，且是机架级而非实测 | 中 |
| **Q7** | Ironwood 对 B200/B300 的优势 | SemiAnalysis 自承是**建模估算**而非实测，且不含 FP4、不含解聚服务 | 中 |
| **Q8** | **Anthropic 自研芯片是否落地** | 官方口径"仍可能放弃"；三星合作未经 Anthropic 正面确认 | 中 |
| **Q9** | Omdia 提到"至少三个未公开身份项目"（一为 SoftBank、一或与 Anthropic 相关） | 未披露身份，影响 2028 份额预测 | 中 |
| **Q10** | Jalapeño 低延迟点 **8.6–104.3× 吞吐/千瓦** | 数量级异常，需独立复核 | 中 |
| **Q11** | Broadcom FY2028 $230B AI 收入中，有多少已被合约锁定 vs 意向 | 官方仅称"有 line of sight to supply" | 低 |

---

## 10. 本轮检索路径（可复现）

| # | 查询 | 命中 | 收获 |
|---|---|---|---|
| 1 | `Google TPU Ironwood external sales Anthropic 1 million TPU deal 2026 TPU revenue` | ✅ | SemiAnalysis InferenceX 首个第三方实测；Anthropic 100 万颗拆分（40 万买 + 60 万租） |
| 2 | `AWS Trainium3 Project Rainier 500000 chips 2026 Inferentia deployment revenue` | ✅ | **$25B run rate 全部来自云账单、未售一颗裸芯片**（关键反转） |
| 3 | `Microsoft Maia 200 Azure 2026 deployment Fairwater why not sold externally status` | ✅ | 仅内部、仅推理、Wedbush 批评 + 第三代拐点论 |
| 4 | `Broadcom AI revenue 2026 Q3 custom accelerator XPU OpenAI 10 gigawatt Marvell custom ASIC earnings` | ✅ | 官方财报原件：AI $16.7B、XPU +3.5×、FY2028 $230B |
| 5 | `Meta MTIA 2026 custom accelerator deployment Broadcom production shipment generation` | ✅ | Iris 2026-09 量产、只做排序推荐、四代路线 |
| 6 | `OpenAI Jalapeno Broadcom 10 gigawatt custom chip first generation inference 2027 deployment` | ✅ | Hot Chips + InferenceX 实测；3nm+HBM4+700W；9 个月流片 |
| 7 | `Marvell 2026 custom ASIC revenue data center earnings Microsoft Maia Trainium XPU share versus Broadcom` | ✅ | **Google 认股权证结构拆解**；份额 70/20；Trainium 设计权争议 |
| 8 | `merchant GPU versus custom ASIC average selling price premium 3-7x Morgan Stanley 2028 accelerator market` | ✅ | **每 GW 三口径冲突**（本节最核心发现）；Omdia 2028 交叉 |
| 9 | `Google Cloud TPU external sales revenue Q2 2026 backlog Anthropic Meta direct chip sales billion` | ✅ | **$35B/GW 总额法、$12 亿/Q2、$1500 亿 backlog** |
| 10 | `custom ASIC TCO 30-50% cheaper claim which workloads training inference not suitable limitations` | ✅ | 适用边界四条；TorchTPU 10 月开源是关键变量 |
| 11 | `阿里 真武 字节跳动 自研AI芯片 ASIC 2026 百度昆仑 腾讯 进展 出货` | ✅ | 中国侧五家对照；字节 RRAM 绕 HBM |
| 12 | `Anthropic custom chip Samsung 2nm SoftBank undisclosed ASIC project 2026 Omdia three secret programs` | ✅ | Anthropic 官宣组团队；三星 2nm 分流台积电 |

**11 次检索，全部命中，零空查询。**
**方法论验证**：本轮继续沿用"用具体产物名检索"的纪律（Ironwood / Jalapeño / Maia 200 / MTIA Iris / Project Rainier / TorchTPU / InferenceX），**没有一条是抽象术语**。这条纪律在本轮再次被验证有效。

---

## 11. 给后续子课题的跟踪清单

1. ~~**TorchTPU 开源**~~ ✅ **2026-10 已开源**（Google 官方博客 + 2026 路线图）——"便宜选项 → 可部署替代方案"的第一关已过；下一关：**vLLM/SGLang 集成与 Pod 级线性扩展是否兑现**（roadmap 承诺，待验证）
2. **Helios 与 Vera Rubin 的首份独立 MLPerf**（`B1` 遗留，当前均缺）
3. **Marvell 认股权证首个归属季度**（FY2027 Q3 起，每 $5 亿收入触发一批）——判断 Google-Marrvell 合作是真需求还是宣传
4. ~~**Maia 300 是否如期发布**~~ ✅ **2026-10-12 已发布**（2nm、288 GB HBM4、18 TB/s、2027 数 GW 部署、自报每兆瓦 Token +40%）——第三代拐点验证通过；下一关：**2027 部署是否如期 + Evercore 的 10% 芯片支出占比是否兑现**
5. **Anthropic 自研芯片是否真正 tapeout**
6. **Q1 的四个每 GW 数字**——找到任一个的权威原始定义，就能判定 ASP 溢价命题
