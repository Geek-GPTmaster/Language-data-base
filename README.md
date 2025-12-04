# Language-data-base
It is a SQL based database for language learners
# 多语言词典数据库

本项目提供一个通用的多语言词典 SQL 结构与示例数据，适合在 SQLite、MySQL、PostgreSQL 中实现。默认脚本使用 SQLite 语法。

## 文件说明
- `schema.sql`：数据库表结构（语言、单词、义项、翻译、例句）。
- `seed.sql`：示例数据（英/中/西：hello/你好/hola）。
- `queries.sql`：常用查询示例。

## 快速使用（SQLite）
1. 初始化数据库：
   - `sqlite3 dictionary.db < schema.sql`
2. 导入示例数据：
   - `sqlite3 dictionary.db < seed.sql`
3. 试运行查询：
   - `sqlite3 dictionary.db ".read queries.sql"`

## 在 MySQL 中使用
- 创建数据库时设定 UTF-8 编码：`CREATE DATABASE dictionary CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;`
- 将 `AUTOINCREMENT` 改为 `AUTO_INCREMENT`；时间默认可用 `CURRENT_TIMESTAMP`；`PRAGMA` 语句删除。
- 注意外键、唯一索引语法与检查约束（`CHECK`）在 MySQL 版本上的支持差异。

## 在 PostgreSQL 中使用
- 数据库默认 UTF-8 即可：`CREATE DATABASE dictionary WITH ENCODING 'UTF8';`
- 将 `INTEGER PRIMARY KEY AUTOINCREMENT` 改为 `GENERATED ALWAYS AS IDENTITY` 或 `SERIAL`。
- 删除 `PRAGMA` 语句；其余语法基本兼容（检查约束、外键等）。

## 设计要点
- 所有文本列均为 `TEXT` 并以 UTF-8 存储，支持多语言字符集。
- `languages(code)` 作为语言主键（例如 `en`/`zh`/`es`）。
- `words` 表按语言与词形去重（`UNIQUE(language_code, lemma, part_of_speech)`）。
- `senses` 拆分义项，`translations` 在义项间建立跨语言对应，避免含义不一致。
- `examples` 存放例句及其译文，便于教学与检索。

## 备选扩展
- 增加同形异义处理（同一 `lemma` 存多 `sense`）。
- 增加音标系统（多语音标、方言、音频链接）。
- 增加主题领域/标签、频次统计、来源字典等元数据。
