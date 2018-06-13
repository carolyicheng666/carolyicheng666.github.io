---
title: 软件测试基础
date: 2018-06-13 14:13:49
tags: [test]
categories: '编程'
---

{% cq %}
大千世界  
多的是想不通的事，多的是猜不透的心，多的是看不透的人  
不如不想，不如不猜，不如不看  
不如快意人生  
不如，吃茶去  

**许嵩《不如吃茶去》**
{% endcq %}

<!-- more -->



# 案例

{% note danger %}
1. 日本证券公司超过400亿日元损失的bug
2. 1990年AT&T公司断网，损失超7500万
3. 千年虫bug，全球损失超5000亿
{% endnote %}



# 历史

{% note success %}
1. 1972年，Bill Hetzel在NORTH Carolina大学举行第一次以软件测试为主题的正式会议
2. 1979年，Glenford J.Myers 《The art of software testing》 提出测试的经典定义
3. 1996年，测试能力成熟度模型TMM被提出，Ken Beck在极限编程XP方法论中提出TDD
4. 2009年，James A. Whittaker提出探索式测试理论
{% endnote %}



# 定义

- ## 早期定义
{% note warning %}
软件测试是对程序能够按预期运行建立起一种信心 —— Bill Hetzel 1973
{% endnote %}


- ## 经典定义
{% note warning %}
测试是为发现错误而执行程序的过程 —— Glenford J.Myers 1979
{% endnote %}

- ## IEEE定义
{% note warning %}
使用人工或自动的**手段**来运行或测定某个软件系统的过程，其目的在于检验它是否满足**规定的需求**或弄清**预期结果**与**实际结果**之间的差别
{% endnote %}

- ## 测试的五大要素
{% note warning %}
质量、人员、资源、流程、技术
{% endnote %}

- ## 测试的两个目标
{% note warning %}
测试覆盖率、测试效率
{% endnote %}



# 软件测试分类

- ## 按测试阶段分类
{% note primary %}
单元测试、集成测试、系统测试、验收测试
{% endnote %}

  - ### 单元测试
  {% note success %}
  对软件中的**最小可测试单元**进行检查和验证
  {% endnote %}

    - #### 单元测试原则
    {% note info %}
    1. 尽可能保证各个测试用例是相互独立的  
    2. 一般由代码的开发人员来实施，用以检验所开发的代码功能符合自己的设计要求
    {% endnote %}

    - #### 单元测试益处
    {% note info %}
    1. 能尽早发现缺陷
    2. 有利于重构
    3. 简化集成
    4. 减少文档
    5. 用于设计
    {% endnote %}

    - #### 单元测试限制
    {% note info %}
    1. 不可能覆盖所有的执行路径，所以不可能保证捕捉到所有路径的错误
    2. 每一行代码，一般需要 3 ~ 5 行测试代码才能完成单元测试，所以存在投入和产出的平衡
    {% endnote %}

  - ### 集成测试
  {% note success %}
  **在单元测试的基础上**，测试在将所有的软件单元按照概要设计规格说明的要求**组装**成模块、子系统或系统的过程中，各部分工作是否达到或实现相应技术指标及要求的活动
  {% endnote %}

    - #### 集成测试方案
    {% note info %}
    Big Bang、自顶向下、自底向上、核心系统集成、高频集成
    {% endnote %}

  - ### 单元测试&集成测试
  {% note success %}
  测试对象不同、测试依据不同、测试方法不同
  {% endnote %}

  - ### 系统测试
  {% note success %}
  将经过集成测试的软件，作为计算机系统的一个部分，**与系统中其他部分结合起来**，在**实际运行环境下**对计算机系统进行一系列严格有效地测试，以发现软件潜在的问题，保证系统的正常运行
  {% endnote %}

    - #### 系统测试关注点
    {% note info %}
    1. 关注系统本身的使用
    2. 关注系统与其他相关系统间的连通
    3. 关注系统在不同压力下的表现
    4. 关注系统在真实使用环境下的表现
    {% endnote %}

  - ### 系统测试&集成测试
  <table>
    <tr>
      <th width="20%" style="text-align:center;"></th>
      <th style="text-align:center;">集成测试</th>
      <th style="text-align:center;">系统测试</th>
    </tr>
    <tr>
      <td style="text-align:center;">测试对象</td>
      <td style="text-align:center;">由通过了单元测试的各个模块所集成起来的构件</td>
      <td style="text-align:center;">除了软件之外，还包括计算机硬件及相关的外围设备、数据采集和传输机构、支持软件、系统操作人员等整个系统</td>
    </tr>
    <tr>
      <td style="text-align:center;">测试时间</td>
      <td style="text-align:center;">介于单元测试和系统测试之间</td>
      <td style="text-align:center;">在集成测试之后</td>
    </tr>
    <tr>
      <td style="text-align:center;">测试内容</td>
      <td style="text-align:center;">各个单元模块之间的接口</td>
      <td style="text-align:center;">整个系统的功能和性能</td>
    </tr>
    <tr>
      <td style="text-align:center;">测试角度</td>
      <td style="text-align:center;">偏于技术角度的验证</td>
      <td style="text-align:center;">偏于业务角度的验证</td>
    </tr>
  </table>

  - ### 验收测试
  {% note success %}
  也称交付测试。针对用户需求、业务流程的正式的测试，确定系统是否满足验收标准，由用户、客户或其他授权机构决定是否接受系统
  {% endnote %}

    - #### 验收测试分类
    {% note info %}
    用户验收测试、运行验收测试、合同和规范验收测试、Alpha测试、Beta测试
    {% endnote %}

