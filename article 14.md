# Multidimensional Scientometric indicators for the detection of emerging research topics (2021)

### 1. 研究目标 · 内容 · 问题 · 出发点
- **研究领域与背景、具体对象 / 数据集**
  - **研究领域**：科学计量学、技术预测与社会变迁、科技政策。
  - **研究背景**：及时准确地识别新兴研究主题（Emerging Research Topics, ERT）对于科研基金和政策制定者优化资源配置、促进有前景的研究至关重要。
  - **具体对象**：本研究以“干细胞研究”领域作为案例，展示其提出的ERT识别方法的有效性。
  - **数据集**：使用了Web of Science (WoS) 数据库中的422,101篇干细胞研究论文和PATSTAT数据库中的50,556项干细胞相关专利，时间跨度为2004年至2018年。

- **论文想解决的核心问题**
  1.  **定义模糊**：ERT的概念以及其与研究前沿（Frontier Topics, FT）、颠覆性主题（Transformative Topics, TT）等相关概念的界限模糊，缺乏一个清晰、可操作的定义。
  2.  **指标片面**：以往的ERT识别方法主要关注“新颖性”和“增长性”这两个属性，而忽略了同样重要的“潜在社会经济影响”和“不确定性”等维度。
  3.  **缺乏实用框架**：缺少一个能够将多维属性操作化、并在细粒度上识别ERT的系统性框架和流程。

- **研究动机 / 假设**
  - **研究动机**：为了更全面、准确地辅助科技决策，需要一个超越传统指标的、能够综合考量ERT多重特性的新方法。
  - **核心假设**：一个成功的ERT不仅应具备新颖和快速增长的特点，还应表现出持续性、连贯性，并具有潜在的高社会经济影响和随时间发展的“不确定性降低”趋势。一个综合衡量这五个维度的多维指标框架，可以更有效地识别出真正有价值的ERT。

- **工作内容概览**
  - **第一部分（理论背景）**：通过定性和定量方法（如出版物趋势、VOSviewer覆盖图分析）对ERT、FT、TT等概念进行辨析。回顾了现有的ERT识别方法，将其分为基于引文、基于词汇和混合方法三类。
  - **第二部分（方法论）**：构建了一个识别ERT的理论框架，该框架将ERT的特征定义为五个维度：根本新颖性、相对快速增长、持续性与连贯性、潜在高影响以及不确定性与模糊性降低。并为这五个维度设计了一套可操作的多维科学计量指标。
  - **第三部分（案例研究）**：将所提出的方法应用于干细胞研究领域。详细描述了从数据采集、主题发现、多轮筛选到最终识别出26个ERT的完整流程，并对结果进行了验证和分析。
  - **第四部分（结论与讨论）**：总结了研究的主要发现和贡献，分析了研究的局限性，并提出了未来研究方向。特别地，根据识别出的ERT的不同特征模式，提出了相应的七种研发（R&D）布局策略。

### 2. 研究方法（含模型 / 技术详解）
- **理论框架与算法**
  - **理论框架**：本研究的框架分为两步：1) **主题发现**：从大规模文献数据中识别出有意义的研究主题；2) **ERT识别**：利用一套多维指标体系，从发现的主题中筛选出符合ERT特征的主题。
  - **核心算法**：
    - **Leiden算法**：用于主题发现。这是一种先进的社区发现算法，应用于WoS数据库的直接引文网络。它能在保证社区内部良好连接性的前提下，将海量出版物划分到不同的层级（宏观、中观、微观）。本研究使用其“微观”层面的分类结果（4047个研究领域）来识别干细胞领域内的具体主题。

