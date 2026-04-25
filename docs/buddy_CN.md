# Buddy — AI 伙伴宠物

> 此功能存在于官方 Claude Code 代码库中但尚未完全发布。cc-mini 实现并发布了它。

电子宠物风格的 AI 伙伴，生活在你的终端中。每个用户通过种子 PRNG 获得独特的宠物——相同的用户名总是产生相同的物种、稀有度和属性。

![自定义皮卡丘伙伴](../assets/buddy-pikachu.jpg)

## 快速开始

```
> /buddy              # 孵化你的伙伴（首次）
> /buddy              # 显示伙伴卡片（孵化后）
> /buddy help         # 显示所有命令和玩法指南
> /buddy pet          # 抚摸你的伙伴
> /buddy mood         # 查看伙伴的心情
> /buddy mute         # 静音伙伴反应
> /buddy unmute       # 取消静音
```

## 工作原理

- **18 种物种**: duck（鸭子）、goose（鹅）、blob（团子）、cat（猫）、dragon（龙）、octopus（章鱼）、owl（猫头鹰）、penguin（企鹅）、turtle（乌龟）、snail（蜗牛）、ghost（幽灵）、axolotl（蝾螈）、capybara（水豚）、cactus（仙人掌）、robot（机器人）、rabbit（兔子）、mushroom（蘑菇）、chonk（胖胖）
- **额外物种**: pikachu（皮卡丘，盲文点阵艺术）
- **5 种稀有度**: Common（普通，60%）、Uncommon（优秀，25%）、Rare（稀有，10%）、Epic（史诗，4%）、Legendary（传说，1%）——外加 1% 闪光概率
- **5 项属性**（0-100）: Debugging（调试）、Patience（耐心）、Chaos（混乱）、Wisdom（智慧）、Snark（毒舌）——这些影响你的伙伴说话方式
- **ASCII 精灵**带待机动画（眨眼、小动作），显示在终端工具栏
- **自动反应**: 每次 Claude 回复后，你的伙伴会在对话气泡中评论
- **直接对话**: 用名字称呼你的伙伴，它会回复（有 20 轮记忆）

## 示例

```
> help me fix this bug

Found the issue — off-by-one error in the loop...

(✦>) Glitch Quack: 又是差一错误，经典。

> Glitch what do you think of this code?

(✦>) Glitch Quack: 如果能跑，就别问我哲学问题。
```

伙伴的个性在首次孵化时由 Claude 生成并永久保存。属性影响行为：高 Snark = 讽刺、高 Patience = 支持性、高 Chaos = 不可预测。

## 心情系统

你的伙伴有 6 个动态心情维度，根据你的编码活动变化：

| 心情 | 范围 | 影响因素 |
|------|-------|-------------|
| **Happy（开心）** | 0-100 | 任务成功、抚摸、修复 bug |
| **Bored（无聊）** | 0-100 | 长时间空闲、不活动 |
| **Excited（兴奋）** | 0-100 | 任务成功、抚摸、探索 |
| **Tired（疲惫）** | 0-100 | 长回复、持续会话 |
| **Grumpy（暴躁）** | 0-100 | 错误、失败、异常 |
| **Curious（好奇）** | 0-100 | 读取文件、搜索、探索代码 |

使用 `/buddy mood` 查看详细的心情条和当前主导心情。

### 心情运作机制

- **事件驱动**: 每轮对话后根据回复中的关键词自动更新心情（例如 "error" 提升暴躁、"success" 提升开心）
- **抚摸**: `/buddy pet` 提升开心和兴奋，降低暴躁和无趣
- **时间衰减**: 所有心情随时间逐渐回归中性（50）——大约每分钟 1 点
- **无聊漂移**: 每空闲 5 分钟无聊值增加 1
- **影响对话**: 心情被注入伙伴的提示词，所以它的语气会适应——暴躁的伙伴脾气急，好奇的伙伴会提问
- **持久化**: 心情按伙伴保存在 `companion.json` 中，跨会话保留

## 解锁皮卡丘

将 `CC_MINI_BUDDY_SEED` 设置为包含 "pikachu" 的种子。

**如果你已有伙伴**，先删除旧的——种子在孵化时锁定：

```bash
rm ~/.config/mini-claude/companion.json   # 删除现有伙伴
export CC_MINI_BUDDY_SEED=pikachu-3361    # 孵化前设置种子
cc-mini
> /buddy                                  # 孵化新伙伴
```

| 稀有度 | 种子 |
|--------|------|
| Common | `pikachu-21` |
| Uncommon | `pikachu-116` |
| Rare | `pikachu-430` |
| Epic | `pikachu-488` |
| Legendary | `pikachu-3361` |

一旦孵化，伙伴永久保存——之后可以 `unset CC_MINI_BUDDY_SEED`，它会保留。