- ## 按测试手段分类
{% note primary %}
- 黑盒测试、白盒测试
- 静态测试、动态测试
- 手工测试、自动化测试
{% endnote %}
  
  - ### 黑盒测试&白盒测试
  <table>
    <tr>
      <th width="20%" style="text-align:center;"></th>
      <th style="text-align:center;">黑盒测试</th>
      <th style="text-align:center;">白盒测试</th>
    </tr>
    <tr>
      <td style="text-align:center;">设计方法</td>
      <td style="text-align:center;">等价类划分法、边界值分析法、错误推测法、因果图法、正交试验分析法、状态迁移法、流程分析法</td>
      <td style="text-align:center;">代码检测法、静态结构分析法、静态质量度量法、逻辑覆盖法、基本路径测试法</td>
    </tr>
    <tr>
      <td style="text-align:center;">优点</td>
      <td style="text-align:center;">
        <span>1. 容易实施，不需要关注内部的实现</span>
        <br> 
        <span>2. 更贴近用户的角度</span>
      </td>
      <td style="text-align:center;">
        <span>1. 迫使测试人员去仔细思考软件的实现，理解原理</span>
        <br> 
        <span>2. 可以检测代码中每条分支和路径</span>
        <br> 
        <span>3. 揭示隐藏在代码中的错误</span>
        <br> 
        <span>4. 对代码的测试比较彻底</span>
      </td>
    </tr>
    <tr>
      <td style="text-align:center;">缺点</td>
      <td style="text-align:center;">
        <span>1. 测试覆盖率较低，一般只能覆盖到代码量的不到40%</span>
        <br> 
        <span>2. 针对黑盒的自动化测试，复用率较低，维护成本较高</span>
      </td>
      <td style="text-align:center;">
        <span>1. 昂贵</span>
        <br> 
        <span>2. 无法检测代码中遗漏的路径和数据敏感性错误</span>
        <br> 
        <span>3. 不能直接验证需求的正确性</span>
      </td>
    </tr>
  </table>

    **灰盒测试**： 介于黑、白盒测试之间，关注输出对于输入的正确性，同时也关注内部表现

  - ### 静态测试&动态测试
    **静态测试**： 无须执行被测程序，而是通过评审软件文档或代码，度量程序静态复杂度，检查软件是否符合编程标准，借以发现编写的程序的不足之处，减少错误出现的概率  
    **动态测试**： 通过运行被测程序，检查运行结果与预期结果的差异，并分析运行效率、正确性和健壮性等

  - ### 手工测试&自动化测试
    **手工测试**： 由专门的测试人员从用户视角来验证软件是否满足设计要求的行为，更适合针对深度的测试和强调主观判断的测试  
    **自动化测试**： 使用单独的测试工具软件控制测试的自动化执行以及对预期结果进行自动检查
    
    <table>
      <tr>
        <th width="20%" style="text-align:center;"></th>
        <th style="text-align:center;">手工测试</th>
        <th style="text-align:center;">自动化测试</th>
      </tr>
      <tr>
        <td style="text-align:center;">优点</td>
        <td style="text-align:center;">
          <span>1. 易发现缺陷</span>
          <br> 
          <span>2. 容易实施</span>
          <br> 
          <span>3. 创造性</span>
        </td>
        <td style="text-align:center;">
          <span>1. 高效率、速度快</span>
          <br> 
          <span>2. 高复用性</span>
          <br> 
          <span>3. 覆盖率容易度量</span>
          <br> 
          <span>4. 准确、可靠</span>
          <br> 
          <span>5. 不知疲劳</span>
        </td>
      </tr>
      <tr>
        <td style="text-align:center;">缺点</td>
        <td style="text-align:center;">
          <span>1. 覆盖量化难</span>
          <br> 
          <span>2. 重复测试效率低</span>
          <br> 
          <span>3. 不一致性、可靠性低</span>
          <br> 
          <span>4. 人力资源依赖</span>
        </td>
        <td style="text-align:center;">
          <span>1. 机械、发现缺陷率低</span>
          <br> 
          <span>2. 一次性投入比较大</span>
        </td>
      </tr>
    </table>

