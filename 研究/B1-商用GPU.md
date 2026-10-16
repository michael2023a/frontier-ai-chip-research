# B1 · 商用 GPU 格局（NVIDIA / AMD）

> 所属：**B 线（芯片）第 1 个子课题**。母纲 `B0-芯片行业总纲.md`，完整纪律见 `00-课题地图与研究路径.md` **第五节**（2026-10-12 重写后由第三节改为第五节）。**全课题骨架见该文件 §1 结算框架。**
> 完成时间：2026-09-18 ｜ 本轮检索：**10 次，全部命中** ｜ 上一手层级：`B3-上游制造与封装.md`（供给上限）、`B4-电力与散热.md`（接入上限）
> **本层要回答的三问**（来自 `B0-接续卡.md`）：
> 1. NVIDIA 份额 87%→75% 的斜率会不会加速？
> 2. AMD 的 14GW 承诺有多少能在 2027 前变成真实收入？认股权证与反向投资的会计准则影响要不要计入？
> 3. CUDA 护城河在哪些负载上仍不可绕过，哪些已可绕开？

---

## 〇、先说结论：三个问题里，有两个的提法本身需要修正

| 原问题 | 修正后 |
|---|---|
| 份额斜率会不会加速？ | **斜率取决于你选哪个分母**。2026 年三个口径给出的答案相差 30 个百分点（87.4% / 75–81% / 58%），且没有一个是错的。真正的答案是：**NVIDIA 的份额不是被抢走的，是自己供不上而让出去的**——管理层明说 FY2028 全程受供给约束、只能满足约 70% 需求。 |
| 14GW 能兑现多少？ | 问题应该改成：**每一 GW 到底值多少钱**。五个来源给出 $13.6B–$37B 的 2.7 倍差距。而且 AMD 自己也变成了出资方——它对 Anthropic 的 $5B 股权投资，让"收入"和"出资"落在同一笔下注上。 |
| CUDA 护城河在哪？ | **护城河已经不在软件里了**。它搬到了「**每 GW 能打包多少钱**」上：Hopper $18B/GW → Blackwell $25B/GW → Rubin $40B/GW。软件层的锁在快速松开（Triton 成为 PyTorch 默认路径），但整机柜层面的捆绑在加深。 |

**一句话**：这一年商用 GPU 层发生的事，不是"AMD 抢 NVIDIA 的份额"，而是**竞争单位从芯片搬到了机架，再从机架搬到了每 GW 的完整账单**。谁能在一个 GW 里塞进更多自有产品，谁就能在份额下降的同时收入上升。

---

## 一、规模对照：先把口径错开，否则后面全是假结论

### 1.1 最新一季度（两家都是 2026 年 6/7 月结）

| | NVIDIA Q2 FY2027（截至 2026-07-26） | AMD Q2 2026（截至 2026-06-27） |
|---|---|---|
| 总营收 | **$96.22B**（+106% YoY） | **$11.536B**（+50% YoY） |
| 数据中心 | **$89.02B**（+117% YoY，+18% QoQ） | **$6.72B**（+107% YoY） |
| DC 占总营收 | **92.5%** | **58%** |
| Non-GAAP 毛利率 | **75%**（+250bp YoY，环比持平） | **56%**（GAAP 54%） |
| 下季指引 | **$108B ±2%**（+89% YoY） | **~$13B ±$0.3B**（+41% YoY） |
| 全年增长指引 | FY2028 约 **+70%** | 2027 数据中心 $>80\%$ CAGR（自设目标） |

**【官方】** AMD 2026-08-04 财报新闻稿 / 10-Q（SEC）｜NVIDIA Q2 FY2027 财报（经 Nasdaq、腾讯新闻、247wallst 三方转述一致）

> **口径陷阱（必须先看这一条）**：AMD 的 "Data Center" 里混着 **EPYC 服务器 CPU**（本季 clients 业务的 CPU 是主要增量之一），NVIDIA 的 DC 里 92.5% 几乎全是 AI 基础设施。所以"NVIDIA DC 是 AMD 的 13 倍"这句话成立，但**"AI 加速器收入是 13 倍"不成立**——真实倍率更高。Citi 拆解 AMD 2026 DC 收入约 $33.7B 中，AI GPU 约 **$14.5B**、服务器 CPU 约 **$18.8B**。

### 1.2 三个常被混淆的"份额"，它们是三件事

| 口径 | 数字 | 来源与说明 |
|---|---|---|
| **A. SEC 三厂商收入口径** | NVIDIA **87.4%**（AMD 6.7%、Intel 5.9%） | Axis Intelligence 依 Q1 2026 三家 SEC 文件算：三家 DC 合计 $86.075B。**分母只有三家**。且 AMD/Intel 的 DC 含 CPU → **这个 87.4% 高估了 NVIDIA 的真实地位，同时高估了对手** |
| **B. 全 AI 加速器收入口径（含云厂 ASIC）** | NVIDIA **75–81%**，AMD **5–7%** | IDC / Bloomberg Intelligence / TrendForce / Silicon Analysts 均落在此区间。峰值 ~87%（2024）→ 2026 约 75–81% |
| **C. 出货量口径（颗）** | 2026：GPU **58%**、ASIC **42%**（16.3M 颗中 GPU 9.5M / ASIC 6.8M）<br>2027：GPU **47%**、ASIC **53%** | JPMorgan（摩根大通）2026 年预测 |

