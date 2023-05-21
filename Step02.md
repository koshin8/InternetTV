# STEP02:データベースの構築とサンプルデータ挿入

### データベースの構築

- ターミナルで `mysql -u my_name -p`と入力し起動させる
- `CREATE DATABASE internet_tv;`と入力しデータベースを作成する (Query OK と出たらできています)

- `SHOW DATABASE;`　と入力し作成できているか確認

### テーブルの作成

- `USE internet_tv;`　で使うデータベースを切り替える
- その後各テーブルを作成する SQL 文を書いていきます
    <details>
    <summary>テーブル作成SQL文</summary>

  ```sql
  --チャンネルテーブル--
  CREATE TABLE channels (
  channel_id INT PRIMARY KEY AUTO_INCREMENT,
  channel_name VARCHAR(255) UNIQUE NOT NULL
  );
  --番組テーブル--
  CREATE TABLE programs (
  program_id INT PRIMARY KEY AUTO_INCREMENT,
  program_title VARCHAR(255) UNIQUE NOT NULL
  );
  --番組シーズン数テーブル--
  CREATE TABLE program_season (
  season_id INT PRIMARY KEY AUTO_INCREMENT,
  season_num INT,
  program_id INT,
  FOREIGN KEY (program_id) REFERENCES programs (program_id),
  UNIQUE KEY (season_num, program_id)
  );
  --番組詳細テーブル--
  CREATE TABLE program_details (
  program_detail_id INT PRIMARY KEY AUTO_INCREMENT,
  program_id INT,
  program_detail TEXT,
  FOREIGN KEY (program_id) REFERENCES programs (program_id),
  UNIQUE KEY (program_detail(255), program_id)
  );
  --ジャンルテーブル--
  CREATE TABLE genres (
  genre_id INT PRIMARY KEY AUTO_INCREMENT,
  genre_name VARCHAR(255) UNIQUE NOT NULL
  );
  --番組ジャンルテーブル--
  CREATE TABLE program_genre (
  program_id INT,
  genre_id INT,
  PRIMARY KEY (program_id, genre_id),
  FOREIGN KEY (program_id) REFERENCES programs(program_id),
  FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
  );
  --チャンネルジャンルテーブル--
  CREATE TABLE channel_genre (
  channel_id INT,
  genre_id INT,
  PRIMARY KEY (channel_id, genre_id),
  FOREIGN KEY (channel_id) REFERENCES channels(channel_id),
  FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
  );
  --エピソードテーブル--
  CREATE TABLE episodes (
  episode_id INT PRIMARY KEY AUTO_INCREMENT,
  season_id INT,
  program_id INT,
  episode_num INT,
  episode_title VARCHAR(255),
  episode_detail TEXT,
  video_time TIME,
  release_date DATE,
  viewer INT DEFAULT 0,
  FOREIGN KEY (season_id) REFERENCES program_season (season_id),
  FOREIGN KEY (program_id) REFERENCES programs (program_id),
  UNIQUE KEY (episode_num, season_id)
  );
  --番組表テーブル--
  CREATE TABLE schedules (
  schedules_id INT PRIMARY KEY AUTO_INCREMENT,
  relese_date DATE,
  start_time TIME,
  end_time TIME,
  channel_id INT,
  program_id INT,
  episode_id INT,
  FOREIGN KEY (channel_id) REFERENCES channels (channel_id),
  FOREIGN KEY (program_id) REFERENCES programs (program_id),
  FOREIGN KEY (episode_id) REFERENCES episodes (episode_id),
  UNIQUE KEY (channel_id, start_time,end_time)
  );
  ```

  </details>

### サンプルデータ挿入

<details>
    <summary>サンプルデータ挿入</summary>

```sql

--channels テーブルのサンプルデータ
INSERT INTO channels (channel_name) VALUES
('Channel 1'),
('Channel 2'),
('Channel 3'),
('ドラマ');

-- programs テーブルのサンプルデータ
INSERT INTO programs (program_title) VALUES
('Program 1'),
('Program 2'),
('Program 3'),
('絶対泣けるドラマ');

-- genres テーブルのサンプルデータ
INSERT INTO genres (genre_name) VALUES
('Genre 1'),
('Genre 2'),
('Genre 3'),
('ドラマ');

-- episodes テーブルのサンプルデータ
INSERT INTO episodes (season_id, program_id, episode_num, episode_title, episode_detail, video_time, release_date, viewer) VALUES
(1, 1, 1, 'Episode 1', 'Episode 1 details', '01:30:00', '2023-01-01', 100),
(1, 1, 2, 'Episode 2', 'Episode 2 details', '00:45:00', '2023-01-08', 150),
(2, 2, 1, 'Episode 1', 'Episode 1 details', '01:00:00', '2023-02-01', 200),
(4, 4, 1, '', 'ドラマ details', '01:00:00', '2023-02-01', 300);

-- program_season テーブルのサンプルデータ
INSERT INTO program_season (season_num, program_id) VALUES
(1, 1),
(2, 2),
(1, 3),
(1, 4);

-- program_details テーブルのサンプルデータ
INSERT INTO program_details (program_id, program_detail) VALUES
(1, 'Program 1 details'),
(2, 'Program 2 details'),
(3, 'Program 3 details'),
(4, '感動ドラマ詳細');
-- program_genre テーブルのサンプルデータ
INSERT INTO program_genre (program_id, genre_id) VALUES
(1, 1),
(1, 2),
(2, 2),
(4, 7);

-- channel_genre テーブルのサンプルデータ
INSERT INTO channel_genre (channel_id, genre_id) VALUES
(1, 1),
(2, 2),
(3, 3);

-- schedules テーブルのサンプルデータ
INSERT INTO schedules (relese_date, start_time, end_time, channel_id, program_id, episode_id)
VALUES
    ('2023-01-01', '09:00:00', '10:00:00', 1, 1, 4),
    ('2023-01-02', '11:00:00', '12:00:00', 2, 1, 5),
    ('2023-01-03', '15:00:00', '16:00:00', 3, 2, 6);



```
