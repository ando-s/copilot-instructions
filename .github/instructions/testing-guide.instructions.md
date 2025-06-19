---
applyTo: '**'
---

# テスト指針

## テスト戦略

### テストピラミッド
```
     ┌─────────────────┐
     │    E2E Tests    │ ← 少数、重要なユーザーシナリオ
     ├─────────────────┤
     │ Integration     │ ← 中程度、システム間連携
     │ Tests           │
     ├─────────────────┤
     │   Unit Tests    │ ← 多数、個別の機能・メソッド
     └─────────────────┘
```

### テストの種類と役割

#### Unit Tests（単体テスト）
- **目的**: 個別の関数・メソッド・クラスの動作確認
- **範囲**: 最小単位の機能
- **実行頻度**: 開発者が頻繁に実行
- **カバレッジ目標**: 80%以上

#### Integration Tests（統合テスト）
- **目的**: モジュール間の連携確認
- **範囲**: API、データベース連携、外部サービス連携
- **実行頻度**: CI/CDパイプラインで自動実行
- **確認項目**: データフロー、エラーハンドリング

#### E2E Tests（エンドツーエンドテスト）
- **目的**: ユーザーシナリオの動作確認
- **範囲**: アプリケーション全体
- **実行頻度**: リリース前、定期実行
- **確認項目**: 主要な業務フロー

## TDD（テスト駆動開発）

### Red-Green-Refactorサイクル
1. **Red**: 失敗するテストを書く
2. **Green**: テストを通す最小限の実装
3. **Refactor**: コードの品質を向上

### TDDのメリット
- 要件の明確化
- リファクタリングの安全性
- 設計品質の向上
- ドキュメントとしての価値

### 実践のポイント
- 小さなステップで進める
- テストファーストを徹底
- 最小限の実装から始める
- リファクタリングを怠らない

## テスト実装ガイド

### 単体テストの書き方

#### JavaScript/TypeScript（Jest）
```javascript
// 例：計算機能のテスト
describe('Calculator', () => {
  let calculator;

  beforeEach(() => {
    calculator = new Calculator();
  });

  describe('add', () => {
    it('should return sum of two positive numbers', () => {
      // Arrange
      const a = 2;
      const b = 3;

      // Act
      const result = calculator.add(a, b);

      // Assert
      expect(result).toBe(5);
    });

    it('should handle negative numbers', () => {
      expect(calculator.add(-1, 1)).toBe(0);
    });

    it('should throw error for non-numeric inputs', () => {
      expect(() => calculator.add('a', 1)).toThrow();
    });
  });
});
```

#### Python（pytest）
```python
# 例：計算機能のテスト
import pytest
from calculator import Calculator

class TestCalculator:
    def setup_method(self):
        self.calculator = Calculator()

    def test_add_positive_numbers(self):
        # Arrange
        a, b = 2, 3

        # Act
        result = self.calculator.add(a, b)

        # Assert
        assert result == 5

    def test_add_negative_numbers(self):
        assert self.calculator.add(-1, 1) == 0

    def test_add_invalid_input(self):
        with pytest.raises(TypeError):
            self.calculator.add('a', 1)
```

### 統合テストの書き方

#### API テスト
```javascript
// Express.js APIのテスト例
describe('User API', () => {
  it('should create new user', async () => {
    const userData = {
      name: 'Test User',
      email: 'test@example.com'
    };

    const response = await request(app)
      .post('/api/users')
      .send(userData)
      .expect(201);

    expect(response.body.name).toBe(userData.name);
    expect(response.body.email).toBe(userData.email);
  });
});
```

#### データベーステスト
```python
# Django ORMのテスト例
from django.test import TestCase
from myapp.models import User

class UserModelTest(TestCase):
    def test_create_user(self):
        user = User.objects.create(
            name='Test User',
            email='test@example.com'
        )

        self.assertEqual(user.name, 'Test User')
        self.assertTrue(user.email, 'test@example.com')
```

### E2Eテストの書き方

#### Playwright（JavaScript）
```javascript
// E2Eテストの例
const { test, expect } = require('@playwright/test');

test('user login flow', async ({ page }) => {
  // ログインページに移動
  await page.goto('/login');

  // 認証情報を入力
  await page.fill('#email', 'user@example.com');
  await page.fill('#password', 'password123');

  // ログインボタンをクリック
  await page.click('#login-button');

  // ダッシュボードに遷移することを確認
  await expect(page).toHaveURL('/dashboard');
  await expect(page.locator('h1')).toContainText('Dashboard');
});
```

## テストデータ管理

### テストデータの原則
- **独立性**: テスト間でデータを共有しない
- **再現性**: 同じ条件で同じ結果を得る
- **クリーンアップ**: テスト後にデータを削除
- **現実性**: 実際のデータに近い内容

### ファクトリーパターン
```javascript
// テストデータファクトリーの例
class UserFactory {
  static create(overrides = {}) {
    return {
      id: Math.random(),
      name: 'Test User',
      email: 'test@example.com',
      createdAt: new Date(),
      ...overrides
    };
  }

  static createAdmin(overrides = {}) {
    return this.create({
      role: 'admin',
      permissions: ['read', 'write', 'delete'],
      ...overrides
    });
  }
}
```

### モック・スタブの活用
```javascript
// 外部APIのモック例
jest.mock('./api-client', () => ({
  fetchUserData: jest.fn(() =>
    Promise.resolve({
      id: 1,
      name: 'Mock User'
    })
  )
}));
```

## パフォーマンステスト

### 負荷テスト
- 想定負荷での動作確認
- レスポンス時間の測定
- リソース使用量の監視

### ストレステスト
- 限界値での動作確認
- システムの破綻点の特定
- 復旧能力の検証

### ツール
- **K6**: API負荷テスト
- **JMeter**: 総合的な性能テスト
- **Lighthouse**: フロントエンドパフォーマンス

## セキュリティテスト

### 脆弱性テスト
- SQLインジェクション
- XSS（クロスサイトスクリプティング）
- CSRF（クロスサイトリクエストフォージェリ）

### 認証・認可テスト
- 不正アクセスの防止確認
- 権限制御の動作確認
- セッション管理の検証

## テスト自動化

### CI/CDパイプライン統合
```yaml
# GitHub Actions例
name: Test
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm install
      - name: Run unit tests
        run: npm test
      - name: Run integration tests
        run: npm run test:integration
      - name: Run E2E tests
        run: npm run test:e2e
```

### テストレポート
- カバレッジレポートの生成
- テスト結果の可視化
- 失敗したテストの詳細情報

## 品質メトリクス

### テストカバレッジ
- **行カバレッジ**: 実行された行の割合
- **分岐カバレッジ**: 実行された分岐の割合
- **関数カバレッジ**: 実行された関数の割合

### 品質指標
- テスト実行時間
- テスト成功率
- バグ検出率
- リリース品質
