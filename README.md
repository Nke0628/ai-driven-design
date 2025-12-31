# AI駆動開発ガイド

AI を活用した開発フローの実践ガイド

---

## 1. コンセプト

### 1.1 AI駆動開発とは

従来の開発と AI 駆動開発の違い：

```
従来:
  仕様書 → 人間が読んで解釈 → 実装 → レビュー

プロンプト駆動:
  仕様（プロンプト） → AIが解釈して実装 → 人間がレビュー・調整
```

**本質**: プロンプトの品質 = 成果物の品質

曖昧なプロンプトは曖昧な実装を生み、明確なプロンプトは意図通りの実装を生む。
つまり「AI が理解できる形で仕様を伝える技術」が開発効率を決定する。

### 1.2 従来手法との関係

| 観点    | 手法           | 説明                             |
| ------- | -------------- | -------------------------------- |
| 起点    | Issue 駆動     | Jira/GitHub チケットが全ての起点 |
| 設計    | 仕様駆動的     | 先に仕様を明確化してから実装     |
| AI 活用 | AI駆動 | プロンプトの精度が品質を決める   |
| 実装    | 生成 AI 支援   | AI が実装、人間がレビュー        |

### 1.3 メリット

- 設計・実装・テストの高速化
- 属人化の軽減（プロンプトが知識の媒体になる）
- 一貫性のある成果物
- レビュー負荷の軽減（AI がセルフレビュー可能）

### 1.4 注意点

- AI の出力は必ず人間がレビューする
- プロンプトテンプレートはプロジェクトに合わせて育てる
- セキュリティ上機密情報をプロンプトに含めない

---

## 2. 全体フロー

### 2.1 フロー概要

```
┌─────────────────────────────────────────────────────────────┐
│ 機能開発フロー（大規模）                                     │
│                                                             │
│  /requirement <identifier> <説明>                           │
│       ↓                                                     │
│  /design <identifier> <説明>                                │
│       ↓                                                     │
│  /tasks <identifier>  → tasks.md 生成                       │
│       ↓                                                     │
│  /implement <identifier> 1  → タスク1を実装                  │
│  /implement <identifier> 2  → タスク2を実装                  │
│       ↓                                                     │
│  /review <identifier> 1     → レビュー                      │
│       ↓                                                     │
│  /testcase <identifier>     → テスト仕様書                  │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ バグ修正フロー                                               │
│                                                             │
│  /bugfix <identifier> <説明>                                │
│       ↓                                                     │
│  /implement <identifier>    → 実装                          │
│       ↓                                                     │
│  /review <identifier>       → レビュー                      │
│       ↓                                                     │
│  /testcase <identifier>     → テスト仕様書                  │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ 小規模改修フロー                                             │
│                                                             │
│  /feature <identifier> <説明>                               │
│       ↓                                                     │
│  /implement <identifier>    → 実装                          │
│       ↓                                                     │
│  /review <identifier>       → レビュー                      │
│       ↓                                                     │
│  /testcase <identifier>     → テスト仕様書                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 出力フォルダ構造

```
spec/
└── <identifier>/           # 例: SMP-123
    ├── requirement.md      # /requirement で生成（受け入れ条件込み）
    ├── design.md           # /design で生成
    ├── tasks.md            # /tasks で生成（対応UC紐付け）
    ├── feature.md          # /feature で生成（小規模改修）
    ├── bugfix.md           # /bugfix で生成
    └── testcase.md         # /testcase で生成
