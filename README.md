
![Code: GPL-3.0-or-later](https://img.shields.io/badge/Code-GPL--3.0--or--later-blue)
![Assets: CC BY-NC 4.0](https://img.shields.io/badge/Assets-CC%20BY--NC%204.0-lightgrey)
![Minecraft 1.20.1](https://img.shields.io/badge/Minecraft-1.20.1-red)

**中文简介**  
面向生存/服务器经济的银行系统数据包：支持铜/钻石双币种、**动态汇率**、**GUI 存取/兑换**、**玩家账本**与**银行准备金**统计、以及**管理员工具**。结算基于记分板，避免物品丢失与复制，提供一键操作（×1/×10/×64/×100/×1000/×10000）。

**English**  
Banking datapack for survival/server economy: **dual currency** (copper/diamond), **dynamic exchange rate**, **GUI deposit/withdraw/exchange**, **player ledgers** & **bank reserves**, plus **admin tools**. Scoreboard-driven settlement; one-click actions (×1/×10/×64/×100/×1000/×10000).

---

## ✨ Features / 功能
- Dual currency with dynamic FX / 双币种与动态汇率  
- GUI for deposit / withdraw / exchange / 图形化存取与兑换  
- Player ledger & bank reserves / 玩家账本与银行准备金  
- Admin panel & stats / 管理员面板与统计查询  
- Safe scoreboard settlement / 记分板安全结算  
- Optional outlet entity (e.g., chest minecart) / 可选矿车网点  

---

## 🔧 Installation / 安装
1. 将数据包 `BankDatapack-1.20.1-*.zip` 放入你的世界目录：`world/datapacks/`（无需解压）。  
2. 进入世界或启动服务器后执行：`/reload`。  
3. 确保 `pack.mcmeta` 的 `pack_format` 与 1.20.1 兼容。  
4. 成功时聊天栏会显示加载提示（或查看 `/datapack list`）。

> Java 版 1.20.1；单人或服务端均可。纯数据包，无需模组。

---

## 🚀 Getting Started / 快速开始
- 打开主 GUI：
  ```
  /function bank:open
  ```
- 使用界面中的面额按钮进行存入/取出/兑换。  
- 关闭按钮位于顶排右侧（红色“关闭”）。

> 如果你的分发版包含管理员工具，请参考下文或 `/function bank:admin/*`。

---

## ⚙️ Configuration / 配置
以下为常见的记分板参数（示例；若你的实现不同，请按实际为准）：
```mcfunction
# 手续费（百分比）默认 2
/scoreboard players set #FEE bank_fee 2

# 差额权重（千分比），影响准备金与账面差额反馈（默认 1000）
/scoreboard players set #ALPHA_C bank_cfg 1000
/scoreboard players set #ALPHA_D bank_cfg 1000

# 最小/最大汇率（铜/钻），用于夹紧（默认 1 ~ 999999）
/scoreboard players set #RMIN bank_cfg 1
/scoreboard players set #RMAX bank_cfg 999999

# 初始化准备金（可按需修改初始库存）
/scoreboard players set #C bank_reserve 1000000
/scoreboard players set #D bank_reserve 1000
```
> 这些默认值通常在 `bank:load` 初始化；你可以 `/function bank:load` 后再用上述指令调整。

---

## 🧮 Exchange Rate Idea / 动态汇率思路
可采用“基准 + 漂移 + 准备金弹性”模型（示意）：
```
rate = clamp(base ± drift ± k_reserve * f(reserves, total_ledger), min, max)
```
- `base` 基准汇率  
- `drift` 时间或事件触发的微调步长  
- `k_reserve` 准备金弹性系数（准备金越低，钻→铜越不利）  
通过 scoreboard 变量与函数周期更新即可。

---

## 🛠 Admin Tools / 管理员工具（如有）
- 打开管理员面板：`/function bank:admin/panel`  
- 查看总账与准备金：`/function bank:admin/stats`  
- 授权玩家为管理员（示例 tag）：`/tag <player> add bank.admin`  
- 清理/对账/卸载：见 `bank:admin/*` 函数集合

> 建议仅 OP 或带 `bank.admin` 标签的玩家使用。

---

## 🧰 Uninstall / 卸载
1. 清空网点实体与未结算物品（若提供清理函数先执行）。  
2. 从 `world/datapacks/` 移除数据包并 `/reload`。

---

## ❓ FAQ / 常见问题
**Q1：物品会不会丢？**  
A：结算基于记分板并加入多步校验；异常中断时可由管理员对账/回滚（具体以你的管理函数为准）。

**Q2：如何固定汇率，不让它浮动？**  
A：把定时更新逻辑移除或把 `drift` 设为 0；也可以固定设置 `bank_rate`。

**Q3：支持多世界/多维度吗？**  
A：记分板是全局的；若网点实体依赖坐标，建议各世界单独部署网点。

---

## 🤝 Contributing / 贡献
欢迎 PR！提交前请自测：`/reload` 无报错；存取与兑换流程正确；若改动配置名或变量名，请同步更新文档。

---

## 📜 License / 许可
- **Code 代码**：**GPL-3.0-or-later**（见 [LICENSE](./LICENSE)）  
  > 衍生/再发布需保持同许可证开源  
- **Assets 资源**（图标/图片/纹理/截图等非代码）：**CC BY-NC 4.0**（见 [LICENSE-ASSETS](./LICENSE-ASSETS)）  
  > 需署名，**禁止商用**；如需商用请联系作者获取单独许可

**Trademark / 商标**  
“Minecraft” 是 Mojang Synergies AB 的商标；本项目与 Mojang 无关且未获其认可。

---

## 🔗 Links
- Releases / 发行版：见 GitHub Releases  
- Issues / 反馈：欢迎提交问题与建议

*Last updated: 2025-11-12*
