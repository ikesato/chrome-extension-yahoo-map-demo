# Yahoo 雨雲レーダー Chrome Extension デモ


Yahoo 雨雲レーダーを表示する Chrome Extension のデモです。


スクリーンショット
------------------

![](https://github.com/ikesato/chrome-extension-yahoo-map-demo/raw/master/screenshot.png)



ディレクトリの説明
------------------

- chrome-extension  
 Chrome Extension 用のコードです。

- html  
 外部 http サーバに配置する html ファイルです。


使用方法
--------

### 準備するもの

- Chrome ブラウザ  
 このデモは Chrome バージョン 39.0.2171.71 (64-bit) で作成しました。

- 外部 HTTP サーバ  
 **HTTPS** サーバでは動作しません。**HTTP** サーバにする必要があります。


### 使い方

1. html/yahoo-map.html を編集  
 Yahoo マップを利用するために Yahoo! Open Local Platform (YOLP) からアプリケーションIDの登録が必要になります。
 https://e.developer.yahoo.co.jp/register から登録してください。登録後 html/yahoo-map.html 内の YOUR_API_ID を登録したアプリケーションIDで書き換えてください。

2. html/yahoo-map.html を外部 HTTP サーバに配置  
 ブラウザで参照できるようにしてください。  

3. chrome-extension/popup.html を編集  
 popup.html 内の YOUR_HTTP_SERVER/PATH を編集し、2で配置したURLになるようにします。

4. Chrome ブラウザで拡張機能を読み込む  
 Chrome ブラウザで拡張機能を読み込みます。

 - 拡張機能ページを開きます
 - デベロッパーモードにチェックをつけます
 - "パッケージ化されていない拡張機能を読み込む..." をクリックして、git clone した chrome-extension ディレクトリを選択してください

5. ブラウザにアイコンが表示するのでクリックしてみてください。  
 Yahoo 雨雲レーダーが表示します。




Content Security Policy (CSP) について
--------------------------------------

なぜ外部 HTTP サーバを用意する必要があるかを説明します。  
なにも考えずに Chrome Extension で Yahoo マップを表示しようとしても通常できません。
理由は Chrome Extension の Content Security Policy (CSP)  により外部サーバにある Javascript コンテンツが読み込めないためです。

Yahoo API を利用するには Javascript に以下の記述が必要です。

```````````````````````````````````
<script type="text/javascript"
  charset="utf-8"
  src="http://js.api.olp.yahooapis.jp/OpenLocalPlatform/V1/jsapi?appid=【アプリケーションID】">
</script>
``````````````````````````````````

http://developer.yahoo.co.jp/webapi/map/openlocalplatform/v1/js/

ですが CSP により src には http が指定できません。  
セキュリティ上の理由で http は無効にしているようです。  
https://developer.chrome.com/extensions/contentSecurityPolicy#relaxing-remote-script  
に詳細書いてあります。https であればホワイトリストに登録できるようですが http は中間者攻撃を食らう可能性があるので許可されてません。

YOLP を無料範囲内で利用する場合は http となります。ちなみに YOLP Premire の場合は https が利用できるようです。  

http がダメなので、回避策として外部 http サーバ + iframe を利用することにしました。ポップアップで表示する html に iframe で外部リソースを読み込ませることができます。
