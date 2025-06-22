# my_mcp_sse_servers

## 概要

このリポジトリは、複数の MCP (Model Context Protocol) サーバーを管理し、Docker Compose を使用して簡単に起動するためのプロジェクトです。
各 MCP サーバーはサブモジュールとして管理されており、それぞれ特定の機能を提供します。

## 目的

- 複数の MCP サーバーを一元管理する。
- Docker Compose を利用して、開発環境や本番環境でのサーバー起動を容易にする。
- 各サーバーの設定と依存関係を明確にする。

## セットアップ

### 1. 環境変数の設定

各 MCP サーバーのディレクトリには `.env.sample` ファイルが含まれています。
これらのファイルを `.env` という名前でコピーし、必要な API キーや設定値を記述してください。

例:
```bash
cp brave-search-sse-mcp/.env.sample brave-search-sse-mcp/.env
cp mcp_grareco/.env.sample mcp_grareco/.env
# playwright-mcp-docker/.envのみ正常動作しないため、ルートディレクトリ直下にする
#cp playwright-mcp-docker/.env.sample playwright-mcp-docker/.env
cp playwright-mcp-docker/.env.sample .env
cp firecrawl-sse-mcp/.env.example firecrawl-sse-mcp/.env
cp note-mcp-server/.env.example note-mcp-server/.env
```

### 2. Docker Compose による起動

以下のコマンドを実行して、すべての MCP サーバーを起動します。

```bash
docker-compose up -d
```

これにより、`docker-compose.yml` に定義された各サービスがバックグラウンドで起動します。

## 起動中のサーバー

- **brave-search-sse**: Brave Search API を利用した Web 検索およびローカル検索機能を提供します。
- **mcp_grareco**: テキストや Web ページをグラフィックレコーディング風の HTML に変換する機能を提供します。
- **playwright-mcp-docker**: Playwright を使用してブラウザ操作を行う機能を提供します。
- **firecrawl-sse-mcp**: Firecrawl を利用して Web ページのスクレイピング、クローリング、検索を行う機能を提供します。

## 停止

サーバーを停止するには、以下のコマンドを実行します。

```bash
docker-compose down
```

## Roo-Code等のMCPクライアントでの設定

```json
{
  "mcpServers": {
    "brave-search-sse": {
      "url": "http://host.docker.internal:3101/sse",
      "headers": {},
      "timeout": 30,
      "disabled": false,
      "alwaysAllow": []
    },
    "playwright_sse": {
      "url": "http://host.docker.internal:3103/sse",
      "headers": {},
      "timeout": 30,
      "disabled": false,
      "alwaysAllow": [
      ]
    },
    "grareco": {
      "url": "http://host.docker.internal:3102/sse",
      "headers": {},
      "timeout": 900
    },
    "firecrawl": {
      "url": "http://host.docker.internal:3104/sse",
      "headers": {},
      "timeout": 30
    },
    "note_mcp": {
      "url": "http://host.docker.internal:3105/mcp",
      "type": "streamable-http",
      "headers": {},
      "timeout": 30
    }
  }
}
```