- ## 按测试模式分类
{% note primary %}
瀑布模型、敏捷测试、基于脚本的测试、基于风险的测试、探索式测试等
{% endnote %}
  - ### 传统的瀑布模型
  {% note success %}
  项目计划 → 需求分析 → 软件设计 → 程序开发 → 软件测试 → 集成维护
  {% endnote %}

    <table>
      <tr>
        <td width="20%"  style="text-align:center;">优点</td>
        <td style="text-align:center;">
          <span>1. 强调需求、设计的作用</span>
          <br> 
          <span>2. 前一阶段完成后，只需关注后续阶段</span>
          <br> 
          <span>3. 为项目提供了按阶段划分的检查点，里程碑清晰</span>
          <br> 
          <span>4. 文档规范</span>
        </td>
      </tr>
      <tr>
        <td width="20%" style="text-align:center;">缺点</td>
        <td style="text-align:center;">
          <span>1. 难以适应需求的频繁变更</span>
          <br> 
          <span>2. 项目周期后段才能看到成果</span>
          <br> 
          <span>3. 强制的里程碑、完成时间点</span>
          <br> 
          <span>4. 文档工作量大</span>
        </td>
      </tr>
    </table>

    **瀑布模型的演变**： V模型、W模型、X模型、H模型

  - ### 敏捷测试
    - #### 特点
    {% note success %}
    1. 强调从用户的角度
    2. 重点关注持续迭代地测试新开发的功能，而不再强调传统测试过程中严格的测试阶段
    3. 建议尽早开始测试，不间断测试，具备条件即测试
    4. 强调持续反馈
    5. 预防缺陷重于发现缺陷
    {% endnote %}

    - #### 四个核心价值
    {% note success %}
    1. 个体和互动高于流程和工具
    2. 可用的软件高于详尽的文档
    3. 客户合作高于合同谈判
    4. 响应变化高于遵循计划
    {% endnote %}

  - ### 敏捷测试&传统测试
    <table>
      <tr>
        <th style="text-align:center;">传统测试</th>
        <th style="text-align:center;">敏捷测试</th>
      </tr>
      <tr>
        <td style="text-align:center;">
          <span>1. 测试是质量的最后保护者</span>
          <br> 
          <span>2. 严格的变更管理</span>
          <br> 
          <span>3. 预先的计划和细节的准备</span>
          <br> 
          <span>4. 重量级文档</span>
          <br> 
          <span>5. 各阶段测试有严格的入口和出口标准</span>
          <br> 
          <span>6. 更多在回归测试时进行重量级的自动化测试</span>
          <br> 
          <span>7. 严格依赖流程执行</span>
          <br> 
          <span>8. 测试团队和开发团队是相对独立的</span>
        </td>
        <td style="text-align:center;">
          <span>1. 开发和测试人员紧密合作，大家都有责任对软件负责</span>
          <br> 
          <span>2. 变更是可接受的，拥抱变更</span>
          <br> 
          <span>3. 计划随着进展时常调整</span>
          <br> 
          <span>4. 只需要绝对必要的文档</span>
          <br> 
          <span>5. 各迭代之间已经没有明显的入口和出口标准</span>
          <br> 
          <span>6. 所有阶段都需要自动测试，每个人都需要做，是项目集成的一部分</span>
          <br> 
          <span>7. 流程不再需要严格执行</span>
          <br> 
          <span>8. 团队合作是无缝隙合作</span>
        </td>
      </tr>
    </table>

- ## 按测试类型分类
{% note primary %}
功能测试、性能测试、部署测试、文档测试、安全测试、兼容性测试、易用性测试、本地化测试、无障碍测试、可靠性测试
{% endnote %}

- ## 其他分类
{% note primary %}
回归测试、冒烟测试、Monkey测试、A/B测试
{% endnote %}