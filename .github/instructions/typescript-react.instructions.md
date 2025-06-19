---
applyTo: "**/*.ts,**/*.tsx,**/*.js,**/*.jsx"
---

# TypeScript・React 開発規約

## TypeScript コーディング規約

### 1. 型定義
- 型定義は interface または type で明確に定義する
- Any型の使用は避け、可能な限り具体的な型を指定する
- Union型やGenerics型を適切に活用する

```typescript
// Good
interface User {
  id: string;
  name: string;
  email: string;
}

// Bad
const user: any = { id: 1, name: "John" };
```

### 2. 関数定義
- 関数の引数と戻り値の型を明示する
- アロー関数と通常の関数定義を使い分ける
- 純粋関数を推奨し、副作用を最小限に抑える

```typescript
// Good
const calculateTotal = (items: Item[]): number => {
  return items.reduce((sum, item) => sum + item.price, 0);
};

// Bad
function calculate(items) {
  let total = 0;
  // ...
  return total;
}
```

## React コーディング規約

### 1. コンポーネント設計
- 関数コンポーネントとフックを使用する（クラスコンポーネントは使用しない）
- Props は TypeScript の interface または type で定義する
- コンポーネントは単一責任の原則に従う

```tsx
// Good
interface ButtonProps {
  children: React.ReactNode;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
}

const Button: React.FC<ButtonProps> = ({ children, onClick, variant = 'primary' }) => {
  return (
    <button className={`btn btn-${variant}`} onClick={onClick}>
      {children}
    </button>
  );
};
```

### 2. 状態管理
- useState、useEffect、useContext等のフックを適切に使用する
- 複雑な状態管理にはuseReducerやZustand/Reduxを検討する
- 状態の更新は不変性を保つ

```tsx
// Good
const [user, setUser] = useState<User | null>(null);

const updateUser = (newUser: User) => {
  setUser(prevUser => ({ ...prevUser, ...newUser }));
};

// Bad
const updateUser = (newUser: User) => {
  user.name = newUser.name; // 直接変更は避ける
  setUser(user);
};
```

### 3. イベントハンドリング
- イベントハンドラーの型を明確に指定する
- useCallbackを使用してパフォーマンスを最適化する

```tsx
// Good
const handleSubmit = useCallback((event: React.FormEvent<HTMLFormElement>) => {
  event.preventDefault();
  // 処理
}, [dependency]);

// Bad
const handleSubmit = (event) => {
  // 処理
};
```

## パフォーマンス最適化

### 1. コンポーネントの最適化
- React.memo()を使用して不要な再レンダリングを防ぐ
- useMemo()やuseCallback()を適切に使用する
- 重い計算処理はuseMemo()でメモ化する

### 2. バンドルサイズの最適化
- 必要なモジュールのみをインポートする
- Dynamic importを使用して遅延読み込みを実装する
- Tree shakingを意識したコード記述

## エラーハンドリング

### 1. Error Boundary
- エラーが発生する可能性のあるコンポーネントをError Boundaryで包む
- ユーザーにとって分かりやすいエラーメッセージを表示する

### 2. 非同期処理のエラーハンドリング
- async/awaitでのエラーハンドリングにはtry-catch文を使用する
- Promiseの場合は.catch()を適切に使用する

```tsx
// Good
const fetchUser = async (id: string): Promise<User> => {
  try {
    const response = await api.getUser(id);
    return response.data;
  } catch (error) {
    console.error('ユーザー取得エラー:', error);
    throw error;
  }
};
```