> ⚠️ **【待验证】** B 口径的 75–81% 全部来自二手估算。Axis Intelligence 自己也在同一页承认：第三方 McKenzie 都**不披露完整可复核方法**。没有任何一家给出过可 audit 的计算过程。

---

## 二、问题一：份额斜率会不会加速？

### 2.1 最重要的一条：这不是「被抢走」，是「供不上」

NVIDIA Q2 FY2027 电话会原文级别的信息（多家独立转述一致）：

- 管理层称 FY2028 展望**受供给约束而非需求约束**（supply-constrained, not demand-constrained），且这一状态持续到 **FY2028 年底**。
- 预计**只能满足约 70% 的需求**，多个细分领域需求增速接近 100%。
- 供给与产能承诺（Supply and capacity commitments）在一个季度内从 **$119B 暴增至 $279B**，主因是存储采购。
- 库存增至 **$31.58B**，为 Vera Rubin 备货。

**【二手三方】** Nasdaq 财报解读、TipRanks 电话会纪要、InvestorStack（三者转述一致，属同源）

> **这条改变了整道题的性质。** 当你的对手能卖多少取决于它能买到多少 CoWoS 和 HBM，而不是取决于它的产品竞争力时，**份额下降 = 分配优先级的选择，不是竞争失败**。`B3` 已经给了佐证：NVIDIA 的 CoWoS 份额 2026 约 56–63% → 2027 约 **45%**，但**绝对量 780 → 1,222 千片（+57%）**。是被增量稀释，不是被抢走。

### 2.2 但斜率确实在三个方向上加速 —— 只是不在同一个时间轴上

| 时间 | 会先松动的部分 | 判据 |
|---|---|---|
| **2026–2027** | **出货量份额**（口径 C） | JPMorgan：ASIC 颗数 2027 首次超过 GPU（53% vs 47%）。Omdia 更保守，认为是 **2028**（10.5M ASIC vs 10.1M GPU） |
| **2029 之后** | **收入份额**（口径 B） | Omdia：**到 2032 年 GPU 收入仍领先**，但差距从 2029 开始收窄 |
| **不会** | NVIDIA 自身的收入绝对值 | Morgan Stanley：商户 GPU 收入 2025→2028E CAGR **28%**，达 $712B；ASIC 同 期 CAGR **69%** 达 $269B。**份额跌、金额涨** |

**← Omdia（2028）与 JPMorgan（2027）差一整年，我并列，不取一。** Bernstein 给的第三种口径更新**：ASIC 份额从 2025 的 10% 翻倍到 2027 的 20%**——比上面两家都保守。三个独立机构跨度 1 年、相差一倍。

**为什么收入能撑住**：商户 GPU 的 ASP 比 ASIC 高 **3–7 倍**（Morgan Stanley）。所以颗数被超越不代表钱被超越。

### 2.3 中国：别只读百分比，那里有一半是分母效应

这是本轮我认为**最容易被误读**的一组数字。

| FY | 中国区收入 | 占总营收 |
|---|---|---|
| FY2024 | $12.33B | 20.2% |
| FY2025 | **$25.048B** | 19.2% |
| FY2026 | $19.677B（同比 −21%） | **9.1%** |

同期公司总收入：$130.497B → $215.938B（**1.65 倍**）。

> **9.1% 里约有一半是分母变大造成的，不是中国收入塌了一半。** XenoSpectrum 明确指出了这一点；多数媒体只写"占比从 20% 掉到 9%"，隐含给读者"损失一半收入"的印象，实际是 −21%（$25.0B → $19.7B）。

其余硬事实：

- H20 对华收入从 **$4.6B 归零**；Q3 FY2027 指引**明确假设中国 DC compute 收入为零**；中国 Hopper 出货占 DC 收入 **<1%**。
- 会计上的荒诞对比：**$4.5B 减值 vs 之后实际确认 $60M 收入 → 75 倍差**（均出自 FY2026 10-K 同一份文件）。
- 2026-08 恢复少量 H200 出货（25% 关税、逐案审批）：20,000 颗 ≈ $600M 毛订单 ≈ $450M 净收入 ≈ **单季收入的 0.5%**；另有 $400M H200 存货减值。
- ** Bernstein：NVIDIA 中国 AI 芯片份额 40%（2025）→ 约 8%（2026 末），华为升至 ~50%。** 北京被传要求云厂 AI 加速器采购中 **≥30% 用国产**（字节/腾讯获 H200 个案豁免）。