- **关键模型/技术逐一说明**
  本研究的核心在于一套多维度的ERT识别指标体系，分为“门槛指标”和“参考指标”。

  - **门槛指标 (Threshold Indicators)**
    - **(1) 新颖性与增长性 (Novelty and Growth)**
      - **架构**：通过计算四个子指标的平均增长率 (AGR) 来衡量一个主题的活跃度和新颖程度。这四个指标被视为识别ERT的基础门槛。
      - **输入**：特定主题在连续时间窗口内的论文数、期刊数、基金资助数、作者数。
      - **输出**：四个维度的AGR值。
      - **流程**：使用特定公式计算AGR，近期增长率被赋予更高权重。AGR持续为负或关键指标为空的主题被过滤掉。
      - **优势**：多角度衡量增长，比单一的论文增长率更稳健。
      - **公式**: 平均增长率 (AGR)
        $$V = \frac{1}{N_r} \sum \frac{N_{t} - N_{t-1}}{N_{t-1}}$$
        其中，$V$ 是AGR，$N_r$ 是计算的期数，$N_t$ 是在第 $t$ 个时间区间的实体数量（论文、期刊等）。

    - **(2) 持续性与连贯性 (Persistence and Coherence)**
      - **架构**：通过主题演化图和相似性计算来评估。一个主题若能在多个连续时间窗口内保持存在并增强其内在关联，则被认为具有持续性和连贯性。
      - **输入**：相邻时间窗口内，同一主题下的出版物集合。
      - **输出**：杰卡德相似度 (Jaccard Similarity) 分数和演化路径图。
      - **流程**：计算相邻时间窗口间主题的杰卡德相似度。相似度持续增加表明主题的连贯性在增强。
      - **优势**：量化了主题的稳定性和内在聚合度，过滤掉昙花一现的热点。
      - **公式**: 杰卡德相似度
        $$J(A, B) = \frac{|A \cap B|}{|A \cup B|}$$

  - **参考指标 (Reference Indicators)**
    - **(3) 潜在高影响 (Potential Prominent Impact)**
      - **架构**：通过分析科学（论文）与技术（专利）之间的相互引文关系，衡量主题的潜在社会经济价值。
      - **输入**：特定主题的论文集、专利集，以及它们之间的相互引文数据。
      - **输出**：两个关键指标 $U_{BA}$ 和 $U_{AB}$。
      - **流程**：计算从专利到论文的引文（代表基础科学向应用科学转化）和从论文到专利的引文（代表应用科学支撑基础科学）在主题总论文中的占比。
      - **优势**：超越了纯学术影响力的评估，直接关联到技术应用和经济潜力。
      - **公式**:
        - 基础科学到应用科学转化能力: $U_{BA} = \frac{\text{被专利引用的论文数}}{\text{主题总论文数}}$
        - 应用科学到基础科学转化能力: $U_{AB} = \frac{\text{引用了专利的论文数}}{\text{主题总论文数}}$

    - **(4) 不确定性与模糊性降低 (Reduction to Uncertainty and Ambiguity)**
      - **架构**：通过衡量一个主题对整个知识网络结构的影响力来评估其发展前景的确定性。一个能显著影响网络结构的主题，其未来发展路径更清晰，不确定性更低。
      - **输入**：特定领域内所有主题构成的引文网络。
      - **输出**：两个网络拓扑变化指标 $\Delta N_{SC}$ 和 $\Delta N_{WC}$。
      - **流程**：从整个知识网络中逐一移除某个候选ERT，然后计算网络的强连通分量 (Strongly Connected Components) 和弱连通分量 (Weakly Connected Components) 数量的变化率。
      - **优势**：为“不确定性”这一抽象概念提供了创新的量化方法，通过网络拓扑变化来预测主题的未来核心地位。
      - **指标**:
        - $\Delta N_{SC}$: 强连通分量数量的平均增长率。
        - $\Delta N_{WC}$: 弱连通分量数量的平均增长率。
        一个理想的ERT应能显著增加强连通性（成为知识核心），同时其不确定性会降低。

### 3. 实验设计与结果（含创新点验证）
- **实验流程**
  1.  **数据采集**：从WoS和PATSTAT数据库获取2004-2018年干细胞领域的论文和专利数据。将时间划分为11个重叠的5年期子区间（如2004-2008, 2005-2009...）。
  2.  **主题发现与初筛**：
      - 使用Leiden算法的微观分类，识别出干细胞领域的主题。
      - 在每个时间区间内，选取相关性最高的Top 50主题，合并后得到 **65个** 唯一的候选主题。
  3.  **多轮过滤**：
      - **持久性过滤**：移除在少于3个时间区间内出现的主题，剩余 **57个**。
      - **趋势过滤**：移除那些短暂增长后迅速衰退的主题，剩余 **54个**。
      - **增长性门槛过滤 (Step 1)**：
        - 计算所有主题在全部11个区间的四项AGR（论文、期刊、基金、作者）。移除在期刊、基金、作者上有空值的主题，剩余 **44个**。
        - 仅考察 **最近5个时间区间** 的论文AGR，移除近期呈负增长的主题。最终得到 **26个** 候选ERT。
  4.  **指标计算与分析**：
      - 对这26个ERT，计算它们的 **持续性与连贯性**（杰卡德相似度）、**潜在高影响**（$U_{BA}$ 和 $U_{AB}$）以及 **不确定性降低**（$\Delta N_{SC}$ 和 $\Delta N_{WC}$）指标。
  5.  **结果验证与解释**：
      - 将识别出的26个ERT（如“细胞外基质对干细胞命运的调控”、“脱细胞支架在组织再生医学中的应用”）与该领域的权威综述、年度科学突破新闻（如发表在 *Cell*, *Nature* 上的成果）进行对比。

- **数据集、参数、评价指标**
  - **数据集**：WoS干细胞论文 (422,101篇)，PATSTAT干细胞专利 (50,556项)。
  - **参数**：
    - 时间窗口：5年，滑动步长1年。
    - Top N主题：每个窗口选Top 50。
    - 持久性阈值：至少在3个窗口中出现。
    - 杰卡德相似度阈值（可视化用）：>= 0.04。
  - **评价指标**：
    - 内部指标：8个二级指标（4个AGR，$U_{BA}$, $U_{AB}$, $\Delta N_{WC}$, $\Delta N_{SC}$）。
    - 外部验证：与领域专家的定性证据（新闻、综述文章）进行比对，验证识别结果的有效性。

