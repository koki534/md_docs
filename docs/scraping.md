# スクレイピング

## Beautiful Soup

Beautiful Soup : HTMLの構造を解析する目的で用いられるライブラリ

ライブラリのインストール

```bash
pip install BeautifulSoup4
pip install requests
```



## selenium

seleniumはブラウザの操作が得意

### ライブラリのインストール

```bash
pip install selenium
brew install chromedriver
```

ライブラリの読み込み

```python
from selenium import webdriver
from time import sleep
```

ブラウザの起動

```
driver = webdriver.Chrome()
```



```python
# webページを開く
URL = ''
driver.get(URL)
time.sleep(2)
```





### ヘッドレスモード

画面を介さずにコマンドのみで処理を完結させるモード

```python
from selenium.webdriver.chrome.options import Options
options = Options()
options.add_argument('--headless')
browser = webdriver.Chrome(options=options)
```