> ⚠️ **冲突并列**：黄仁勋 2025-10 公开表述"我们从 95% 份额掉到 0%"。Bernstein 给的是 2026 末 **8%**。两个数字都可能是真的（一个是"先进训练芯片"口径，一个是"含许可产品的全部 AI 芯片"口径），但**我找不到来源明确各自口径**，列入待验证。

**对 Q1 的回答**：中国不是"缺口"，是**已经完成重定价的部分**。它已经进了基数、进了指引假设（零），所以**中国不会再让斜率加速**——能让它加速的只剩供给。

### 2.4 结论

> **收入份额：缓慢下降（87% → 75–81%），斜率受供给保护，不会崩。**
> **出货量份额：快速下降（58% → 47%），2027–2028 被 ASIC 超过是大概率。**
> **两者同时为真。** 任何把这两个数字混在一篇报道里比较的文章，得出的"份额"都是错的。

---

## 三、问题二：AMD 的 14GW 能兑现多少？

### 3.1 承诺清单（全部「up to」，全部有里程碑条件）

| 客户 | 容量 | 首批 | 芯片 | 附加股权安排 |
|---|---|---|---|---|
| OpenAI | 6 GW | **首个 1GW 于 2026 下半年开始部署**（MI450） | 多代 Instinct | **认股权证 1.6 亿股，行权价 $0.01**（≈10% 股本） |
| Meta | 6 GW | 首批 1GW 2026 下半年 | **定制版** MI450 + Helios | **认股权证 1.6 亿股，$0.01**，股价门槛最高 **$600** |
| Anthropic | 2 GW | **首个 1GW 2027 上半年** | MI450 系列 + Helios | AMD **反向投资最多 $50 亿** 的 Anthropic 股权 |
| Oracle | 50,000 颗 MI450（Q3 2026 起） | — | MI450 | — |
| Microsoft / HUMAIN / Humain 等 | 未披露具体容量 | — | Helios on Azure | — |

**【官方】** AMD 10-Q（截至 2026-06-27）原文写明 OpenAI 与 Meta 各「up to 6 gigawatts」；AMD 财报新闻稿确认 Anthropic 2GW 与 $5B 投资；Fierce Network 现场报道确认 Oracle 50,000 颗。

### 3.2 每 GW 值多少钱？—— 五个来源，2.7 倍差距

| 来源 | 每 GW 对应 AMD 收入 | 14GW 外推 |
|---|---|---|
| KeyBanc（John Vinh, 2026-07） | **$13.6B** | $190.4B |
| Citi（2026-07-28） | **$15B** | ~$210B |
| Bloomberg Intelligence | **$15–20B** | $210–280B |
| ai2.work 按机架价反推（naive） | **$37B**（$5.25M/机架 × ~7,100 机架） | $518B |
| **已披露的交易名义额** | OpenAI ~$90B + Meta ~$60B + Anthropic ~$27B ≈ **$177B** | — |

> **我倾向采信 $13.6–20B/GW 与实际交易名义额互相印证的那一段（$177–210B），并把 $37B/GW 判为方法错误。** 理由：GW 通常按**设施总功率**计，含制冷与配电 overhead，不等于 IT 负载；而且承诺跨多个芯片世代、五年期，单价不可能持平。这点 ai2.work 自己也承认其 naive 外推"是错的"。

> **另一个不可忽略的分流（Bloomberg Intelligence 指出）**：客户可能通过云服务商消费算力，而非直采硬件 —— 最终落到 AMD 硬件收入上的金额会低于按容量外推。**这和 `B4` 里"数据中心上架率"是同一个问题。**

### 3.3 时间表：绝大部分不在 2027

- **Helios 首批出货在 Q3 末才开始**，真正有意义的机架级收入**最早 Q4 2026**。
- **主体收入落在 2027–2028**。
- Anthropic 首个 1GW 明确 **2027 上半年**才开始。
- **所以：14GW 中能在 2027 年底前变成真实收入的，我估不到 1/4。** 这是外推，非披露数据，标为估算。

### 3.4 认股权证的会计影响 —— 10-Q 原文（硬事实）

> AMD 10-Q 原文：「As of June 27, 2026, **no warrant tranches had vested or become exercisable, and the warrants had no impact on our results** for the three and six months ended June 27, 2026.」

翻译：**截至 2026 年 6 月底，一档都没有归属，对损益表零影响。**

但要注意三层：

1. **现在没影响 ≠ 未来没影响。** 一旦按 GPU 采购里程碑 + 股价门槛（Meta 最高 $600）开始归属，这是一笔**非现金费用**，会砸 GAAP EPS —— 而非 GAAP EPS 通常会被公司加回去。
2. **它把客户变成了股东。** OpenAI 若全额行权 ≈ AMD 10% 股本。**逻辑闭环：OpenAI 买得越多 → AMD 股价越可能涨 → OpenAI 的权证越值钱 → 越有动力继续买。** 这是`〇`节说的".repository financing"。
3. **反向投资同理**：AMD 投 Anthropic 最多 $5B，Anthropic 用这笔钱的一部分买 AMD 硬件。**两家公司都能发布好消息，但不一定有一个外部美元真正进场。**

