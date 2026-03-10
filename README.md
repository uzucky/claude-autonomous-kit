# 非エンジニアのためのClaude Codeテンプレート

**Claude Codeを「使い捨てチャット」から「育つ相棒」に変える、最小構成のテンプレートです。**

---

## これは何？

Claude Codeは、ターミナルで動くAIアシスタントです。
普通に使うと、セッションが切れるたびにAIは全てを忘れます。

このテンプレートは、**AIに「記憶」を持たせる仕組み**です。

- `CLAUDE.md` = AIへの指示書（毎回自動で読み込まれる）
- `memory/` = AIの記憶置き場（セッションをまたいで引き継がれる）
- `.claude/skills/` = AIに教えた「やり方」（コマンドで呼び出せる）

**元になっているのは、実際に稼働している自律型AI事業システムです。**
そこから非エンジニアでも使える最小構成を抽出しました。

---

## 誰のため？

- ターミナルの基本操作（`cd`, コピペ）ができる人
- Claude Code（Pro / Max）を契約済みの人
- AIとのやり取りを「蓄積」して、プロジェクトを育てたい人

---

## セットアップ（3ステップ）

### 1. このリポジトリをコピー

```bash
# GitHubからクローン（URLは公開後に差し替え）
git clone https://github.com/YOUR_USERNAME/claude-code-template.git my-project
cd my-project
```

または、GitHubで「Use this template」ボタンを押して、自分のリポジトリとして作成してください。

### 2. CLAUDE.mdを自分のプロジェクトに合わせて編集

`CLAUDE.md` を開いて、`<!-- ... -->` のコメント部分を自分のプロジェクトの内容に書き換えてください。

### 3. Claude Codeを起動

```bash
claude
```

起動すると、AIは自動的に `CLAUDE.md` を読み込みます。
`/onboard` と入力すると、AIが記憶を読み込んで現在地を把握します。

---

## 含まれているもの

```
my-project/
├── CLAUDE.md                    ← AIへの指示書（最重要ファイル）
├── .gitignore                   ← Git管理から除外するファイルの設定
├── .claude/
│   └── skills/                  ← AIのスキル（コマンド）
│       ├── onboard/SKILL.md     ← /onboard: セッション開始時の記憶読み込み
│       └── handover/SKILL.md    ← /handover: セッション終了時の記憶保存
└── memory/                      ← AIの記憶
    ├── handovers/               ← 引き継ぎノート（短期記憶）
    ├── learnings/               ← 学んだこと（長期記憶）
    └── decisions/               ← 判断の記録
```

---

## 基本の使い方

### セッション開始時
```
/onboard
```
AIが記憶を読み込み、前回の続きから始められます。

### セッション終了時
```
/handover
```
AIが今回の作業内容を記録し、次回に引き継ぎます。

### 日常の流れ
1. `/onboard` で開始
2. 普通にAIと会話・作業
3. 重要な判断があったら `memory/decisions/` に記録（AIに頼める）
4. `/handover` で終了

---

## カスタマイズのヒント

- **CLAUDE.mdに原則を追加**: 「コードは書かないで」「日本語で回答して」など、AIの振る舞いを制御できます
- **スキルを追加**: `.claude/skills/` にMarkdownファイルを追加すると、`/コマンド名` で呼び出せます
- **記憶を整理**: `memory/` の中身が増えてきたら、古いものをアーカイブしましょう

---

## 関連記事

このテンプレートの背景にある考え方や実践例は、noteで発信しています:

- [suzuidaki (note.com)](https://note.com/suzuidaki)

---

## ライセンス

MIT License - 自由に使ってください。
