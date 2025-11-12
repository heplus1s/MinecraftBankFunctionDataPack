# BankDatapack (Minecraft Java 1.20.1)

![Code: GPL-3.0-or-later](https://img.shields.io/badge/Code-GPL--3.0--or--later-blue)
![Assets: CC BY-NC 4.0](https://img.shields.io/badge/Assets-CC%20BY--NC%204.0-lightgrey)
![Minecraft 1.20.1](https://img.shields.io/badge/Minecraft-1.20.1-red)

**中文简介**  
面向生存/服务器经济的银行系统数据包：**铜/钻双币种**、**差额驱动动态汇率**（可设最小/最大与权重）、**GUI 存入/兑换/取款**、实时**账本与准备金**。界面基于**矿车网点**，结算采用记分板，避免物品丢失与复制。

**English**  
Banking datapack for survival/server economy: **dual currency** (copper/diamond), **difference-driven dynamic FX** (min/max & weights), **GUI deposit/exchange/withdraw**, live **ledgers & reserves**. Uses a **chest minecart outlet** GUI; scoreboard-driven settlement.

---

## ✨ Features / 功能（与当前版本一致）
- **Dual currency**：copper & diamond / 铜与钻双币种  
- **Dynamic exchange rate**（差额驱动）：基于准备金与账面差额（`#ALPHA_C/#ALPHA_D`）并在 `[RMIN, RMAX]` 内夹紧  
- **GUI**：
  - 存入：**顶排第 2 槽**放入铜锭/钻石即可入账  
  - 兑换：**买入/卖出钻石**按钮位于第 2/3 排（槽 9–14、18–23），支持 **×1/×10/×64/×100/×1000/×10000**  
  - 取款：铜与钻各提供 **×1/×10/×64**（槽 15–17、24–26）  
  - 顶排第 8 槽为**关闭**按钮  
- **Fees**：交易费率 `#FEE`（默认 2%）  
- **Hooks**：自动 `load`/`tick`，清理 GUI 掉落物与离开范围的界面

> 注：本版本**未包含**“管理员面板/查询指令集合”，参数调整通过记分板变量完成（见下）。

---

## 🔧 Installation / 安装
1. 将 `BankDatapack-1.20.1-*.zip` 放入：`world/datapacks/`  
2. 进入世界或服务器执行：`/reload`（自动挂载 `bank:load` 与 `bank:tick`）  
3. 兼容性：`pack.mcmeta` → `pack_format: 15`（MC 1.20.1）

---

## 🚀 Open the GUI / 打开界面
- 服务器玩家（无 OP）：
  ```
  /trigger bankGUI set 1
  ```
- 管理员/OP 亦可直接：
  ```
  /function bank:open
  ```
进入后按界面提示操作（顶排 2 槽存入；第 2 排买入钻石；第 3 排卖出钻石；15–17/24–26 为取款；8 为关闭）。

---

## ⚙️ Configuration / 配置（记分板变量，与实现一致）
```mcfunction
# 费率（%），默认 2（load 中已自动设为 2，存在时不覆盖）
/scoreboard players set #FEE bank_fee 2

# 差额权重（‰），影响 EC/ED 计算（默认 1000，各自独立）
/scoreboard players set #ALPHA_C bank_cfg 1000
/scoreboard players set #ALPHA_D bank_cfg 1000

# 汇率上下限（铜/钻），默认 [1, 999999]
/scoreboard players set #RMIN bank_cfg 1
/scoreboard players set #RMAX bank_cfg 999999

# 可选：初始化准备金与账面（按需）
/scoreboard players set #C bank_reserve 1000000   # 铜准备金
/scoreboard players set #D bank_reserve 1000      # 钻准备金
# 总账面（通常由交易维护，无需手动）
# /scoreboard players set #TC bank_total <value>
# /scoreboard players set #TD bank_total <value>
```

**内部变量与目标（摘录）**  
- 玩家余额：`copper` / `diamond`  
- 交易临时：`bank_tmp` / `bank_tmp2`  
- 汇率：`#rate bank_rate`（由 `internal` 计算并夹紧至 `[RMIN, RMAX]`）  
- 准备金：`#C/#D bank_reserve`；账面：`#TC/#TD bank_total`  
- 常量：`bank_const`（`#HUNDRED/#PERMIL/#k64/#k100/#k1000/#k10000` 等）

---

## 🧰 Uninstall / 卸载
1. 退出所有玩家的 GUI（顶排 8 槽关闭或离开范围自动清理）  
2. 从 `world/datapacks/` 移除数据包并 `/reload`

---

## ❓ FAQ / 常见问题
**Q：存款是按钮吗？**  
A：不是。把铜锭/钻石放入**顶排第 2 槽**即会入账。

**Q：为什么没有 100/1000/10000 的取款按钮？**  
A：当前版本仅提供 **×1/×10/×64** 取款。大额可多次操作或由管理员添加扩展按钮。

**Q：如何固定汇率？**  
A：把定时/触发的漂移逻辑关闭（本版本默认无定时漂移），或直接设定 `#rate` 并保持 `ALPHA_*` 为 0；也可通过上下限夹紧到定值。

---

## 📜 License / 许可
- **Code 代码**：**GPL-3.0-or-later**（见 [LICENSE](./LICENSE)）  
- **Assets 资源**（图标/图片/纹理/截图等非代码）：**CC BY-NC 4.0**（见 [LICENSE-ASSETS](./LICENSE-ASSETS)）  
*“Minecraft” 是 Mojang Synergies AB 的商标；本项目与 Mojang 无关且未获其认可。*

*Last updated: 2025-11-12*