### 3.5 循环融资：这条已经有了官方定性

- **BIS（国际清算银行）2026 年报**把 AI 基建循环融资列为**全球金融稳定的三大风险之一**，与"AI capex 崩盘""主权债务脆弱性"并列。**这是本层能拿到的最高等级来源。**
- 规模：缠绕交易的估值区间 **$800B – $1.2T**（多方估算）。Bloomberg 图表（经 Michael Burry 转发）：约 **$46B 直接股权 + $879B 多年采购承诺**在 Microsoft / Oracle / Amazon / Google / Meta / OpenAI / Anthropic / xAI / CoreWeave / NVIDIA / AMD 之间流动。
- NVIDIA：对前沿实验室投资近 **$500 亿**；联合 BlackRock / Blackstone / KKR / Apollo / Brookfield / 高盛撬动第三方资金 $5,000 亿。
- **反向意见**：Meta 与 NVIDIA 均已公开否认"循环融资"这个标签是恰当的（黄仁勋称"ridiculous"）。**我并列双方说法，不下判断。**

### 3.6 三条真实的执行风险（不是需求风险）

1. **内部集群不足。** SemiAnalysis 给出的正面概率上修是**有条件**的 —— 条件是 AMD 解决自身测试集群短缺。这是"拿自己没测过的东西去卖 14GW"。
2. **量产时点争议未决。** SemiAnalysis（2026-02）称 MI455X 量产爬坡滑到 **Q2 2027**；AMD EVP Forrest Norrod 公开否认，称"完全不知这说法从哪来"、无显著散热问题，Lisa Su 在 Advancing AI 2026 重申 Q3 末出货、Q4 爬坡。**截至本文写作日（2026-09-18）双方立场都还挂着，看 Q3/Q4 实际出机数而非口头力争。**
3. **CoWoS 配额。** 摩根士丹利估算 NVIDIA 2026 单家消耗约 **595 千片** CoWoS。AMD 若被 `B3` 所述的设备 2 年交期锁死，14GW 就是"看得见吃不着的产能预约"。

附带信号：AMD Q2 资本开支 **$808M**（+186.5% YoY），接近市场共识 **3 倍**；财报超预期（Non-GAAP EPS $1.66 vs 预期），**股价仍盘后跌 8–9%** —— 市场的注意力已经从"是否超预期"切换到"资本回报率何时兑现"。

### 3.7 一个我发现的矛盾：AMD 并不是"便宜的第二货源"

| 层级 | AMD vs NVIDIA |
|---|---|
| **单芯片**：MI350 ~$25k vs B200 ~$30k | AMD **便宜 ~17%** ✅ |
| **整机柜**：Helios **$5.25M / 另一说 $5.5M** vs Vera Rubin NVL72 ~$3.75–3.93M | AMD **贵 ~40%** ❌ |

> 这两个数字同时是真的。**含义很重要**：AMD 的竞争叙事已经不是"更便宜",而是"每美元能买更多 token（自称 +30%）"。而那个 +30% 目前**只有模拟数据，没有第三方实测**（见 4.5）。**一旦实测出来说没那么多，AMD 在整机柜层面就没有任何价格理由了。**

---

## 四、问题三：CUDA 护城河 —— 哪里松了，哪里搬家了

这是本轮信息密度最高的部分，我分四层写。

### 4.1 已经松开的（这部分的锁在一年内基本拆完）

| 机制 | 证据 | 来源 |
|---|---|---|
| **Triton 成为 PyTorch 默认 kernel 生成路径** | 走 `torch.compile` 的 kernel 在 CUDA 与 ROCm 上无需改源码即可工作 → **大量移植成本直接蒸发** | 技术分析文（多处独立一致） |
| **vLLM 的 ROCm CI** | 测试通过率 **37% → 93%**（2026 上半年） | AMD 侧 + 技术分析文 |
| **推理服务路径已经生产验证** | Azure ND MI300X v5 GA（2024-05）、Oracle BM.GPU.MI300X.8（2024-09，MI355X 2026 跟进）、Meta 用 Grand Teton 跑 Llama 3/4 推理、**OpenAI 在其上跑 GPT 推理** | 同左 |
| **HPC 代码可自动移植** | hipify 把 CUDA 源码转 HIP；HPC 批量代码**开箱移植率 80–95%**（CORAL-2 官方数字） | 同左 |
| **开源社区补偿滞后从"年"缩短到"月"** | HipKittens（Stanford Hazy Research，2025-11）用 ~500 行达到手工调优 AITER 的前向 parity | 同左 |

### 4.2 没松开的（这才是真正的护城河）