- **创新点如何得到验证，结果对比与可视化描述**
  - **创新点验证**：
    - **多维框架的有效性**：实验识别出的26个ERTs被重新打上语义化标签后（见附录表A5），与干细胞领域的重大进展新闻（见表10）高度吻合。例如，识别出的主题353（细胞外基质）和814（脊髓损伤）等，都在2018年有顶级期刊的突破性研究发表，证明了该方法的预测能力。
    - **指标独立性**：对8个二级指标进行皮尔逊相关性分析（见表9），发现它们之间大多不相关，这反过来证明了使用多维指标的必要性，因为每个指标都从不同角度捕捉了ERT的特征。
  - **结果对比**：本研究与Wang (2018)的研究不同，后者同时考虑所有科学领域，而本研究聚焦于特定领域以获取更深入信息。与Porter et al. (2019)也不同，本研究直接识别ERT而非依赖复杂的黑箱模型。最关键的区别在于，本研究明确地将“潜在影响”和“不确定性”这两个被以往研究忽视的维度操作化并纳入框架。
  - **可视化描述**：
    - **图7 & 图8**: 使用演化图展示了ERTs随时间的持续性和连贯性。图中每个主题都有一条清晰的演化轨迹，且在后期（2013-2017之后）杰卡德相似度（连贯性）普遍急剧上升，直观地验证了这些主题的聚合趋势。
    - **图9**: 展示了早期（2004-2008）主题间的引文网络结构，为计算不确定性指标提供了基础。

- **主要实验结论与作者解释**
  - 实验成功在干细胞领域识别出26个具有高新颖性和增长性的ERT。
  - 这些ERT在“潜在影响”和“不确定性”两个参考指标上表现各异，这表明即使都是ERT，其发展模式和政策需求也不同。
  - 基于这三个主要维度的表现，作者将26个ERT分为7种模式（如NG-PI-UR型，即三高型），并为每种模式提出了具体的R&D布局建议。例如，对于三高型主题（如Topic 1046），建议优先布局和重点投资；而对于仅新颖性高（NG型）的主题，则建议在适当布局的同时，加速成果转化并监控其发展不确定性。这为政策制定提供了直接的、可操作的指导。

### 4. 研究结论
- **重要发现（定量 / 定性）**
  1.  **方法有效性**：本研究提出的多维指标框架能够有效地在特定领域内以细粒度识别出ERT，并且识别结果得到了外部定性证据的支持。
  2.  **ERT模式划分**：ERT并非单一模式，可以根据新颖增长性（NG）、潜在影响（PI）和不确定性降低（UR）这三个维度的不同组合，划分为七种不同的发展模式。
  3.  **指标独立性**：构成框架的多个指标在很大程度上是相互独立的，这强调了综合评估的必要性和优越性。
  4.  **策略应用**：不同的ERT模式对应不同的R&D布局策略，从而使科技政策的制定更具针对性和前瞻性。

- **对学术或应用的意义**
  - **学术意义**：
    - 首次尝试将ERT的五个关键属性（包括潜在影响和不确定性）进行全面的操作化和实证分析，丰富了ERT的理论内涵。
    - 为“不确定性”等抽象概念提供了创新的量化思路，推动了科学计量学方法论的发展。
    - 通过定量方法对ERT、FT、TT等概念进行了辨析，为相关研究提供了更清晰的概念基础。
  - **应用意义**：
    - 为政府、科研资助机构等决策者提供了一个灵活、可定制的工具，用于评估和优先排序科研方向。
    - 提出的“ERT模式-R&D策略”对应框架，为科技政策的制定和创新资源的优化配置提供了直接、具体的决策支持。

### 5. 创新点列表
1.  **首创全面的ERT五维框架**：首次设计并操作化了一个综合衡量新兴研究主题（ERT）五大特征的框架，即：根本新颖性、相对快速增长、持续性与连贯性、潜在高影响、以及不确定性与模糊性降低。
2.  **量化“不确定性”维度**：创新性地提出通过测量一个主题对知识网络拓扑结构（强/弱连通分量）的影响来量化其“不确定性降低”的程度，为这一抽象概念的评估提供了可行的计算方法。
3.  **深化“潜在影响”的衡量**：通过对科学论文与技术专利间的**双向**引文关系（即论文引专利、专利引论文）进行综合分析，更全面地评估了ERT的潜在社会经济影响，超越了传统的单向分析。
4.  **清晰化相关概念**：结合定性讨论与定量分析（出版物趋势、覆盖网络图、词频分析），系统地辨析了ERT与研究前沿（FT）、颠覆性主题（TT）等易混淆概念之间的区别与联系。
5.  **提出 actionable 的R&D策略**：基于多维指标的评估结果，将识别出的ERTs划分为七种不同的发展模式，并为每种模式提出了具体、可操作的R&D布局策略，极大地增强了研究的现实应用价值和政策指导意义。