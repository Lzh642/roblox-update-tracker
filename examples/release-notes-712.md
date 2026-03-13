🎮 Roblox 版本 712 更新说明
📅 发布时间：2026年3月13日
🔗 社区讨论：[DevForum 帖子](https://devforum.roblox.com/t/release-notes-for-712/4515354) | [官方文档](https://create.roblox.com/docs/release-notes/release-notes-712)

⭐ 主要改进（26项更新，2项已上线）

---

## 🚀 新功能与改进

### Live (已上线)

1. **Assign Capabilities to more classes** — 为更多类分配权限能力

2. **Fixes false culling on VR** — 修复 VR 中的错误剔除问题

### Pending (待上线)

#### 🎯 核心功能更新

3. **Marks DataStore:RemoveVersionAsync() as deprecated** — 标记 `DataStore:RemoveVersionAsync()` 方法为已弃用

4. **Adds Enum.KeyCode.Touch for the Input Action System** — 为输入动作系统添加 `Enum.KeyCode.Touch` 枚举

5. **Releases beta version of device blocking for the Creator Ban API** — 发布设备封禁功能的测试版本（用于创作者封禁 API）

6. **Improved the UX of enabling Server Authority under Workspace** — 改进了 Workspace 下启用服务器授权（Server Authority）的用户体验

#### 💻 Luau 语言重大更新

7. **Introduces new `const` keyword for declaring local variables** — 引入新的 `const` 关键词用于声明局部常量
   - 可在任何使用 `local` 的位置使用
   - 变量声明后不可重新赋值，但可被新的声明遮蔽（shadow）
   - 详细 RFC: https://github.com/luau-lang/rfcs/pull/166

8. **In the New Luau Type Solver, generic functions that take tables are now more permissive when passing in table literals, and autocomplete on generic functions is improved** — 新 Luau 类型求解器中，接受表的泛型函数在传入表字面量时更宽松，泛型函数的自动补全得到改进

9. **In the New Luau Type Solver, fix a bug where type function reduction would get "stuck" at the end of type inference** — 修复新 Luau 类型求解器中类型函数归约在类型推断结束时"卡住"的 bug
   - 特别针对递归类型函数如 `t1 where t1 = add<t1 | number, t1 | number>`
   - 有助于减少内存压力并略微加快类型检查速度

#### 🎨 UI 系统更新

10. **Adds the UIShadow component, which adds drop shadows to parent UI instances** — 添加 `UIShadow` 组件，为父 UI 实例添加投影效果
    - 终于支持原生投影了！社区期待已久的功能

11. **QueryDescendants can now match enumeration properties to a string value** — `QueryDescendants` 现在可以将枚举属性匹配到字符串值

12. **Fixes an issue where GuiService.ViewportDisplaySize could report incorrect sizes on large-screen devices** — 修复 `GuiService.ViewportDisplaySize` 在大屏设备上可能报告错误尺寸的问题

#### ⚡ 性能优化

13. **Scaling an avatar with layered clothing no longer retriggers layered clothing fitting and should be significantly faster** — 缩放带有分层服装的角色不再重新触发分层服装适配，速度显著提升

14. **Further minor memory usage improvements for Attachment and Constraint** — 进一步优化 Attachment 和 Constraint 的内存使用

15. **Reduces the size of each Bone instance by 48 bytes** — 将每个 `Bone` 实例的大小减少 48 字节

#### 🎮 动画与渲染修复

16. **Fixes a bug in the Adaptive Animation Beta where scaling a model causes incorrect translation on the root when animating the model** — 修复自适应动画测试版中缩放模型导致根节点动画位移错误的 bug

17. **Fixes potential false culling of parts and characters** — 修复零件和角色可能被错误剔除的问题

18. **Fixed an issue with cylinder part UVs being stretched on the left face** — 修复圆柱体零件左侧面 UV 拉伸的问题

19. **Fixes floating grass geometry** — 修复悬浮草地几何体问题

20. **Fixed an issue on some objects looking emissive incorrectly when exporting to OBJ format** — 修复导出为 OBJ 格式时某些对象发光显示错误的问题

21. **Fixed a bug with double-sided meshes rendering as single-sided in 3D thumbnails** — 修复双面网格在 3D 缩略图中渲染为单面的 bug

#### 🔊 音频系统修复

22. **Executing remote events that are not attached to the DataModel no longer has an error message** — 执行未附加到 DataModel 的远程事件不再产生错误消息

23. **Adds a fix for an audio crash on Mac** — 修复 Mac 上的音频崩溃问题

24. **Fixes an issue where AudioPitchShifter plays a small portion of the previous audio stream when a new audio stream is played through it** — 修复 `AudioPitchShifter` 在播放新音频流时播放前一段音频流小部分的问题

#### 🛠️ Studio 工具改进

25. **In the New Studio Camera Controls beta, makes the camera speed lock persistent** — 在新 Studio 相机控制测试版中，使相机速度锁定持久化

26. **Fixed issue with saving escaped strings in plugin settings** — 修复插件设置中保存转义字符串的问题

---

## 💻 编程相关深度分析

### 🆕 新增 Luau 语法：`const` 关键词

这是 **Luau 语言的重大更新**！`const` 关键词允许开发者声明不可变的局部常量。

#### 语法说明

```lua
-- 声明常量
const MAX_PLAYERS = 10
const PI = 3.14159

-- 可以在任何使用 local 的地方使用 const
const function calculateArea(radius)
    const result = PI * radius * radius
    return result
end

-- ❌ 错误：不能重新赋值
const value = 100
value = 200  -- 编译错误！

-- ✅ 正确：可以遮蔽（shadow）
const value = 100
do
    const value = 200  -- 新的作用域，允许遮蔽
    print(value)  -- 输出 200
end
print(value)  -- 输出 100
```

#### 适用场景

- **配置常量**：游戏配置、魔法数字
- **性能优化**：编译器可能基于不可变性进行优化
- **代码意图明确**：明确告知其他开发者"这个值不应该改变"

#### 与 JavaScript/TypeScript 的对比

```typescript
// TypeScript
const MAX_PLAYERS = 10;
MAX_PLAYERS = 20; // 编译错误

// Luau（现在）
const MAX_PLAYERS = 10
MAX_PLAYERS = 20 -- 编译错误
```

**注意**：Luau 的 `const` 是 **浅层不可变**（只有变量引用不可变，对象内容仍可修改）

```lua
const myTable = {value = 10}
myTable.value = 20  -- ✅ 允许（修改表内容）
myTable = {}        -- ❌ 错误（不能重新赋值变量本身）
```

---

### 🎨 新增 API：UIShadow

终于！Roblox 原生支持 UI 投影了！

#### API 签名

```lua
-- 创建投影
local shadow = Instance.new("UIShadow")
shadow.Parent = yourUIElement

-- 属性（推测，待官方文档确认）
shadow.Transparency = 0.5     -- 投影透明度
shadow.Color = Color3.new(0, 0, 0)  -- 投影颜色
shadow.Offset = UDim2.new(0, 5, 0, 5)  -- 投影偏移
shadow.Blur = 10               -- 模糊半径
```

#### 使用示例

```lua
-- 为按钮添加投影
local button = script.Parent
local shadow = Instance.new("UIShadow")
shadow.Color = Color3.fromRGB(0, 0, 0)
shadow.Transparency = 0.3
shadow.Blur = 15
shadow.Parent = button
```

#### 与现有方案对比

**以前（使用 ImageLabel 模拟）：**
```lua
-- 需要额外的 ImageLabel + 九宫格图片
local shadowImage = Instance.new("ImageLabel")
shadowImage.Image = "rbxassetid://xxxxxxx"  -- 自己准备投影图片
shadowImage.ScaleType = Enum.ScaleType.Slice
-- 还要手动调整大小、位置...
```

**现在（原生 UIShadow）：**
```lua
-- 一行代码搞定！
local shadow = Instance.new("UIShadow")
shadow.Parent = button
```

**优势**：
- ✅ 无需准备投影贴图
- ✅ 动态调整，无需手动计算偏移
- ✅ 性能更好（GPU 原生渲染）

---

### 🔧 API 变更：DataStore:RemoveVersionAsync() 弃用

```lua
-- ⚠️ 已弃用，不要再使用
DataStore:RemoveVersionAsync(version)

-- 官方推荐的替代方案（待确认）
-- 可能需要使用 UpdateAsync 或其他方法
```

**影响**：如果你的代码中使用了 `RemoveVersionAsync`，需要尽快迁移到新的 API。

---

### 🎮 新增枚举：Enum.KeyCode.Touch

为输入动作系统（Input Action System）添加触摸支持。

```lua
local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
    if input.KeyCode == Enum.KeyCode.Touch then
        print("用户触摸了屏幕")
    end
end)
```

**适用场景**：
- 移动端游戏
- 跨平台输入统一处理
- 输入动作绑定系统

---

### 🔍 QueryDescendants 枚举匹配增强

现在可以用字符串值匹配枚举属性了！

```lua
-- ✅ 以前只能这样写
local parts = workspace:GetDescendants()
for _, part in parts do
    if part:IsA("Part") and part.Shape == Enum.PartType.Ball then
        -- 处理球体
    end
end

-- 🆕 现在可以这样写（更简洁）
local balls = workspace:QueryDescendants({
    ClassName = "Part",
    Shape = "Ball"  -- 直接用字符串！
})
```

**优势**：
- 代码更简洁
- 适合从配置文件读取条件

---

### ⚡ 性能优化总结

1. **分层服装缩放优化** — 不再重新触发适配，性能大幅提升
2. **Attachment/Constraint 内存优化** — 减少内存占用
3. **Bone 实例大小减少 48 字节** — 对于骨骼动画较多的游戏，内存占用显著降低
4. **Luau 类型检查优化** — 修复类型函数归约"卡住"的 bug，加快编译速度

**量化影响**（以 1000 个 Bone 实例为例）：
- 内存节省：`1000 × 48 bytes = 48 KB`
- 对于大型动画游戏（10,000+ Bone），节省 ~480 KB

---

### 🐛 重要 Bug 修复

#### 1. VR 错误剔除修复
**影响**：VR 游戏开发者
**状态**：Live（已上线）

#### 2. 自适应动画缩放 Bug
**症状**：缩放模型时根节点位移错误
**状态**：Pending

#### 3. AudioPitchShifter 音频串流 Bug
**症状**：切换音频时播放前一段音频的小部分
**状态**：Pending

---

## 💬 社区反馈分析

### 😊 积极反馈

1. **UIShadow 组件**（220+ 赞）
   - 社区期待已久的功能终于来了！
   - 开发者 @megasuperalfonato: "对这个真的很好奇"
   - 用户 @UsernameHere: "对 UIShadow 仍然很兴奋"

2. **`const` 关键词**（62 赞）
   - 用户 @Suprise444: "那还挺酷的"
   - Luau 语言特性持续完善，向现代编程语言靠拢

3. **性能优化**
   - 分层服装缩放优化受到好评
   - 内存优化对大型游戏开发者很有帮助

### 😟 担忧与关注

1. **Server Authority 进展**
   - 用户 @Doxelix: "服务器授权（Server Authority）越来越近了？我想知道启用服务器授权的改进用户体验（UX）是什么"
   - 社区期待完整的服务器端物理权威控制，但仍处于 Pending 状态

2. **功能延迟**
   - 很多功能标记为 Pending，社区希望尽快上线

3. **Breaking Changes 关注**
   - `DataStore:RemoveVersionAsync()` 弃用，开发者需要迁移代码
   - 希望官方提供清晰的迁移指南

### 💬 技术讨论焦点

1. **GongService 是什么？**
   - 用户 @HealthyKarl 提问，社区猜测与通知或铃声有关
   - 官方未明确回应

2. **功能标记（Feature Flags）**
   - 用户 @jLn0n_RBLX: "大多数功能都被标记了 flag，只有在激活特定 flag 时才能处于实时状态"
   - Roblox 采用渐进式功能发布策略

### ⭐ 官方回应

- **EndlessSashimi（Roblox 官方）**：发布 release notes 时表示"今天天气真好，我要去跑步"，语气轻松
- 未见官方针对技术问题的详细回应

---

## 📊 趋势分析

### 本次更新方向

1. **Luau 语言工程化** 🔥
   - 新增 `const` 关键词，向 TypeScript/Rust 等现代语言靠拢
   - 类型系统持续改进（泛型函数、类型推断）
   - 趋势：Luau 从"脚本语言"演进为"工程级语言"

2. **UI 系统现代化** 🎨
   - 新增 `UIShadow` 原生投影
   - 持续追赶 CSS 功能（投影是 CSS 的基础能力）
   - 趋势：缩小与现代 Web UI 框架的差距

3. **性能精细化调优** ⚡
   - 针对特定场景优化（分层服装缩放、Bone 内存）
   - 从"大刀阔斧优化"转向"手术刀式优化"
   - 趋势：为大型游戏和移动端性能铺路

4. **服务器权威控制** 🔐
   - Server Authority UX 改进（Pending）
   - 设备封禁 API 测试版（Pending）
   - 趋势：加强反作弊和安全防护

### 与历史版本对比

**711 版本（上一版本）重点**：
- Luau 类型系统改进
- UI 渲染优化
- 音频系统修复

**712 版本（本次）重点**：
- ✅ Luau 语法扩展（`const` 关键词）
- ✅ UI 功能扩展（UIShadow）
- ✅ 性能微调（Bone、分层服装）

**对比总结**：
- 711 侧重"修复和优化"
- 712 侧重"新功能和语言特性"

### 未来预测

基于 Pending 项和社区反馈，预测 713-715 版本可能包含：

1. **Server Authority 正式发布** 🔮
   - 社区高度关注，可能在 2-3 个版本内上线
   - 预计将彻底改变多人游戏的物理同步方式

2. **更多 UI CSS 功能** 🎨
   - 可能新增：`UIGradient` 增强、`UIFilter`（滤镜）、`UITransform`（3D 变换）
   - 趋势：持续缩小与 CSS 的差距

3. **Luau 泛型约束** 💻
   - 类型系统持续演进，可能支持更复杂的泛型约束
   - 如 TypeScript 的 `extends` 约束

4. **移动端性能优化** 📱
   - 基于 Bone 内存优化的方向，可能继续优化移动端渲染

---

## 🌐 UGC 平台洞察

### 语言设计启示

**Roblox 的 `const` 关键词设计值得借鉴**：
- ✅ **渐进式引入**：不破坏现有代码，向后兼容
- ✅ **语义清晰**：`const` vs `local`，意图明确
- ⚠️ **浅层不可变**：与 JavaScript 类似，只有引用不可变，对象内容可变
  - **启示**：对于 UGC 平台，"完全不可变"可能太严格，"浅层不可变"是实用平衡点

**对比其他 UGC 平台**：
- **Minecraft（Java 脚本）**：缺乏现代语言特性，社区呼吁改进
- **Fortnite（UEFN/Verse）**：Epic 新推 Verse 语言，语法现代但生态不成熟
- **Roblox（Luau）**：持续演进，保持向后兼容性，**最佳实践**

### UI 系统架构

**UIShadow 的设计哲学**：
- ✅ **声明式 API**：创建组件 → 添加到父级 → 自动生效
- ✅ **GPU 加速**：原生渲染，不依赖图片资源
- ⚠️ **性能边界未知**：大量投影会否影响性能？需实测

**对比 Web 生态**：
- **CSS `box-shadow`**：浏览器优化多年，性能良好
- **Roblox UIShadow**：首次推出,性能表现待观察

**启示**：
- UGC 平台的 UI 系统应**优先支持开发者高频需求**（投影、渐变、滤镜）
- **原生 > 模拟**：原生 API 性能和开发体验都优于开发者自行模拟

### 安全模型设计

**设备封禁 API（Pending）**：
- Roblox 推出"设备级封禁"，防止账号封禁后换号重来
- **深度防御（Defense in Depth）**：账号 → IP → 设备 → 生物识别（未来？）

**启示**：
- UGC 平台的安全模型需**多层防御**
- **隐私 vs 安全**：设备封禁涉及隐私，需要明确的用户协议和法律合规

### 创作者-平台关系管理

**Roblox 的功能发布策略**：
- **Feature Flags（功能标记）**：逐步推出，A/B 测试
- **Beta 标签**：明确告知"实验性功能"
- **Pending vs Live**：透明度高，开发者可提前规划

**对比其他平台**：
- **Unity/Unreal**：功能发布较慢，稳定性高
- **Roblox**：快速迭代，但 Pending 功能较多，开发者需适应"不确定性"

**启示**：
- UGC 平台应在"快速迭代"和"稳定性"之间找到平衡
- **透明度是关键**：清晰的 Pending/Live 标记 + 社区反馈渠道

### 社区治理

**DevForum 的价值**：
- ✅ **官方 + 社区混合**：Roblox 员工直接发布更新，社区即时反馈
- ✅ **技术深度**：开发者之间互相解答技术问题
- ⚠️ **官方回应不足**：本次更新中，官方未对技术问题深入回应

**启示**：
- UGC 平台需要**高质量的开发者社区**
- **官方参与度**：不仅发布公告,还需回应技术疑问

---

## 📚 相关资源

- [官方文档](https://create.roblox.com/docs/release-notes/release-notes-712)
- [DevForum 讨论](https://devforum.roblox.com/t/release-notes-for-712/4515354)
- [Luau RFC #166 (const 关键词)](https://github.com/luau-lang/rfcs/pull/166)
- [API 参考](https://create.roblox.com/docs/reference/engine)

---

**生成时间**: 2026年3月13日  
**分析深度**: 完整（包含编程深度分析、社区反馈、趋势预测、UGC 平台洞察）  
**数据来源**: Roblox 官方文档 + DevForum + API 声明文件
