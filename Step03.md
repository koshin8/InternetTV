# STEP0 ３:データ抽出

- よく見られているエピソードを知りたいです。エピソード視聴数トップ 3 のエピソードタイトルと視聴数を取得してください

```sql
SELECT episode_title, viewer
     FROM episodes
     ORDER BY viewer DESC
     LIMIT 3;
```

- よく見られているエピソードの番組情報やシーズン情報も合わせて知りたいです。エピソード視聴数トップ 3 の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください

```sql
SELECT p.program_title, ps.season_num, e.episode_num, e.episode_title, e.viewer
    FROM episodes AS e
    INNER JOIN program_season AS ps ON e.season_id = ps.season_id
    INNER JOIN programs AS p ON e.program_id = p.program_id
    ORDER BY e.viewer DESC
     LIMIT 3;
```

- 本日の番組表を表示するために、本日、どのチャンネルの、何時から、何の番組が放送されるのかを知りたいです。本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします

```sql
SELECT c.channel_name, CONCAT(s.relese_date, ' ', s.start_time) AS start_datetime, CONCAT(s.relese_date, ' ', s.end_time) AS end_datetime,
            ps.season_num, e.episode_num, e.episode_title, e.episode_detail
     FROM schedules AS s
     JOIN channels AS c ON s.channel_id = c.channel_id
     JOIN episodes AS e ON s.episode_id = e.episode_id
     JOIN program_season AS ps ON e.season_id = ps.season_id
     WHERE s.relese_date = CURDATE()
     ORDER BY s.start_time;
```

- ドラマというチャンネルがあったとして、ドラマのチャンネルの番組表を表示するために、本日から一週間分、何日の何時から何の番組が放送されるのかを知りたいです。ドラマのチャンネルに対して、放送開始時刻、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を本日から一週間分取得してください

```sql
 SELECT s.relese_date, s.start_time, s.end_time, ps.season_num, e.episode_num, e.episode_title, e.episode_detail
FROM schedules AS s
JOIN channels AS c ON s.channel_id = c.channel_id
JOIN episodes AS e ON s.episode_id = e.episode_id
JOIN program_season AS ps ON e.season_id = ps.season_id
WHERE c.channel_name = 'ドラマ'
  AND s.relese_date BETWEEN CURDATE() AND DATE_ADD(CURDATE(), INTERVAL 6 DAY)
ORDER BY s.relese_date, s.start_time;
```

s
