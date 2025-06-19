---
applyTo: "**/*.test.ts,**/*.test.tsx,**/*.spec.ts,**/*.spec.tsx"
---

# テスト作成規約

## テスト駆動開発（TDD）の原則

### 1. TDDサイクル
1. **Red**: まず失敗するテストを書く
2. **Green**: テストを通すための最小限のコードを書く
3. **Refactor**: コードをリファクタリングして改善する

### 2. テストファースト
- 機能実装前に、期待する動作を明確にするテストを作成する
- テストを通じて仕様を明確化する
- 実装はテストを満たすために行う

## テストコード記述規約

### 1. テストの構造
- `describe` / `it` ブロックで階層的に整理する
- テストの説明は「何をテストしているか」を明確に記述する
- AAA（Arrange, Act, Assert）パターンに従う

```typescript
describe('ユーザー管理機能', () => {
  describe('ユーザー作成', () => {
    it('有効なデータでユーザーを作成できること', () => {
      // Arrange: テストデータの準備
      const userData = { name: '山田太郎', email: 'yamada@example.com' };
      
      // Act: 実際の処理を実行
      const user = createUser(userData);
      
      // Assert: 結果の検証
      expect(user).toHaveProperty('id');
      expect(user.name).toBe('山田太郎');
      expect(user.email).toBe('yamada@example.com');
    });
  });
});
```

### 2. React コンポーネントのテスト
- `@testing-library/react` を使用してユーザー視点のテストを記述する
- 実装詳細ではなく、ユーザーの操作や見た目の振る舞いをテストする
- `screen` を使用して要素を取得する

```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

describe('LoginForm', () => {
  it('有効な情報を入力してログインできること', async () => {
    // Arrange
    const mockOnLogin = jest.fn();
    render(<LoginForm onLogin={mockOnLogin} />);
    const user = userEvent.setup();
    
    // Act
    await user.type(screen.getByLabelText('メールアドレス'), 'test@example.com');
    await user.type(screen.getByLabelText('パスワード'), 'password123');
    await user.click(screen.getByRole('button', { name: 'ログイン' }));
    
    // Assert
    expect(mockOnLogin).toHaveBeenCalledWith({
      email: 'test@example.com',
      password: 'password123'
    });
  });
});
```

### 3. 外部依存のモック化
- 外部APIやモジュールは `jest.mock()` を使ってモック化する
- モックの戻り値を適切に設定する
- テスト間でモックをリセットする

```typescript
// 外部モジュールのモック
jest.mock('../services/api');
const mockApi = api as jest.Mocked<typeof api>;

describe('UserService', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });
  
  it('APIからユーザー情報を取得できること', async () => {
    // Arrange
    const mockUser = { id: '1', name: '山田太郎' };
    mockApi.getUser.mockResolvedValue(mockUser);
    
    // Act
    const user = await userService.fetchUser('1');
    
    // Assert
    expect(user).toEqual(mockUser);
    expect(mockApi.getUser).toHaveBeenCalledWith('1');
  });
});
```

## テストケース設計

### 1. 正常系・異常系の網羅
- **正常系**: 期待通りの動作をするケース
- **異常系**: エラーや例外が発生するケース
- **境界値**: 最小値、最大値、境界近辺の値

### 2. テストケースの命名
- 日本語で「何をテストしているか」を明確に記述する
- 「〜すること」「〜できること」「〜の場合」などの表現を使用

```typescript
// Good
it('空のメールアドレスでバリデーションエラーが発生すること', () => { });
it('最大文字数を超えた場合にエラーメッセージが表示されること', () => { });

// Bad
it('should work', () => { });
it('test email validation', () => { });
```

## パフォーマンステスト

### 1. レンダリング性能
- 大量のデータを扱うコンポーネントのレンダリング時間を測定
- 不要な再レンダリングが発生していないかを確認

### 2. メモ化の効果測定
- React.memo、useMemo、useCallbackの効果を測定
- パフォーマンス改善前後の比較

## テストカバレッジ

### 1. カバレッジ目標
- **行カバレッジ**: 最低80%を目標
- **分岐カバレッジ**: 最低70%を目標
- **関数カバレッジ**: 90%以上を目標

### 2. カバレッジの確認
- テスト実行時にカバレッジレポートを生成
- 未カバーの重要な処理は追加テストを作成

## 継続的テスト

### 1. テストの実行タイミング
- コミット前に全テストを実行
- CI/CDパイプラインでの自動テスト実行
- プルリクエスト時のテスト結果確認

### 2. テストの保守
- 仕様変更時はテストも合わせて更新
- 不安定なテスト（フレイキーテスト）は早急に修正
- 古くなったテストは定期的に見直し
