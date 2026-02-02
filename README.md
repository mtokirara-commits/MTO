# MTO配車システム - 即起動版

## 構成

Next.js App Routerベースの即起動型配車管理アプリケーション

### ディレクトリ構造

```
mto-dispatch/
├── app/
│   ├── layout.tsx          # ルートレイアウト + AuthProvider
│   ├── page.tsx            # メインUI（即起動）
│   └── globals.css         # Tailwindスタイル
├── lib/
│   ├── dispatch-types.ts   # 型定義
│   ├── data-store.ts       # データストア + localStorage
│   ├── dispatch-logic.ts   # 配車ロジック
│   └── auth-context.tsx    # 認証コンテキスト
├── package.json
├── tsconfig.json
├── next.config.js
├── tailwind.config.ts
└── postcss.config.js
```

## セットアップ

### 1. 依存パッケージインストール

```bash
cd mto-dispatch
npm install
```

### 2. 開発サーバー起動

```bash
npm run dev
```

ブラウザで `http://localhost:3000` を開くと即座にアプリが起動します。

### 3. Replit Deployの場合

1. Replitで `Run` ボタンをクリック
2. Preview URLを開く
3. アプリが即座に起動

## 機能

### 即起動
- URLアクセス時点でアプリUI表示
- 手動操作不要
- ダミー画面なし

### タブ構成
1. **配車管理**: 待機中キャスト、配車便一覧
2. **承認**: 承認待ち・処理済み承認リスト
3. **通知**: 未読・既読通知管理

### データ永続化
- localStorage使用
- dataStore.subscribe() による自動更新
- オフライン検知

### 既存ロジック保持
- 深夜制御（0-6時の承認必須）
- 排他制御（mutex）
- 二重実行防止
- トランザクション処理

## 技術スタック

- **Next.js 14** (App Router)
- **React 18**
- **TypeScript**
- **Tailwind CSS**
- **localStorage** (データ永続化)

## 実装済み安全装置

✓ approveApproval 二重実行防止（mutex）
✓ approval_result を requestedBy.userId に通知
✓ updateApproval の pending ガード
✓ getApprovals の当日限定フィルタ
✓ 深夜ブロック（recalc/confirm/cancel）
✓ オフライン検知と操作拒否
✓ 再計算中の confirmDispatch ブロック
✓ トランザクション失敗時ロールバック

## 注意事項

- 初回起動時、配車担当ユーザーで自動ログイン
- マスターデータは window.MTO_ADMIN_API から登録
- 日跨ぎは進行中便がない場合のみ実行可能
