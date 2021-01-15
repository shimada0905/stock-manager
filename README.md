# テーブル設計

## users テーブル

| Column   | Type   | Options     |
|----------|--------|-------------|
| nickname | string | null: false |
| email    | string | null: false |
| password | string | null: false |

### Association

- has_many :room_users
- has_many :rooms, through: room_users
- has_many :stocks
- has_many :relationships
- has_many :followings, through: :relationships, source: :follow
- has_many :reverse_of_relationships, class_name: 'Relationship' foreign_key: 'follow_id'
- has_many :followers, through: :reverse_of_relationships, source: :user

## rooms テーブル

| Column    | Type   | Options     |
|-----------|--------|-------------|
| room_name | string | null: false |

### Association

- has_many :room_users
- has_many :users, through: room_users
- has_many :stocks

## room_users テーブル

| Column | Type       | Options                        |
|--------|------------|--------------------------------|
| user   | references | null: false, foreign_key: true |
| room   | references | null: false, foreign_key: true |

### Association

- belongs_to :room
- belongs_to :user

## stocks テーブル

| Column     | Type       | Options                        |
|------------|------------|--------------------------------|
| stock_name | string     | null: false                    |
| quantity   | integer    | null: false                    |
| user       | references | null: false, foreign_key: true |
| room       | references | null: false, foreign_key: true |

### Association

- belongs_to :room
- belongs_to :user

## relationships テーブル

| Column | Type       | Options                                     |
|--------|------------|---------------------------------------------|
| user   | references | null: false, foreign_key: true              |
| follow | references | null: false, foreign_key: {to_table: users} |

### Association

- belongs_to :user
- belongs_to :follow, class_name: 'User'