# gunsrpg

极限挑战 · 无暇赴死 — Guns RPG 技能树移植模组（1.20.1 Forge）。

> **整合包状态（2026-05）**：已暂时从 `mods/` 禁用（`gunsrpg-*.jar.disabled`），改用外部枪械 mod 联调。恢复：`gunsrpg-1201/tools/enable-mod.ps1`；禁用：`tools/disable-mod.ps1`。

## 功能（0.1.0-alpha）

- 从 `config/gunsrpg/port_from_gunsrpg/` 加载 314 技能节点、52 天赋、装配体扩展索引
- 技能格图标来自 Guns RPG `textures/icons/`；**装配体**显示 `weapon_mapping.json` 中的 CGM 枪图标
- 中文/英文技能名与说明见 `assets/gunsrpg/lang/*_skills.json`
- **O 键** `SkillTreeScreen`：技能树 / 扩展 / 天赋；选中节点 **解锁**（消耗技能点或武器扩展点）
- **首次进世界新手礼包**（`config/gunsrpg/starter_kit.json`）：5 技能点 + 磨骨机 I / 木制子弹 / 枪械零件 / M1911 + 手枪与子弹，避免「无枪无法涨级」
- **枪械台** `gunsrpg:gunsmith_table`：3×3 +「制造」按钮；骨粉仅此处合成
- **CGM 枪械击杀** → 账号等级 + 技能点、武器等级 + 扩展点（`leveling_strategy.json`）
- **天赋页** 选中查看说明与 `[已实现]`/`[未实装]`；再左键 +1 / 右键 -1 投资
- **生存 Debuff**（对标 Guns RPG）：出血、骨折、中毒、感染；配置 `config/gunsrpg/debuff_config.json`
  - 骨折：摔落/受击触发，减速+挖掘疲劳，冲刺/跳跃额外受伤
  - 出血/毒/感染：阶段恶化 + 持续伤害；感染可由高阶段出血扩散
  - 抗性技能树（`fracture_resistance_i` 等）+ 天赋（`fracture_resistance`、`fracture_delay` 等）降低几率、延缓恶化
  - 右上角 HUD 图标；`/gunsrpg debuff status|clear|apply <type> <stage>`
- 命令：`/gunsrpg status|reload|bootstrap`、`/gunsrpg points <n>`、`/gunsrpg unlock <技能id>`（OP）
- 联调清单：整合包 `docs/gunsrpg_playtest.md`

## 构建

```powershell
cd d:\minecraft\.minecraft\versions\1.20.1-Forge_47.4.20\gunsrpg
.\gradlew.bat build
```

将 `build/libs/gunsrpg-0.1.0-alpha.jar` 复制到整合包 `mods/`。

需 **JDK 17**。首次构建会下载 Forge 与映射，耗时较长。

## 数据依赖

确保已存在（对话中已导出）：

- `../config/gunsrpg/port_from_gunsrpg/skill_properties/`
- `../config/gunsrpg/port_from_gunsrpg/perks/`
- `../config/gunsrpg/port_from_gunsrpg/skill_index.json`
- `../config/gunsrpg/port_from_gunsrpg/leveling_strategy.json`

## 设计文档

整合包根目录 `docs/wf_gun_skill_system.md`、`docs/gunsrpg_skill_tree_port.md`。

## UI 算法来源（Guns RPG 原版）

技能树网格布局移植自开源仓库 [Toma1O6/Guns-RPG](https://github.com/Toma1O6/Guns-RPG)（分支 `1.16.5`）：

- `Tree.java` → `client/gui/layout/GunsRpgTree.java`
- `SkillTrees.java` → `client/gui/layout/GunsRpgSkillTrees.java`
- `SkillsView` 画布常量：`xUnit=6`、`yUnit=10`、节点 `22×22`

参考源码副本：`_reference_gunsrpg/`（只读，勿改）。

因 1.16.5 → 1.20.1 API 与依赖不同，无法直接编译原版 `SkillTreeScreen`，仅移植布局与连线逻辑；图标纹理后续从 Guns RPG 资源包补齐。

## 下一步

1. 击杀推进 `playerLevel` / `weaponLevel`（对接 CGM）
2. 节点解锁与 pts 消耗
3. Perk 数值应用到战斗（替代部分 KubeJS）
4. `wf_cgm_apoth` 神化 firearm 分类