| 缺口 | 具体 | 严重性 |
|---|---|---|
| **FlashAttention-4 完全没有 ROCm 移植** | FA4（Blackwell，2026-03 发布）**零 ROCm 版本**；FA3 仅部分支持（Dao-AILab 权威实现仍 CUDA-only）；FA2 才在 MI300X 上生产可用 | **研究型前沿代码每落后一代** |
| **架构专用原语无对等物** | 触及 Hopper/Blackwell 专用原语（TMA descriptors、wgmma、tcgen05.mma）的代码**无干净 ROCm 对应**，必须手写重写 | 上述 80–95% 自动移植率**对现代 AI kernel 不成立** |
| **无第一方 TensorRT-LLM 对应物** | AMD 的答案是外挂 vLLM + 自家算子库 AITER | 服务端最优化缺失 |
| **实测性能差距** | **Phoronix（2026-03）**：ROCm 7.2 在标准 PyTorch / vLLM / SGLang 负载上，同等精度同等硅，**比等价 CUDA 慢 10–25%** | 第三方实测 ⭐ |
| **组织层面** | NVIDIA **把工程师嵌进前沿实验室**；AMD **通过 GitHub 发 kernel** | 一字之差，商业后果差一个量级 |

> **这句话我认为是本层最好的一句总结**：**ROCm 7 达到的是 feature parity（功能对等），不是 perf parity（性能对等）。**

### 4.3 一条交叉验证：NVIDIA 自己也不再只靠 CUDA

- SemiAnalysis **独立验证**了 AMD 的 GEAK kernel agent：在 **MI355X** 上重写 MXFP8 decode-bound 稠密线性 kernel，端到端吞吐提升 **~21.8%**，方法论完整公开。
- **但**：grouped MoE GEMM 的改进卡在 **~1.1x 天花板**。

> 这条的含义是双向的：一方面 AMD 的**自动化 kernel 生成**被第三方证实有效（这是绕过 CUDA 的新路径）；另一方面它恰好在 MoE（当前旗舰模型的标配）上失效 —— **而 MoE 正是前沿模型的主战场**。

### 4.4 真正的护城河已经搬到哪里了：每 GW 打包值

这是我认为 Q3 的唯一正解。

| 平台 | NVIDIA 每 GW 可获取收入 |
|---|---|
| Hopper | **~$180 亿** |
| Blackwell | **~$250 亿** |
| Vera Rubin | **~$400 亿** |

**【官方-管理层口径】** NVIDIA Q2 FY2027 电话会

同期结构变化：

- **网络业务已成增长主引擎**：网络收入 $14.8B（**+199% YoY**），而 DC compute +77% → **网络增速是算力的 2.6 倍**。网络占 DC 收入从两年前的 ~12% 升至 **~20%**。
  - ⚠ **口径冲突（2026-10-12 由 `B5` 追加）**：SEC 年报口径为 **FY2025 11.3%（$129.90 亿 / DC $1,151.86 亿）→ FY2026 16.2%（$313.76 亿 / DC $1,937.37 亿）**，FY2026 网络同比 **+141.5%** vs 算力 +58.9%。**"~20%"与本行 SEC 的 16.2% 差异，可能来自季度 vs 财年、或是否含 LinkX 线缆的定义差异 → 列入待验证，两个数字并列不换算。**
  - **★ 补充（B5 本层第一发现）**：【官方·CFO Colette Kress，CES 2026】**网络 attach rate 接近 90%**（买 NVIDIA AI 系统的客户中近 90% 同时买网络产品），**且"即使部分部署使用自研 AI 芯片的客户，通常仍在使用 NVIDIA 的网络技术"。** → 这条比份额数字更能说明护城河的性质。
- **以太网侧也在赢**：IDC 数据，NVIDIA 数据中心以太网交换 Q1 2026 **+192.7%** 至 $21 亿，份额 **21.5%**。Spectrum-X 以太网收入同比 **2.6 倍**。
- **CPU 入场**：Grace CPU TTM 收入超过 **$50 亿**；Vera CPU 已全面量产（1.8× 基准、5× 带宽/瓦）；管理层认为服务器 CPU TAM 约 **$200 亿**，FY2028 CPU 收入预期翻倍以上。
- **推理专用硅并进**：Groq 3 LPX 全面量产，自称 **~4x tokens/s**（厂商自报，待第三方）；NVIDIA 部署指引中提到可用约 "25% LPU 容量"，所指对象待核 → 详见 `B6`。
- **生态侧**：管理层预期 NeoCloud 合作伙伴 2026 年末上线装机 **8GW**（2025 末 ~3GW）；并推出 **营收分成 + take-or-pay** 框架，把一次性硬件销售变成经常性收入。

> **结论：CUDA 之后，NVIDIA 的护城河变成了在一个 GW 里能塞进多少自有 SKU。** GPU + NVLink + Spectrum-X + BlueField + Grace/Vera CPU + Groq LPU，全部自有。AMD 正在照抄同一份作业（Helios 打包 EPYC + Instinct + Pensando + ROCm），但它有两处没抄到位：**没有 CPO 自研（依赖博通/外部硅）、没有私有 NVLink 等价物（押首批 UALink over Ethernet，2026 首次落地）**。这也是 `B5` 要解决的内容。

