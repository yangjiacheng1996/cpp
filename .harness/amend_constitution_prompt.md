# `/speckit.constitution` 修订提示词（A + B + C）

> **用法**：新建一个会话，执行 `/speckit.constitution`，把下面 `====` 之间的内容**整段粘贴**进去。
>
> **修订依据**：`.harness/c_blocks.md`（C 语言 Block 划分草案）阶段 0 的人工评审结论。

---

====

请修订本项目宪法 `.specify/memory/constitution.md`（当前版本 1.0.0）。

本次修订共三条，**只改这三条**，其余内容逐字保留，不得增删任何其他条款。

## 修订一（对应宪法原则 V，示例可编译、标准可追溯）

将以下原文：

```
- 每张含代码的卡片 MUST 标明所依据的语言标准版本：C 用 `C11`/`C17`，C++ 用 `C++11`/`C++14`/`C++17`/`C++20`。
```

替换为：

```
- 每张含代码的卡片 MUST 标明所依据的语言标准版本。**C 语言 MUST 以 `C17` 为基准**
  （C17 与 C11 在语言层面等价，C11 是最低门槛）；涉及 C23 特性的卡片 MUST 额外标注 `C23`。
  C++ 用 `C++11`/`C++14`/`C++17`/`C++20`。
```

理由：C 语言标准版本必须单一确定，否则卡片标注不一致、读者无所适从。选择 C17 而非 C89/C99/C23 的完整论证见 `.harness/c_blocks.md` 第二节。

## 修订二（对应宪法原则 VI，目标导向）

将以下原文：

```
- 语言版本演进（历史包袱、已废弃写法）MUST 以「注意」形式附注，MUST NOT 占据正文主体。
```

替换为两条：

```
- 单张卡片 MUST NOT 以语言版本演进为主体；历史包袱、已废弃写法 MUST 以「注意」形式附注。
- 标准版本、版本差异对照与编译器扩展（GNU 扩展、编译器内建等）MUST 集中在一个专门的
  「标准版本与编译器扩展」Block 中承载，MUST NOT 分散复制进多张卡片造成重复。
```

理由：现措辞会导致「标准版本与编译器扩展」这一整个 Block 与原则 VI 直接冲突，
该 Block 的每张卡片都会被 `/speckit.analyze` 判为 CRITICAL 违规。新措辞既保住了
「单张卡片不得以版本演进为主体」的原意，又为专门的标准 Block 留出合法位置。

## 修订三（对应宪法原则 III，目录结构与编号）

将以下原文：

```
- 磁盘上的 Block 目录 MUST 与导航页的 Block 分组**一一对应**：
  MUST NOT 出现"导航有 B03 而磁盘无 B03 目录"或反之。
```

替换为两条：

```
- 磁盘上 MUST NOT 出现导航页未登记的 Block 目录。
- 导航页已登记但磁盘暂未创建的 Block 目录，MUST 在其首张卡片落地时创建
  （git 不跟踪空目录，空 Block 目录在其首张卡片入库前不会存在于版本库中）。
```

理由：git 不跟踪空目录，Block 目录要等首张卡片落地才真正进入版本库，
现措辞在 git 层面无法严格执行。

## 版本与日期

- `CONSTITUTION_VERSION`：`1.0.0` → `1.1.0`
  （修订一、修订二实质性扩充既有原则的约束，属 MINOR；修订三是措辞澄清，属 PATCH，合并后取 MINOR）
- `RATIFICATION_DATE`：**保持 `2026-11-17` 不变**
- `LAST_AMENDED_DATE`：`2026-11-18`

## Sync Impact Report

用本次修订的 Sync Impact Report **替换**文件顶部现有的 v1.0.0 Sync Impact Report
（不要叠加两份）。报告需包含：

- 版本变更：1.0.0 → 1.1.0
- 修订一：原则 V（示例可编译、标准可追溯）—— 明确 C 语言标准基准为 C17
- 修订二：原则 VI（目标导向）—— 版本演进附注要求不变，新增「标准版本与编译器扩展集中承载」条款
- 修订三：原则 III（目录结构与编号）—— 放宽 Block 目录与导航页的一一对应要求
- 备注：本次修订依据 `.harness/c_blocks.md` 阶段 0 的人工评审结论；无其他条款变动；
  遗留待办为 Block 的具体划分与卡片清单仍待人工定稿（由导航页承载，不在本文件）

## 约束

- 只写 `.specify/memory/constitution.md` 这一个文件。
- MUST NOT 修改 `.specify/templates/` 下的任何模板源文件。
- MUST NOT 修改 `.opencode/` 下的任何文件。
- MUST NOT 创建、修改或删除任何卡片、导航页、术语表或其他项目文件。
- 修订后请输出：新版本号与 bump 理由、遗留待办、建议提交信息。

====

## 预期结果（供你核对，不必粘贴给命令）

修订后 `.specify/memory/constitution.md` 应当：

1. 版本行变为 `**Version**: 1.1.0 | **Ratified**: 2026-11-17 | **Last Amended**: 2026-11-18`
2. 全文卡片总数不变（原则仍是 7 条），只改了 3 条条款、拆出 4 条新条款
3. Sync Impact Report 顶部只剩一份，且描述本次 1.0.0 → 1.1.0 的变更
4. `git status` 只显示 `.specify/memory/constitution.md` 被修改

建议提交信息：

```
docs: amend constitution to v1.1.0 (C17 baseline, standards block, block-dir rule)
```