```

### 2.3 コマンド一覧

| コマンド | 形式 | 用途 |
|----------|------|------|
| `/requirement` | `/requirement <identifier> <説明>` | 要件定義書の作成 |
| `/design` | `/design <identifier> <説明>` | 詳細設計書の作成 |
| `/tasks` | `/tasks <identifier>` | タスク分割（対応UC紐付け） |
| `/feature` | `/feature <identifier> <説明>` | 小規模改修の仕様書作成 |
| `/bugfix` | `/bugfix <identifier> <説明>` | バグ修正仕様書の作成 |
| `/implement` | `/implement <identifier> [task-no]` | 実装 |
| `/review` | `/review <identifier> [task-no]` | レビュー |
| `/testcase` | `/testcase <identifier>` | テスト仕様書の作成 |

---

## 3. 機能開発フロー（詳細）

### 3.1 要件定義

```bash
/requirement SMP-123 記事検索機能の追加
```

**出力**: `spec/SMP-123/requirement.md`

- 機能概要（What / Why / Who）
- 用語定義
- ユースケース一覧（UC-001, UC-002...）
- ビジネスルール
- 受け入れ条件（UC単位で正常系・異常系）
- 非機能要件
- 確認事項

### 3.2 詳細設計

```bash
/design SMP-123 記事検索機能の API 設計
```

**出力**: `spec/SMP-123/design.md`

- 処理フロー（シーケンス図、処理ステップ）
- データ設計（該当する場合）
- API設計（該当する場合）
- 画面設計（該当する場合）
- 実装方針

### 3.3 タスク分割

```bash
/tasks SMP-123
```

**出力**: `spec/SMP-123/tasks.md`

- タスク一覧（対応UC紐付け）
- 依存関係

※ 受け入れ条件は `requirement.md` を参照

### 3.4 実装

```bash
/implement SMP-123 1    # タスク1を実装
/implement SMP-123 2    # タスク2を実装
```

参照ドキュメント:
- `tasks.md` の対応UC
- `requirement.md` の受け入れ条件・ビジネスルール
- `design.md` の API設計・データモデル

### 3.5 レビュー

```bash
/review SMP-123 1
```

### 3.6 テスト仕様書

```bash
/testcase SMP-123
```

**出力**: `spec/SMP-123/testcase.md`

- すべてのドキュメントからテストケースを抽出
- タスクごとにグループ化

---

## 4. バグ修正フロー（詳細）

### 4.1 仕様書作成

```bash
/bugfix SMP-456 記事詳細ページで日付が表示されない
```

**出力**: `spec/SMP-456/bugfix.md`

- 現象（発生している問題、期待する動作、再現手順）
- 原因分析（根本原因、問題箇所）
- 修正仕様（修正方針、Before/After）
- 受け入れ条件

### 4.2 実装

```bash
/implement SMP-456
```

### 4.3 レビュー & テスト

```bash
/review SMP-456
/testcase SMP-456
```

回帰テストを含むテスト仕様書が生成される。

---

## 5. 小規模改修フロー（詳細）

requirement → design → tasks のフローを経ずに、単体で小規模な改修を行う場合に使用。

### 5.1 仕様書作成

```bash
/feature SMP-789 お気に入りボタンの追加
```

**出力**: `spec/SMP-789/feature.md`

### 5.2 実装 & レビュー & テスト

```bash
/implement SMP-789
/review SMP-789
/testcase SMP-789
```

---

## 6. プロジェクト設定

### 6.1 ディレクトリ構成

```
project/
├── .github/
│   ├── copilot-instructions.md    # 共通指示
│   └── prompts/
│       ├── requirement.prompt.md  # 要件定義
│       ├── design.prompt.md       # 詳細設計
│       ├── tasks.prompt.md        # タスク分割
│       ├── feature.prompt.md      # 小規模改修
│       ├── bugfix.prompt.md       # バグ修正
│       ├── implement.prompt.md    # 実装
│       ├── review.prompt.md       # レビュー
│       └── testcase.prompt.md     # テスト仕様書
└── spec/
    ├── development-guide.md       # このファイルをdevelopment-guideとしてください
    └── <identifier>/              # 各チケットのドキュメント
```

### 6.2 共通設定

`.github/copilot-instructions.md` を作成し、プロジェクト固有のルールを記載する。

```markdown
# プロジェクト規約

## 技術スタック

- Backend: NestJS, Prisma, TypeScript
- Frontend: Next.js (App Router), React, TypeScript
- DB: PostgreSQL

## コーディング規約

- 変数・関数: camelCase
- クラス・型: PascalCase
- ファイル名: kebab-case
- 日本語コメント可

## テスト規約

- 単体テスト: Jest
- ファイル名: *.spec.ts
```

---

## 7. 効果的なプロンプトのコツ

1. **コンテキストを明示する**: 関連ファイルを `#file:` で指定
2. **出力形式を指定する**: 表、コードブロック、箇条書きなど
3. **制約を明記する**: 技術スタック、コーディング規約
4. **具体例を示す**: 期待する出力のサンプルを添える
5. **段階的に進める**: 一度に多くを求めず、ステップを分ける

---

## 8. 実践 Tips

### 8.1 段階的な導入

| Phase | 内容                                                |
| ----- | --------------------------------------------------- |
| 1     | プロンプトテンプレートを用意し、手動で活用          |
| 2     | copilot-instructions.md を整備、チームで標準化      |
| 3     | テンプレートの改善サイクルを回す                    |

### 8.2 人間が担うべき役割

- 要件の本質的な判断
- AI の出力レビュー
- セキュリティ・性能の最終確認
- ビジネス判断が必要な設計決定
- プロンプトテンプレートの改善

### 8.3 AI に任せるべき役割

- 定型的なコード生成
- テストコード作成
- ドキュメント生成
- コードレビューの一次チェック
- リファクタリング案の提示

---

## 更新履歴

| 日付       | 内容                           |
| ---------- | ------------------------------ |
| 2025-XX-XX | 初版作成                       |
| 2025-XX-XX | プロンプトコマンド体系に対応   |
