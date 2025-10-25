# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

このプロジェクトは、Rails 8.1をベースとしたモダンなWebアプリケーションのテンプレートです。

**主要な技術スタック:**
- **Ruby:** 3.4.7
- **Rails:** 8.1
- **フロントエンド:** Vite + TypeScript + Stimulus
- **データベース:** SQLite3（開発環境・本番環境）
- **バックグラウンドジョブ:** Solid Queue
- **キャッシュ:** Solid Cache
- **WebSocket:** Solid Cable
- **テスト:** RSpec + Capybara + Playwright

## セットアップ

```bash
# 初回セットアップ
bin/setup

# 開発サーバーの起動（Rails + Vite）
bin/dev
```

`bin/dev` は `Procfile.dev` で定義されたプロセスを起動します:
- `web`: Rails サーバー (Puma)
- `vite`: Vite 開発サーバー（ホットリロード対応）

## よく使用するコマンド

### テスト実行

```bash
# 全テストを実行
bundle exec rspec

# 特定のファイルをテスト
bundle exec rspec spec/system/root_spec.rb

# 特定の行番号のテストを実行
bundle exec rspec spec/system/root_spec.rb:4
```

### Lint & フォーマット

```bash
# Ruby: RuboCop
bin/rubocop

# Ruby: RuboCop で自動修正
bin/rubocop -A

# JavaScript/TypeScript: Biome
npx biome check .

# JavaScript/TypeScript: Biome で自動修正
npx biome check --apply .
```

### CI実行

```bash
# CI全体を実行（セットアップ、Lint、テスト）
bin/ci
```

`bin/ci` は以下を実行します:
1. `bin/setup --skip-server` - セットアップ
2. `bin/rubocop` - Rubyコードスタイルチェック
3. `bin/importmap audit` - セキュリティ監査
4. `bin/rails rake` - テスト実行
5. `env RAILS_ENV=test bin/rails db:seed:replant` - Seedデータのテスト

### データベース

```bash
# マイグレーション実行
bin/rails db:migrate

# テストDBのセットアップ
bin/rails db:test:prepare

# Seedデータ投入
bin/rails db:seed
```

## アーキテクチャ

### フロントエンド構成

**Vite + TypeScript + Stimulus**

- **エントリーポイント:** `app/javascript/entrypoints/application.ts`
- **Stimulusコントローラー:** `app/javascript/controllers/`
- **スタイル:** `app/javascript/entrypoints/application.css`

**Vite設定のポイント:**
- `vite.config.ts` で設定
- HMR（ホットモジュールリプレースメント）対応
- `config/routes.rb` や `app/views/**/*` の変更時に自動リロード

**Stimulusコントローラーの作成:**

新しいコントローラーを作成する際は、`app/javascript/controllers/` に TypeScript ファイルを作成し、`app/javascript/controllers/index.ts` でエクスポートします。

### バックエンド構成

**Rails 8.1 + Solid Stack**

- **Solid Queue:** バックグラウンドジョブ処理（`config/queue.yml`, `db/queue_schema.rb`）
- **Solid Cache:** キャッシュストア（`config/cache.yml`, `db/cache_schema.rb`）
- **Solid Cable:** WebSocket通信（`config/cable.yml`, `db/cable_schema.rb`）

本番環境では、複数のSQLiteデータベースを使用:
- `storage/production.sqlite3` - メインDB
- `storage/production_cache.sqlite3` - キャッシュ用
- `storage/production_queue.sqlite3` - ジョブキュー用
- `storage/production_cable.sqlite3` - WebSocket用

### テスト構成

**RSpec + Capybara + Playwright**

- **システムテスト:** `spec/system/` - Playwright ドライバーを使用
- **テストヘルパー:** `spec/rails_helper.rb`, `spec/spec_helper.rb`
- **サポートファイル:** `spec/support/` に配置

**システムテストの実行:**
Playwright を使用した E2E テストが `spec/system/` に配置されています。Playwright は Dockerfile で依存関係がインストールされています。

### コーディング規約

**Ruby:**
- RuboCop で管理（`.rubocop.yml`）
- `bin/rubocop` でチェック実行

**JavaScript/TypeScript:**
- Biome で管理（`biome.json`）
- シングルクォート、セミコロン省略（as needed）、トレーリングカンマあり
- インデント: 2スペース
- 行幅: 120文字

## プロジェクト固有の注意事項

1. **dotenv使用:** 環境変数は `.env` ファイルで管理（開発/テスト環境）
2. **Importmap:** JavaScript依存関係の一部はImportmapで管理
3. **Propshaft:** アセットパイプラインとしてPropshaftを使用（Sprocketsではない）
4. **DevContainer対応:** `.devcontainer/` でVS Code Dev Containersに対応
