# GitHub Copilot Instructions

## 📝 概要

このプロジェクトは、**GitHub Copilot**の開発支援機能を最大限活用するための設定・ルール管理リポジトリです。プロジェクト開発において頻繁に行われるタスクを自動化・標準化し、開発効率と品質を向上させるための各種instructionsルールを提供します。

## 🎯 プロジェクトの目的

- **開発効率の向上**: 反復的なタスクの自動化により開発時間を短縮
- **品質の標準化**: 一貫したコーディング規約とプロセスの確立
- **知識の共有**: ベストプラクティスをルール化し、チーム全体で共有
- **新メンバーのオンボーディング支援**: 標準化されたプロセスによる学習コストの削減

## 🚀 主要機能

### 1. 🛠️ 基本的な開発ルール

**適用対象**: 全ファイル（`**`）

プロジェクト全体に適用される基本的な開発ルールとベストプラクティスを定義します。

- 並列処理最適化
- エラーハンドリング方針
- ファイル管理規約
- 段階的実装アプローチ

### 2. 📋 ドキュメント作成支援

**適用対象**: Markdownファイル（`**/*.md`）

要件定義書や設計書などのドキュメント作成を支援します。

- 要件定義プロセス
- ドキュメント構成テンプレート
- 品質基準の明確化

### 3. 🏗️ TypeScript・React開発規約

**適用対象**: TypeScript・JavaScriptファイル（`**/*.ts,**/*.tsx,**/*.js,**/*.jsx`）

TypeScriptとReactを使用した開発における具体的なコーディング規約を定義します。

- 型定義のベストプラクティス
- Reactコンポーネント設計指針
- パフォーマンス最適化
- エラーハンドリング

### 4. ✅ テスト作成規約

**適用対象**: テストファイル（`**/*.test.ts,**/*.test.tsx,**/*.spec.ts,**/*.spec.tsx`）

TDD（テスト駆動開発）に基づくテスト作成のガイドラインを提供します。

- TDDサイクルの実践
- React Testing Libraryの使用方法
- モック化の方針
- テストカバレッジ目標

## 📁 プロジェクト構造

```
copilot-instructions/
├── README.md                                    # このファイル
└── .github/
    └── instructions/                            # GitHub Copilot instructionsファイル群
        ├── general.instructions.md              # 基本的な開発ルール（全ファイル対象）
        ├── documentation.instructions.md        # ドキュメント作成支援（Markdown対象）
        ├── typescript-react.instructions.md     # TypeScript・React開発規約
        └── testing.instructions.md             # テスト作成規約
```

## 🔧 セットアップ方法

### 1. GitHub Copilotの有効化
- VS Code拡張機能「GitHub Copilot」をインストール
- GitHub Copilotにログイン

### 2. Instructionsファイルの配置
このリポジトリの`.github/instructions/`フォルダが、プロジェクトのルートディレクトリに配置されていることを確認してください。

### 3. VS Codeの設定確認
VS Code v1.100.0以降で複数のinstructionsファイルがサポートされています。最新バージョンの使用を推奨します。

## 📖 使用方法

### 自動適用
各instructionsファイルは、指定されたファイルパターンに対して自動的に適用されます：

- **一般的な開発**: 全てのファイルで基本ルールが適用
- **ドキュメント作成**: `.md`ファイルで要件定義支援が適用
- **TypeScript開発**: `.ts,.tsx,.js,.jsx`ファイルでTypeScript・React規約が適用
- **テスト作成**: `.test.*,.spec.*`ファイルでテスト作成規約が適用

### 活用例

```plaintext
# TypeScriptコンポーネント作成時
GitHub Copilotが自動的にTypeScript・React開発規約に従った提案を行います

# テストファイル作成時
TDDの原則に基づいたテストコードを提案します

# ドキュメント作成時
構造化された要件定義書のテンプレートを提案します
```

## ✨ GitHub Copilotとの効果的な連携

### 1. コンテキストの活用
GitHub Copilotは現在開いているファイルとinstructionsの内容を組み合わせて提案を行います。より良い提案を得るために：

- 関連ファイルを同時に開く
- 明確な変数名と関数名を使用する
- コメントで意図を明確に記述する

### 2. 段階的な開発
- 小さな単位で機能を実装し、都度テストを実行
- TDDサイクル（Red → Green → Refactor）を意識
- 各段階でGitHub Copilotの提案を活用

### 3. レビューと改善
- GitHub Copilotの提案をそのまま使用せず、必ずレビューする
- プロジェクト固有の要件に合わせて調整
- 定期的にinstructionsファイルの見直しと改善

## 🤝 貢献方法

このプロジェクトへの貢献を歓迎します：

1. **Issues**: 問題や改善提案をIssueで報告
2. **Pull Requests**: 新しいinstructionsルールや改善の提案
3. **フィードバック**: 実際の使用経験に基づくフィードバック

## 📚 関連リンク

- [GitHub Copilot公式ドキュメント](https://docs.github.com/en/copilot)
- [VS Code GitHub Copilot拡張機能](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
- [GitHub Copilot Instructions設定方法](https://code.visualstudio.com/docs/copilot/copilot-customization#_use-instructionsmd-files)

---

**Note**: このプロジェクトはオープンソースで提供されており、MIT Licenseの下で自由に使用・改変可能です。
