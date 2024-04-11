---
Title: ' Groongaを囲む夕べ4をサクッと振り返る'
Category:
- groonga
- ruby
- mysql
- milkode
Date: 2013-12-02T01:57:01+09:00
URL: https://ongaeshi.hatenablog.com/entry/groonga04
EditURL: https://blog.hatena.ne.jp/tuto0621/ongaeshi.hatenablog.com/atom/entry/12921228815713602835
---

今年も[全文検索エンジンGroongaを囲む夕べ 4](http://atnd.org/events/43461)にいってきました。[去年](http://ongaeshi.hatenablog.com/entry/20121203/1354547962)に引き続き、今回もスピーカーとして参加しました。

## 発表したスライド
- [Milkode2013年の歩み](http://ongaeshi.me/slide/groonga04/) ※ スライドが見れない場合はFirefoxやChromeで見れるはずです。

[10ページ](http://ongaeshi.me/slide/groonga04/#10)からのGroongaのドリルダウンをどうやってMilkodeに組み込んだかの話が割と評判よくてよかったです。先にデモでざっくりとこういう機能があるということを見せてから、その内側を説明する流れは上手くいきました。

毎年やっていると発表も少しずつ上手くなってきた気がします。去年はどれくらいの人がMilkodeを使っているか質問するのを忘れるほど緊張していたのですが今年は無事聞く事が出来ました。(でも手を挙げてくれた人の顔を確認する程の余裕は無かったです・・これはまた来年に)

## 発表メモ
### 第1部
- Groonga族2013 (須藤さん)
  - 今日の勉強会の道しるべを示す
  - 世界進出
  - Droongaとは: 分散Groonga、他のマシンで平行して使いたい (Rubyで書かれている)
  - Grnxxの特徴(大容量、インメモリー)
  - Mroonga、Windowsサポート開始
  - Rroonga、Windowsの64it対応
  - grndump、データベースのダンプするならこちら
- Mroonga今年の収穫 (斯波さん)
  - Mroongaは、MySQL/MariaDBのプラグイン
  - 現在、MariaDBへのバンドル作業中です
- Droongaの紹介 (森さん)
  - Droonga 0.7.0 (初お披露目)
  - Distributed(分散)の"D"
  - ストリーム指向(入出力: JSONデータストリーム)
- Grnxxの紹介 (矢田さん)
  - Groongaの後継
  - インメモリDBの台頭
    - インメモリ度が高い
  - (旧) 最大合計キー長 4GiB, インメモリDBの台頭
  - (新) 1T(1兆1000億), 最大合計キー長 256TiB

### 第2部: Groongaと商用利用
- DeNAの大槻さん
  - 技術企画、イベントのサポート
  - 採用情報→キャリア採用→テクノロジー
- Mroongaサポートの紹介 (SCSKの池田さん)
  - SCSKはMroonga商用の技術サポートをはじめました
-  Groongaビジネスパートナー募集 (南さん)
  - ビジネスパートナー募集中


### 第3部: Groongaユーザーから
- TritonnからMroongaへの移行体験記 (@yoshi_kenさん)
  - MroongaはProductでも使えるよ
- Mroonga de fulltextsearch (@yoku0825さん、)
  - Trittonからの移行先としてはMroonga鉄板でいいと思う
- InnoDB FTS より Mroonga！ (GREEの@ichii386さん)
  - InnoDB FTS は日本語の全文検索が大変そう
- Groongaをem-synchronyから使う (@takiuchiさん)
  - Rubyのライブラリ : 非同期処理を書きやすくするためのライブラリ
- 休憩空け: DeNAのたなべさん
  - 会場がDeNAになった経緯について
- Groongaを支える取り組みの紹介(仮) (クリアコードの林さん)
  - マシンクラッシュ(また後で)
- GroongaとMapKit (DeNAの沖津さん)
  - エンターテイメント事業本部
  - Peko -> USと東南アジア
  - MySQLをJSON形式で格納、Groongaはhttp経由で、マスターはMySQL
  - 位置情報の検索に使う
  - geo_in_rectangleという関数があったが、南半球の人が検索出来ない
  - 位置の変更が行われたら2秒待ってから検索(負荷を減らす)
  - 一カ所にたくさんの人が集中していると見にくいので25件位に絞りこむ(IDを適度にとばしたり)
- Groonga on スマホ (丸山(@iskm)さん)  
  - Groonga on スマホアプリの紹介
  - iMirrors
  - オフライン検索
  - 検索結果をキャッシュしてみる(Google, Facebook, Twitter, Youtube, Blutooth, P2P)
  - アプリが利用できるメモリ上限 (20〜30MBでアプリ強制終了)
- Milkode2013年の歩み (おんがえし(@ongaeshi))
  - 緊張した
- Groongaを支える取り組みの紹介(仮) (クリアコードの林さん)
  - APIドキュメント化
  - 本を書きたい人はいませんか？
- GroongaからMroongaへの移行に際して・・・　(株式会社WEICの網岡隆宏さん)
  - 中国語学習のサイト
  - VECTORカラムの置き換え

## 感想
- Grnxx。性能はとてもよくなりそうだけどRrronga等が動かなくなる可能性？MilkodeはGroongaのままか、Grnxxに移行出来るのかどうなるかなぁ。この辺は来年の夕べで分かりそう。
- GroongaとMapKit。Peko(iPhoneアプリ)とGroongaによる位置検索を連携させる具体例としてとても面白かったです。
- Groonga on スマホ。早くiMirrorsが使ってみたいです、iPhoneアプリが突然落ちる理由がようやく分かりました。

