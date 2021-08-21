# Pytube メモ

[https://pytube.io/en/latest/user/quickstart.html]

インポート

```python
from pytube import YouTube, Channel
```

チャンネルデータの読み込み

```python
c = Channel('チャンネルURL')
```



動画idだけ取り出す

```python
target = 'v='
idx = c.video_urls[0].find(target)
videoid = c.video_urls[0][idx+len(target):]
```



動画データの読み込み

```python
videoid = '動画id'
yt = YouTube('http://youtube.com/watch?v='+videoid)
```

動画タイトルの表示

```python
print(yt.title)
```



## 動画のダウンロード

動画タイプの一覧表示

```python
yt.streams
```

動画タイプをmp4でフィルタリング

```python
yt.streams.filter(file_extension='mp4')
```

インデックスタグを指定する

```python
stream = yt.streams.get_by_itag(299)
```

動画のダウンロード

```python
stream.download('ダウンロード先ディレクトリ')
```



## 字幕

字幕の言語一覧

```python
print(yt.captions.all())
```

日本語(自動生成)を指定

```python
caption = yt.captions.get_by_language_code('a.ja')
```

字幕の表示

```python
print(caption.generate_srt_captions())
```



