# TypeScript Todo App 技術スタック

## 概要
- モノレポ構成（`backend/`, `frontend/`, `examples/`）
- TypeScript / ES Modules（`"type": "module"`）

## バックエンド
- 言語/実行: TypeScript, Node.js, tsx
- Webフレームワーク: Fastify, @fastify/cors
- 認証/認可: JSON Web Token（jsonwebtoken）
- バリデーション: Zod
- ORM/DB: Prisma, PostgreSQL
- テスト: Vitest, Supertest

## フロントエンド
- UI: React 18, React DOM
- 状態管理: Zustand
- データ取得: TanStack React Query
- ビルド: Vite
- CSS: Tailwind CSS, PostCSS, Autoprefixer
- テスト: Vitest, Testing Library, JSDOM
- E2E: Playwright

## 開発・品質
- 型チェック: TypeScript (`tsc`)
- Lint: ESLint
- フォーマット: Prettier
- 依存管理: npm（package-lock.json）

## インフラ/ローカル実行
- コンテナ: Docker Compose
- DBコンテナ: PostgreSQL 16（本番/テスト用 compose）

## 依存関係バージョン（主要）
### ルート（バックエンド/共通）
- fastify ^4.29.0
- @fastify/cors ^9.0.1
- @prisma/client ^5.22.0 / prisma ^5.22.0
- jsonwebtoken ^9.0.3
- zod ^3.24.1
- vitest ^2.1.8 / @vitest/coverage-v8 ^2.1.9
- typescript ^5.6.3
- eslint ^9.39.2 / prettier ^3.8.1
- tsx ^4.16.2

### フロントエンド
- react ^18.3.1 / react-dom ^18.3.1
- @tanstack/react-query ^5.59.10
- zustand ^4.5.5
- vite ^5.4.11 / @vitejs/plugin-react ^4.3.4
- tailwindcss ^3.4.15 / postcss ^8.4.49 / autoprefixer ^10.4.20
- vitest ^2.1.8
- @playwright/test ^1.55.0

## アーキテクチャ概要図（簡易）
```
[HTTP/Transport]
   Handlers (Fastify routes)
            |
         Usecases
            |
         Domain
 (entities / valueObjects / errors)
            |
      Repositories
            |
   Infrastructure (Prisma)
```

## レイヤードアーキテクチャの特徴（本プロジェクトで読み取れる点）
- ドメイン層が中心で、`domain/` にエンティティ・値オブジェクト・ドメインエラーが集約
- ユースケース層（`usecases/`）がアプリケーションの処理フローを担当
- ハンドラ層（`handlers/`）がHTTPリクエスト/レスポンスの変換と入力検証を担当
- リポジトリ層（`repositories/`）が永続化の抽象化を担い、インフラ層で実装
- 依存の向きが「外側 → 内側」になりやすく、ドメインがフレームワークに依存しにくい構造

## 実行手順（dev/test/e2e）
### DB 起動
```
# 開発用DB
npm run db:up

# テスト用DB
npm run db:test:up
```

### バックエンド起動（開発）
```
# ルートで実行（tsx に実行ファイルを渡す）
npm run dev -- backend/src/index.ts
```

### フロントエンド起動（開発）
```
cd frontend
npm install
npm run dev
```

### テスト実行
```
# ルート
npm test

# バックエンドのみ
npm run test:backend

# examples を含むテスト
npm run test:examples
```

### E2E テスト
```
cd frontend
npm run test:e2e
```

## 補足
- `examples/` に同様のバックエンド/フロントエンド構成のサンプルが含まれる
