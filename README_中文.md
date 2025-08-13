# 柔性作业车间调度问题端到端深度强化学习系统

## 项目简介

这是一个基于深度强化学习的柔性作业车间调度问题(FJSP)求解系统。本项目实现了首个针对FJSP的多动作端到端深度强化学习框架，无需预定义调度规则即可直接生成高质量的调度方案。

## 更新日志

**2023/02/15** 修订了 'PPOwithValue.py' 文件，使其适配更高版本的PyTorch。

**2022/11/03** 推荐使用 torch == 1.4.0

**2022/09/24** 上传了FJSP_realworld文件，可以下载并运行 'validation_realWorld.py' 在标准基准实例上进行测试。

**2022/09/12** 解决了一些问题，请下载最新代码。如有任何问题请发邮件至：kunlei@my.swjtu.edu.cn

## 主要功能

### 1. 策略训练
- 运行 `FJSP_MultiPPO` 项目中的 `PPOwithValue` 文件进行策略训练
- 支持随机生成实例的训练和验证

### 2. 模型验证  
- 运行 `validation` 文件在随机生成的实例上测试/验证模型性能
- 运行 `validation_realWorld.py` 在真实世界基准实例上测试

### 3. 基准测试
- 可以下载我GitHub账户中名为 'FJSP-benchmarks' 的项目，在真实世界实例上测试训练好的模型

## 研究动机

大多数传统方法，包括基于数学规划的精确方法和元启发式算法，由于时间复杂性无法应用于大规模FJSP实例或实时FJSP实例。一些研究者使用深度强化学习来解决组合优化问题并取得了良好效果，但FJSP受到的关注较少。一些基于深度强化学习的FJSP求解方法设计为选择复合调度规则而不是直接寻找调度解，其性能依赖于调度规则的设计。据我们所知，还没有研究通过多动作端到端深度强化学习框架在不预定义调度规则的情况下求解FJSP。

在本文中，我们提出了一种新颖的FJSP端到端无模型深度强化学习架构，并证明了它在解质量和效率方面的优异性能。所提出的无模型深度强化学习架构可以直接应用于任意FJSP场景，无需提前对环境建模。也就是说，在调用环境时，与马尔可夫决策过程(MDP)相关的转移概率分布(和奖励函数)没有被显式定义。同时，基于我们策略网络设计的优势，我们的架构不受实例规模限制。

## 技术特点

### FJSP析取图的图神经网络
析取图提供了调度状态的完整视图，包含数值和结构信息，如先后约束、每台机器上的处理顺序、每个操作的兼容机器集合，以及兼容机器对每个操作的处理时间。提取析取图中嵌入的所有状态信息对于实现有效的调度性能至关重要。这促使我们利用图神经网络(GNN)来嵌入复杂的图状态。我们使用图同构网络(GIN)来编码析取图。

### 深度强化学习算法
为了应对这种多动作强化学习问题，我们提出了一种多近端策略优化(multi-PPO)算法，采用多actor-critic架构，并使用PPO作为其策略优化方法来学习两个子策略。PPO算法是一种最先进的策略梯度方法，具有actor-critic风格，广泛用于处理离散和连续控制任务。然而，PPO算法不能直接用于处理多动作任务，因为它通常包含一个actor来学习一个策略，每个时间步只能控制单个动作。相比之下，所提出的multi-PPO架构包含两个actor网络(分别将作业操作和机器编码器-解码器作为两个actor网络)。

## 使用方法

### 环境要求
- Python 3.x
- PyTorch >= 1.4.0
- 其他依赖包见代码中的import语句

### 运行训练
```bash
cd FJSP_MultiPPO
python PPOwithValue.py
```

### 运行验证
```bash
# 随机实例验证
cd FJSP_MultiPPO
python validation.py

# 真实世界实例验证  
cd FJSP_RealWorld
python validation_realWorld.py
```

## 应用扩展

该工作可以扩展到解决其他可以用析取图表示的调度问题类型，例如：
- 流水车间调度问题
- 动态FJSP等
- 带有其他目标的FJSP，例如平均完工时间和序列依赖及设置时间

所提出的multi-PPO算法可以扩展到解决其他需要多动作决策的组合优化问题。

## 引用

如果使用本开源代码，请正确引用我们的工作！

```
Kun Lei, Peng Guo, Wenchao Zhao, Yi Wang, Linmao Qian, Xiangyin Meng, Liansheng Tang,
A multi-action deep reinforcement learning framework for flexible Job-shop scheduling problem,
Expert Systems with Applications,
Volume 205,
2022,
117796,
ISSN 0957-4174,
https://doi.org/10.1016/j.eswa.2022.117796.
```

## 许可证

见 LICENCE 文件

## 联系方式

如有问题，请发邮件至：kunlei@my.swjtu.edu.cn