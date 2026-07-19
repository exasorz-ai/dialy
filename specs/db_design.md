# データベース設計: dialy

## 1. ER図 (概念)
`users` (1) -> (N) `diaries` (N) -> (M) `tags`
- `users` が中心となるエンティティ。
- `diaries` はユーザーに紐付く。
- `tags` は中間テーブル `diary_tags` を介して日記と多対多で紐付く。

## 2. テーブル定義

### 2.1 `users` (ユーザー)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto-inc | ユーザーの一意識別子 |
| `username` | VARCHAR(50) | Unique, Not Null | 表示名 |
| `email` | VARCHAR(255) | Unique, Not Null | ログイン用メールアドレス |
| `password` | VARCHAR(255) | Not Null | ハッシュ化されたパスワード |
| `created_at` | TIMESTAMP | Not Null, Default NOW() | 登録日時 |
| `updated_at` | TIMESTAMP | Not Null, Default NOW() | プロフィール最終更新日時 |

### 2.2 `diaries` (日記)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto-inc | 日記の一意識別子 |
| `user_id` | BIGINT | FK (users.id), Not Null | 投稿ユーザー |
| `title` | VARCHAR(100) | Not Null | 日記のタイトル |
| `content` | TEXT | Not Null | 日記の本文 |
| `mood` | VARCHAR(20) | - | 気分 (例: Happy, Sad) |
| `entry_date` | DATE | Not Null | 日記の対象日付 |
| `created_at` | TIMESTAMP | Not Null, Default NOW() | システム登録日時 |
| `updated_at` | TIMESTAMP | Not Null, Default NOW() | システム最終更新日時 |

### 2.3 `tags` (タグ)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `id` | BIGINT | PK, Auto-inc | タグの一意識別子 |
| `name` | VARCHAR(50) | Unique, Not Null | タグ名 |

### 2.4 `diary_tags` (日記-タグ中間テーブル)
| カラム名 | 型 | 制約 | 説明 |
| :--- | :--- | :--- | :--- |
| `diary_id` | BIGINT | PK, FK (diaries.id), Not Null | 日記への参照 |
| `tag_id` | BIGINT | PK, FK (tags.id), Not Null | タグへの参照 |

## 3. 設計上の決定事項
- **データベース**: 本番環境を想定しないため、組み込み型の H2 Database を使用する。
- **気分 (Mood)**: 柔軟性を持たせるためVARCHARとして保存。固定の選択肢が必要な場合は別途マスタテーブル化を検討する。
- **日記日付 (Entry Date)**: 過去の日付で日記を書けるよう、`created_at` とは別に保持する。
- **インデックス**: パフォーマンス向上のため、`diaries(user_id, entry_date)` および `diaries(mood)` へのインデックス作成を推奨する。