### 4.5 ⚠️ 必须打折的性能数字（全部为厂商自报 / 模拟）

| 主张 | 状态 |
|---|---|
| AMD：Helios 比 Vera Rubin NVL72 **每美元多 30% token** | **来自 AMD 模拟**，Fierce Network 明确标注"关键网络性能数据来自 AMD 模拟" |
| AMD：MI450 同机架功耗下 **性能高 10–15%**（Kimi K2 思考模型） | **「模拟测试」**（凤凰科技原文用词） |
| AMD：MI455X 跑 DeepSeek V4 Flash FP4，token 吞吐达 MI355X **34 倍**、单位成本降 **18 倍** | 厂商自报，无第三方 |
| NVIDIA：Vera Rubin 对比 GB300，**每兆瓦吞吐 30 倍、token 成本降 35 倍** | 厂商自报，无第三方 |
| NVIDIA：Groq 3 LPX **4x tokens/s** | 厂商自报 |

> **截至 2026-09-18，Helios 与 Vera Rubin 均未发表独立 MLPerf 结果。** 这是本层最大的信息黑洞。（gpuinsights.net 甚至给出了自己的 editorial 估算并明确声明"这不是厂商报价或已验证基准"—— 这种诚实应被保留。）

### 4.6 与 `B4` 交汇：表外风险

NVIDIA Q2 FY2027 财报里几个不属于"芯片"的数字：

- **土地 / 电力 / 机壳担保的最大总风险敞口 $1,085 亿**（含 2026 年 8 月签的 SB Energy 担保）
- 应收账款 $631 亿，**DSO 从 45 天升至 60 天**（大客户多季度协议延长账期）
- 发行 $250 亿高级无抵押票据 —— 尽管 FY2026 自由现金流有 $967 亿
- 供给与产能承诺 **$2,790 亿**（环比 $1,190 亿）

> 这意味着：**NVIDIA 正在把资产负债表本身变成竞争工具。** 它替客户担保土地和电力，等于在卖的不是 GPU，而是"通电 Guarantee 的机架"。**这与 `B4` 里"电力才能真正取决于……" 完全一致 —— 商用 GPU 层已经自己也下场做电力了。**

---

## 五、三张可复核表格

### 表 A · 规模（见 1.1，此处不重复）

### 表 B · 三口径份额对照

| 口径 | NVIDIA | AMD | ASIC 整体 | 分母定义 | 可靠度 |
|---|---|---|---|---|---|
| A. SEC 三厂商收入（Q1'26） | **87.4%** | 6.7% | 不含 | 三厂商 DC 收入（含对手 CPU） | 高（可复核）但**口径失真** |
| B. 全加速器收入（2026） | **75–81%** | 5–7% | ~10–20% | 含 TPU/Trainium 等（多数不披露） | 中（**二手估算，无一家披露方法**） |
| C. 出货量颗数 | **58%（2026）→ 47%（2027）** | 含在 GPU 内 | 42% → **53%** | 物理颗数 | 中（JPMorgan；Omdia 判 2028 交叉） |

### 表 C · 护城河迁移

| 时代 | 竞争单位 | NVIDIA 的不可复制处 |
|---|---|---|
| ~2023 | 单芯片 FLOPS | HBM 供给 + NVLink |
| 2024–2025 | 机架（NVL72） | NVLink + CUDA + 液冷工程质量 |
| **2026 起** | **每 GW 的完整账单** | **Hopper $18B → Rubin $40B/GW 的打包深度 + 表外担保能力 + 网络 20% 占比** |

---

## 六、冲突数据并列清单（我不做取舍）

| # | 冲突项 | 并列值 | 差异倍数 | 我的处理 |
|---|---|---|---|---|
| 1 | NVIDIA AI 加速器份额 | SEC 口径 **87.4%** vs 三方估算 **75–81%** | — | **两者都对，分母不同。** 混用即错 |
| 2 | ASIC 颗数超过 GPU 的年份 | JPMorgan **2027** vs Omdia **2028** vs Bernstein（只给份额 **20% 2027**） | 差 1 年 | 并列，等 2027 上半年实际数据 |
| 3 | AMD 每 GW 收入 | KeyBanc **$13.6B** / BI **$15–20B** / Citi **$15B** vs 机架价外推 **$37B** | **2.7 倍** | 采信低区间，$37B 判为方法错误（GW 含制冷/配电 overhead） |
| 4 | Helios 机架定价 | **$5.25M** vs **$5.5M** | 1.05 倍 | 并列；对"比 Rubin 贵 40%"结论无影响 |
| 5 | NVIDIA 中国份额 | 黄仁勋 **~0%** vs Bernstein **8%（2026末）** | — | 口径不明，**待验证** |
| 6 | AMD vs NVIDIA 价格关系 | 芯片级 AMD **便宜 17%** vs 机架级 AMD **贵 40%** | 方向相反 | **两条并列，这是最重要的一对矛盾** |
| 7 | 2027 AI 加速器 TAM | Bernstein **$500B** vs Morgan Stanley 路径（2030E **$980B**） | — | 年份不同（2027 vs 2030），非真实冲突 |
| 8 | ROCm vs CUDA 性能差 | Phoronix 实测 **慢 10–25%** vs AMD 自报 **+10–15% 性能优势** | 方向相反 | 前者是第三方实测、后者是厂商模拟，**采信前者** |

