# dialy (日記アプリケーション)

## 1. プロジェクト概要
ユーザーが日々の出来事や感情を記録し、後から振り返ることができるシンプルな日記アプリケーション。

## 2. 技術スタック
- **言語**: Java 21
- **フレームワーク**: Spring Boot 3.x (最新版)
- **ビルドツール**: Maven
- **データベース**: H2 Database (組み込み)
- **ビュー**: Thymeleaf
- **テスト**: JUnit 5, Mockito, AssertJ

## 3. ディレクトリ構成 (パッケージ構成)
- `com.example.dialy.controller`: コントローラー層
- `com.example.dialy.service`: サービス層 (ビジネスロジック)
- `com.example.dialy.repository`: リポジトリ層 (DBアクセス)
- `com.example.dialy.model`: エンティティ・ドメインモデル
- `com.example.dialy.dto`: データ転送オブジェクト
- `com.example.dialy.exception`: カスタム例外定義

## 4. ドキュメント管理
- プロジェクトの仕様書などは `specs/` ディレクトリに小文字のファイル名で管理する。
- テストデータなどの定義は `specs/test_data.md` を参照すること。
