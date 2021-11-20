# Google Calender API
Googleカレンダーに似たような予定を大量に追加したい際に、わざわざアプリやWEBから一個一個登録するのが面倒なので、一括登録できるものを作成しました。


## APIの有効化
Google PlatFormにて認証などをしてjsonファイルをダウンロードします。
数年前と手順が違うっぽいです。現在はこのサイト(https://ateruimashin.com/diary/2021/02/google-calender-api/)を参考にしたらできると思います。
ダウンロードしたjsonファイルは`credentials.json`とリネームしたのち、`account/{your_account_name}/`に格納してください。

`{your_account_name}`のところは任意の文字列でいいので、Googleアカウント名(`○○@gmail.com`の`○○`の部分)とかを使用するといいと思います。

## `idlist.json` の作成
`account/{your_account_name}/`に、`idlist.json`というファイルを作成
し、カレンダーの予定名とそのIDに関するjsonを作成してください。

IDの入手方法はこちらをご参考に \
https://www.sukicomi.net/2018/07/google-calendarid.html \
(もしAPIでIDを取得する方法などあれば教えてください！)

jsonファイルの中身がわからない場合は`account/sample/idlist.json`を参考にしてください。また、jsonのkeyの部分の文字列は実際のマイカレンダーでの予定名と異なっていても構いませんので適宜変えてください。このkeyの文字列が、そのidの予定のデフォルトのタイトルになります。

## accountフォルダの中身の確認
`account`フォルダの中に好きな名前でディレクトリを作成し、その中に
- credentials.json
- idlist.json
  
が揃っていればOKです！！１度実行すればさらに
- token.picle
  
が自動保存されます。

複数のGoogleアカウントの予定を使い分けたい場合は適宜`account`の中にフォルダとファイルを追加してください。

## モジュールのimport
ターミナルで
```
pip install -r requirements.txt
```
と入力してください。

## 実行
```
python main.py
```
で実行できます。
初回の実行のみGoogleの警告が出ますので認証を許可してあげてください。
そのまま使ってもらうと標準入力・標準出力を使って独自の入力ルールで追加していく仕様になっているので、適当にカスタマイズしてください。

独自の入力ルールの定義は`src/define_format.py`に詳しく書かれていますが軽く説明すると、以下のいずれかを`,`区切りで入力（スペースは無視されます。)すると追加できます。

- 終日予定の場合
1. `{始まりの日付}*{日数}` 
1. `{始まりの日付}-{終わりの日付}` \
  もし`始まり < 終わり`でなければ、終わりの日付は翌月の日付とみなされます。
3. `{始まりの日付}` 
   
- 通常の予定の場合
1. `{日付}/{始まりの時刻}+{所要時間(時刻表記)}`
1. `{日付}/{始まりの時刻}-{終わりの時刻}` \
   もし`始まり < 終わり`でなければ、終わりの時刻は次の日とみなされます。
2. `{日付}/{始まりの時刻}`
    - 時刻の表記ルール
      - `25:00` は次の日の`01:00`扱いになります。
      - 分は省略可能です。  `15:00` = `15` です。

## main()の使用例
### 終日予定の追加で12月を選択した場合
```
8-11, 15, 30*3
```
- 12/8 ~ 12/11
- 12/15
- 12/30-1/1
  
の予定が登録されます。

### 通常の予定で12月を選択した場合
```
2/12-16, 5/13:30-16, 7/13+2:30, 10/23:00+5, 15/25:00, 18/23-3 
```
- 12/2 12:00~16:00
- 12/5 13:30~16:00
- 12/7 13:00~15:30
- 12/10 23:00 ~ 12/11 4:00
- 12/16 01:00~01:00
- 12/18 23:00 ~ 12/19 03:00

の予定が登録されます。


## 仕様
その他の詳細な仕様はソースコードのコメントに書いています！

   
  