---
name: skills-creator
description: skills/note-<topic>/ 内の生メモ(SKILL.md以外の*.md)から、SKILL.md(frontmatter + 要約)を生成・再生成する。「note-xxxを更新して」「メモをスキルにまとめて」「SKILL.md作って」等の依頼時に使う。
---

# skills-creator

## ルール

- `skills/note-<topic>/`: 学習メモベースのスキル。`SKILL.md`以外の`*.md`は生メモ(ファイル名・分割は自由、絶対に編集しない)
- `note-`が付かないディレクトリは手書きスキルなのでノータッチ

## 動作

1. `skills/note-<topic>/`内の生メモを全て読む
2. `SKILL.md`を以下の内容でゼロから書き直す(過去の内容は無視してよい)
   - frontmatter: `name: note-<topic>`, `description`: 何の知見か + いつClaudeが参照すべきか
   - 本文: 生メモの要点を見出し・箇条書きで構造化した要約
