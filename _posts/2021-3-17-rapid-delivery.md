# 研发如何快速交付？

作为开发，一般情况下我们的任务周期是：

1. 产品提前一周～一个月，告知研发接下来的研发任务大概计划，此时可能拿到的一个目标，一个模糊的需求
2. 产品发起需求分享会，研发参与详细需求讨论
3. 研发针对需求，分任务，出估时；产品根据估时排冲刺计划（一般是周一至周五为一个冲刺计划）
4. 研发进入第一个冲刺计划，编码，dev环境测试
5. 冲刺计划完成，每周二发布到SIT(System Integration Test, 系统集成测试)环境，进行SIT测试
6. SIT测试2天，修复问题，周四进入UAT(User Acceptance Test)环境，要求客户进行UAT测试
7. 修复测试的bug，第二周的周二发布产品

所以我们综合来看，作为研发，每周的任务是：
周一周二，新任务的研发+修复上上周任务的UAT的bug+PRDbug
周三周四，新任务的研发+上周研发任务的SIT测试bug+PRDbug
周五，新任务尾声+上周研发任务的UAT测试bug+PRDbug

正常情况下，研发的是同时3个任务在身

## 理想很丰满：
  
  你代码写的好，需求拿捏的准确，几乎 0 bug，你就可以安心的研发新阶段的任务

## 现实很骨感：
  
  你可能因为需求的理解不到位已经在赶本周进度了，代码写着写着，发现需求不对，好不容易加班赶回来的进度，又推翻了[写个p的代码]，然后SIT/UAT/PRD的bug一直不断，加班加到怀疑人生。。。

## STOP！暂停键！

### V字形模型
```
业务需求发起                  业务验证        --需求发起方/业务部门
  需求分析                  需求验证         --产品经理
    系统结构设计          系统测试
      程序详细设计      功能测试，
        编码          单元测试，
   
```

所有的不确定性和问题，都会在研发阶段爆发出来[背锅]


你需要reload！

第一步：	思考下，为什么会rework？
是需求真的变动？还是需求的理解不到位？还是这个需求根本不清楚？

从V字型开发模型里面，我们可以看到，**问题暴露的越晚，修复的代价越高**

改进点：
1. 任务开始前，产品同时跟研发讨论详细需求后，研发各自整理一份对需求的理解，画出书面流程图/数据状态图等，研发开始前小组跟产品开会，各自讲解一下对需求的理解，保证每个人的理解一致，不出现太大偏差
过一遍需求后，问题统一汇总，如果产品同事还需要跟上级领导/业务团队确认的，约好时间，带上研发一起去，因为研发关注的问题，可能是一些细节，可能是一些决定代码的关键问题，让研发更多的参与原始需求的讨论，接近End User，这样在研发时更容易做出满足客户需求的产品。

2. 研发开始前，最好找客户：包括业务团队/EndUser/上级领导，把人物流程大致过一遍，保证不遗漏重大节点，不会因为个人理解的偏差导致后期的rework

3. 研发开始码字前，对当前的任务应该是90%以上是确认的，清楚的，一些很小的非关键点的需求，不影响整体流程推进，可以暂缓搁置



### 关于项目估时

如果对自己的任务大致清楚的情况下，估时应该是件不太困难的事情，why?

1. 需求明确，意味着范围是确定的

2. 需求明确，意味着数据结构是确定的：是单表？1对多？1对多对多？数据的关系是什么样的？需要建几张数据库表？

3. 需求明确，功能复杂度也是相对清楚的：是简单的逻辑？复杂的数据状态？不同数据状态的组合情况多？

4. 页面交互多？页面结构复杂？

5. 如何给出任务的估时？罗列出你需要的写API，按照每个API的复杂程度，给一个估时

6. 不知道怎么估时？ 给个参考标准是不是更容易？

7. **敏捷！=没有单元测试**： 就算敏捷开发，单元测试还是必要的
后台：提供给前端同事的接口：需要自己单元测试，而不是交给前端同事帮你做单元测试
前端：交付到SIT阶段前，必要的数据校验，数据展示，都是经过测试的，低级错误应该在Dev阶段就暴露并解决掉

所以估时：要包含必要的单元测试时间

单元测试怎么估算？看逻辑复杂程度：
复杂的逻辑，可能单元测试时间超过研发的时间

>! TODO: 列一个参考标准，什么样的表结构，什么样的逻辑，什么样的交互效果，API需要的时间范围是什么


### 任务进行中

如果前期的准备是充分的，那么写代码应该是件水到渠成的事情，是吗？[怀疑]

1. 前面估时已经列出的大部分的api了，那么第一步就是把api 根据前端的页面需要 **排序**

2. 编写假接口： 编写假接口的时候，你应该已经了解大部分的数据应该从哪里捞，如果还不清楚的话，说明你的这个需求还不够清楚，你应该及时找leader/产品同事沟通

3. 逐渐把每个假接口实现成真数据

4. 没有什么是不变的，只有变化才是不变的（需求变动？业务理解升级？）

5. 对进入研发阶段的变动，如何把控？多人协同时，往往出现需求的变动不是所有人都知道，直到对接时，才发现问题
如何解决这个问题？ 多人协作的情况，出一个项目负责人，项目负责人需要知道整个任务的大致需求，项目负责人负责统一协调：进度计划的把控，阶段性交付的产出，需求变化的统筹，技术难点的解决方案（计划安排，寻求帮助等），出现delay等情况的安排（寻求团队帮助等）


#### 阶段性交付

问一个问题：如果你是产品同事，一个任务的冲刺计划是一周，

第一天
产品：有没有可以看看的结果？
研发：没有

第二天
产品：有没有可以看看的结果？
研发：没有，问题还很多

第三天
产品：有没有可以看看的结果？
研发：没有，有一点小问题需要解决

第四天
产品：有没有可以看看的结果？
研发：快了，在调试一下就OK了


不需要第五天，你作为产品，你慌不慌？

如果5天的任务，第三天还没有可看到，然后你期望最后1-2天一下子全好，

不可能！ To young to simple!


比较好的方式：

第一天
研发：基于假接口，现在可以看到1-2个页面，UI还比较丑，页面bug还有几个，但是能看个大概

第二天
研发：新增一个页面，可看详情（不过还是假数据），不过可以创建新纪录，并且列表页也能看到真记录

第三天
研发：之前的几个页面大部分都是真数据，虽然还有点bug，有些数据的校验都没做，但是只要必要的数据填上，路是通的

第四天
研发：校验加了不少了，流程基本通畅，可以点点了，顺便帮我测试一下

第五天
研发：之前报的一些问题都修复了，也做了几轮单元测试，基本都ok，可以进SIT阶段


这样项目负责人对进度的把控是合格的，产品同事也是有信心的，团队的交付也是高效的，如果遇到问题，可能在研发第二/三天就暴露了，而不是等到了SIT阶段：
Dev阶段的调整，你最熟悉，而且基本没有其他任务干扰
Sit阶段的调整，新任务已经开始，忙于交付新阶段，部分之前的需求可能都有点忘了

结果是：调整的成本有很大的区别


## SIT/UAT测试

SIT: System Integration Test, 系统集成测试

UAT: User Acceptance Test, 用户验收测试

其他TODO