---

## 七、尚未解决 / 待验证（优先级排序）

1. **⭐Helios 与 Vera Rubin 的独立 MLPerf / 第三方实测** —— 2026-09-18 均无。这是判 Q1 与 Q3 的唯一硬开关。
2. **AMD MI455X 量产爬坡时点**：SemiAnalysis 说 Q2'27，AMD 说 Q3'26 末。**看 Q3 财报实际出机架数。**
3. **认股权证何时首次归属**：一旦归属，GAAP vs Non-GAAP EPS 会出现分裂。10-Q 是目前唯一权威来源，需逐季跟踪。
4. **AMD 从不单独披露 Instinct 收入** —— 所有 "AMD AI GPU 收入 $14.5B" 都是券商拆分（Citi），非公司口径。这是判断 AMD 真实进度的最大障碍。
5. **NVIDIA 供承诺 $2,790 亿中有多少可取消/可修改** —— 10-Q 原文说"在下 firm 订单前部分协议可修改或取消"，但**没给比例**。这是判断下行风险的 key。
6. **循环融资的净额到底是多少** —— $800B vs $1.2T 是两个不同估算口径，BIS 只定性未定量。**需要看是否有人做过去重后的净额。**
7. **Bernstein 的中国 8% 份额的口径**（是否含获许可的 H200），未确认。
8. **OMDIA / JPMorgan 的 ASIC 颗数统计是否已含国产（昇腾等）**，两者均未说明。

---

## 八、与其他层的交汇

| 交叉 | 内容 |
|---|---|
| → `B3` | NVIDIA CoWoS 份额 56–63%→45%（绝对量 +57%）；摩根士丹利估 NVIDIA 2026 用 595 千片 CoWoS。**这是 Q1 答案的物理上限** |
| → `B4` | NVIDIA 土地/电力担保敞口 **$1,085 亿**；AMD 12GW 承诺 vs 美国数据中心 2026 电网需求约 **75.8GW** —— **芯片造得比变电站快** |
| → `B5` | AMD 押首批 UALink over Ethernet + Pensando Vulcano；NVIDIA Spectrum-X CPO 已量产。**护城河之争的下一站** |
| → `B6` | Groq 3 LPX 已全面量产并入 NVIDIA 产品线；"25% LPU 容量"部署指引待验证 |
| → `B7` | Bernstein：NVIDIA 中国份额 40%→8%，华为→50%；北京 ≥30% 国产采购要求。**这解释了 Q1 中国部分的另一半** |
| → `A2` | **模型在实现 for hardware**：Kimi K3 自优化 GPU kernel（与 Fable 5 持平）、48 小时自研推理芯片原型；AMD 用 Claude 优化 ROCm。**软件护城河正在被模型自身侵蚀** |

---

## 九、本轮检索路径（可复现）

| # | 查询 | 结果 | 为什么这么搜 |
|---|---|---|---|
| 1 | `NVIDIA Rubin ramp schedule 2026 production volume shipment Vera Rubin NVL144 availability` | ✅ 命中 5 | 先建 Rubin 时间轴的骨架；拿到 CES/GTC/Q1 财报四段里程碑 |
| 2 | `AMD Q2 2026 earnings data center revenue MI450 MI400 OpenAI 6 gigawatt warrant accounting dilution` | ✅ 命中 5 | **最大收获**：拿到 AMD官方新闻稿 + SEC 10-Q 原文，「截至 6/27 无一档权证归属」是硬事实 |
| 3 | `Mercury Research GPU market share 2026 Q2 AMD Intel NVIDIA data center accelerator share` | ⚠️ 部分 | **跑偏但有收获**：Mercury 给的是 CPU/客户端 GPU（AMD 服务器 34.5%），不是 AI 加速器。**这条提醒我不能混用** |
| 4 | `NVIDIA data center AI accelerator revenue market share 2026 versus AMD second source estimate percent` | ✅ 命中 5 | **发现方法论冲突**：SEC 口径 87.4% vs 三方 75–81%。这是 Q1 的钥匙 |
| 5 | `NVIDIA China revenue 2026 export controls zero H20 sales share loss Datacenter China guidance` | ✅ 命中 5 | 找到 FY24/25/26 三年中国收入的**绝对数字**，从而算出分母效应 |
| 6 | `ROCm 2026 CUDA moat portability Triton PyTorch support AMD gap developer ecosystem survey` | ✅ 命中 5 | **信息密度最高**的一条；拿到 Phoronix 实测 10–25%、FA4 零移植、vLLM CI 37%→93% |
| 7 | `SemiAnalysis MI450 Helios benchmark versus Rubin independent test tokens per dollar inference 2026` | ✅ 命中 5 | 证实 "+30% tokens/$" 是**模拟**；同时拿到 SemiAnalysis 对 GEAK 的独立验证（21.8%） |
| 8 | `circular vendor financing 2026 AI infrastructure deals risk OpenAI AMD Anthropic equity investment Nvidia leases GPU-backed debt` | ✅ 命中 5 | **拿到最高等级来源**：BIS 2026 年报把循环融资列为全球金融稳定三大风险之一 |
| 9 | `NVIDIA networking revenue Q2 FY2027 growth 199% NVLink Spectrum-X full stack system lock-in rack scale moat` | ✅ 命中 5 | **本轮转折**：每 GW $18B→$40B、网络 199% vs 算力 77% → 护城河搬家的直接证据 |
| 10 | `AI accelerator market share forecast 2027 2028 merchant GPU vs custom ASIC trend inference training split` | ✅ 命中 5 | Omdia 2028 vs JPMorgan 2027 的**年度级冲突**；并发现 units/revenue 分野是全部争议的根源 |

