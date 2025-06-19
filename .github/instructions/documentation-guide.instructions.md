# ドキュメント規約

## ドキュメント作成の基本原則

### 目的の明確化
- **読み手を意識**: 技術者、エンドユーザー、管理者など
- **目的の明示**: 理解、実装、運用、トラブルシュート
- **情報の整理**: 必要な情報を適切に構造化

### 品質の確保
- **正確性**: 最新の情報を反映
- **完全性**: 必要な情報をすべて含む
- **一貫性**: 用語・形式の統一
- **簡潔性**: 冗長な表現を避ける

## ドキュメントの種類

### README.md
プロジェクトの入り口となる重要なドキュメント

#### 必須項目
```markdown
# プロジェクト名

## 概要
プロジェクトの目的と機能の概要

## 機能
- 主要機能1
- 主要機能2
- 主要機能3

## 必要要件
- Node.js 16.x以上
- PostgreSQL 13.x以上
- Redis 6.x以上

## インストール
```bash
git clone https://github.com/user/project.git
cd project
npm install
```

## 使用方法
基本的な使用方法の説明

## API文書
API仕様書へのリンク

## 貢献
コントリビューション方法

## ライセンス
MIT License
```

### API仕様書
開発者向けのAPI使用方法

#### OpenAPI（Swagger）形式推奨
```yaml
openapi: 3.0.0
info:
  title: User Management API
  version: 1.0.0
  description: ユーザー管理システムのAPI

paths:
  /users:
    get:
      summary: ユーザー一覧取得
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                type: object
                properties:
                  users:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'
```

### 設計書
システムの構造と設計思想

#### アーキテクチャ文書
```markdown
# システムアーキテクチャ

## 概要
システム全体の構成と各コンポーネントの役割

## コンポーネント図
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Frontend    │◄──►│ Backend     │◄──►│ Database    │
│ (React)     │    │ (Node.js)   │    │ (PostgreSQL)│
└─────────────┘    └─────────────┘    └─────────────┘
```

## データフロー
リクエストの処理フローと データの流れ

## 技術選択理由
各技術を選択した理由と代替案の検討結果
```

### 運用マニュアル
システムの運用・保守方法

#### デプロイ手順
```markdown
# デプロイ手順

## 本番環境デプロイ

### 前提条件
- デプロイ権限があること
- 必要な環境変数が設定済み
- データベースマイグレーション済み

### 手順
1. リリースブランチの作成
```bash
git checkout -b release/v1.2.0
```

2. バージョンの更新
```bash
npm version patch
```

3. 本番環境へのデプロイ
```bash
npm run deploy:production
```

4. 動作確認
- ヘルスチェックエンドポイント確認
- 主要機能の動作確認
- ログの確認

### ロールバック手順
問題が発生した場合の復旧手順
```

## コードドキュメント

### インラインコメント
```javascript
/**
 * ユーザー情報を取得する
 * @param {string} userId - ユーザーID
 * @param {Object} options - オプション設定
 * @param {boolean} options.includeProfile - プロフィール情報を含むか
 * @returns {Promise<Object>} ユーザー情報
 * @throws {NotFoundError} ユーザーが見つからない場合
 */
async function getUser(userId, options = {}) {
  // 入力値の検証
  if (!userId || typeof userId !== 'string') {
    throw new Error('Invalid userId');
  }

  // データベースからユーザー情報を取得
  const user = await User.findById(userId);
  
  if (!user) {
    throw new NotFoundError(`User not found: ${userId}`);
  }

  // プロフィール情報を含める場合
  if (options.includeProfile) {
    user.profile = await Profile.findByUserId(userId);
  }

  return user;
}
```

### 関数・クラスの説明
```python
class UserService:
    """
    ユーザー管理を行うサービスクラス
    
    このクラスはユーザーの作成、更新、削除などの
    ビジネスロジックを担当します。
    
    Attributes:
        db_session: データベースセッション
        logger: ログ出力用オブジェクト
    """
    
    def __init__(self, db_session, logger):
        """
        UserServiceの初期化
        
        Args:
            db_session: データベースセッション
            logger: ログ出力用オブジェクト
        """
        self.db_session = db_session
        self.logger = logger
    
    def create_user(self, user_data):
        """
        新しいユーザーを作成する
        
        Args:
            user_data (dict): ユーザー情報
                - name (str): ユーザー名
                - email (str): メールアドレス
                - password (str): パスワード
        
        Returns:
            User: 作成されたユーザーオブジェクト
        
        Raises:
            ValidationError: 入力データが不正な場合
            DuplicateError: 既に存在するメールアドレスの場合
        """
```

## ドキュメント管理

### バージョン管理
- ドキュメントもソースコードと同じリポジトリで管理
- 機能追加・変更時は関連ドキュメントも更新
- レビュープロセスにドキュメント確認を含める

### 更新タイミング
- **機能実装時**: 設計書、API仕様書の更新
- **リリース時**: README、ユーザーガイドの更新
- **障害対応時**: トラブルシュート手順の追加
- **定期レビュー**: 月次での内容確認と更新

### ファイル構成
```
docs/
├── README.md                    # プロジェクト概要
├── api/
│   ├── openapi.yaml            # API仕様書
│   └── authentication.md      # 認証方法
├── architecture/
│   ├── overview.md             # システム概要
│   ├── database.md             # データベース設計
│   └── security.md             # セキュリティ設計
├── deployment/
│   ├── production.md           # 本番環境構築
│   ├── staging.md              # ステージング環境
│   └── troubleshooting.md      # トラブルシュート
└── user-guide/
    ├── getting-started.md      # 初心者向けガイド
    ├── advanced-features.md    # 高度な機能
    └── faq.md                  # よくある質問
```

## 執筆ガイドライン

### Markdown記法
- 見出しは階層構造を明確に
- コードブロックには言語指定
- リンクは意味のあるテキストを使用
- 画像にはalt属性を設定

### 文体・用語
- 丁寧語で統一（「です・ます」調）
- 技術用語は統一（用語集を作成）
- 略語の初出時は正式名称を併記
- 読み手のレベルに応じた説明

### 構造化
- 目次を設ける（長いドキュメント）
- 段落は短く、要点を明確に
- 箇条書きを効果的に活用
- 図表を使って視覚的に説明

## ツールと環境

### ドキュメント生成
- **JSDoc**: JavaScript/TypeScriptのAPI文書
- **Sphinx**: Pythonのドキュメント生成
- **GitBook**: 総合的なドキュメントサイト
- **Docusaurus**: React製のドキュメントサイト

### 図表作成
- **Draw.io**: フローチャート、システム図
- **PlantUML**: テキストベースのUML図
- **Mermaid**: GitHubで表示可能な図表

### レビューとフィードバック
- プルリクエストでのレビュー
- コメント機能の活用
- 定期的な読み合わせ会
- ユーザーフィードバックの収集

## 品質チェックポイント

### 内容の正確性
- [ ] 最新の実装と一致している
- [ ] サンプルコードが動作する
- [ ] 外部リンクが有効である
- [ ] スクリーンショットが最新である

### 読みやすさ
- [ ] 目的が明確に記載されている
- [ ] 手順が順序立てて説明されている
- [ ] 専門用語に説明がある
- [ ] 図表が効果的に使われている

### 保守性
- [ ] 更新しやすい構造になっている
- [ ] 責任者が明確になっている
- [ ] 更新履歴が残されている
- [ ] 関連文書との整合性がある