**方法论小结**：本轮 10 条**全部命中**，无空查询。有效的原因是**全部用具体产物名与具体现象**（Vera Rubin NVL144 / MI450 Helios / ROCm 7.2 / Phoronix / Spectrum-X Photonics / circular vendor financing），没有构造抽象术语。这与 `00` 里记的教训一致。

---

## 十、来源清单（按分级）

**【官方 —— 财报/SEC/新闻稿/官网】**
- AMD Reports Second Quarter 2026 Financial Results（2026-08-04，PDF 新闻稿）https://d1io3yog0oux5.cloudfront.net/_42774036d5e0365d1af202a2d3bf7213/amd/news/2026-08-04_AMD_Reports_Second_Quarter_2026_Financial_1295.pdf
- AMD 官中财报稿 https://www.amd.com/zh-cn/newsroom/press-releases/amd-2q-2026-earnings.html
- AMD Form 10-Q（2026-06-27 期，经 StockTitan 转载）http://www.stocktitan.net/sec-filings/AMD/10-q-advanced-micro-devices-inc-quarterly-earnings-report-169b0abe8cb8.html
- NVIDIA FY2026 10-K（$4.5B 减值 / $60M 收入 / 中国区三年收入）
- NVIDIA Q2 FY2027 财报解读（Nasdaq）https://www.nasdaq.com/articles/nvidia-q2-earnings-beat-blackwell-ultra-data-center-strength

**【一手三方 —— 有方法论的调研机构/权威媒体】**
- Mercury Research（2026-08-20 数据，经 The Register 转载）— 注意：CPU 口径，非 AI 加速器
- IDC（NVIDIA 以太网交换份额 21.5%、+192.7%）
- Bloomberg Intelligence（每 GW $15–20B；MI355X 定位）
- JPMorgan / Morgan Stanley / Bernstein / KeyBanc / Citigroup / Omdia（均为二手转述，见下）
- Bank for International Settlements（BIS）2026 Annual Report — 循环融资列为三大风险
- SemiAnalysis（GEAK 21.8% 独立验证；MI455X 爬坡时点争议；CoWoS 595 千片）
- Phoronix（2026-03，ROCm 7.2 vs CUDA 实测）
- Dell'Oro Group（以太网 vs InfiniBand）
- Fierce Network（Advancing AI 2026 现场报道；**明确指出性能数据来自 AMD 模拟**）

**【产业媒体】**
- 腾讯新闻、凤凰网科技、21 世纪经济报道、雪球、今日头条产业号、XenoSpectrum、bit-tide / AI Compass、247wallst、Nasdaq、TipRanks

**【待验证 —— 聚合站/AI 内容站，数字仅供参考】**
- aiwiki.ai、gpuinsights.net（自承"editorial estimate"）、axis-intelligence.com（自承 SEC 口径失真）、ainvest.com、remio.ai、thedynamics.ai、metirai.com、blockflow.cc、aioapex.com、valueaddvc.com、techtimes.com、edgen.tech、investorstack.app、deadrisk.com、stocktitan.net 衍生数字

---

## 十一、建议持续跟踪的四个数字（滚动指标）

1. **Helios 与 Vera Rubin 的首份独立 MLPerf / 第三方实测**（当前均缺 → 一旦出现，Q1 与 Q3 的答案会同时落地）
2. **AMD 认股权证首次归属的季度**（届时 GAAP 与 Non-GAAP EPS 会分叉）
3. **NVIDIA 网络业务占 DC 收入的比例**（当前 ~20%，两年前 12%；超过 25% 说明护城河迁移已完成）
4. **FY2028 供给约束是否真的如管理层所说持续到年底**（措辞从 supply-constrained 变成 demand-constrained 即为拐点信号）（若 Q3/Q4 出现 "demand-constrained" 措辞，是拐点信号）
