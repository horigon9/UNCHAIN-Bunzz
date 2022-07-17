### 👩‍💻 コントラクトを新たに作成します

ここでは、Bunzzを使ってスマートコントラクトを作成していきます。

**Bunzzで行うこと**

1.  Create Dappでスマートコントラクトを作成
2.  各項目を入力する
3.  アプリケーションのData API root URLを設定


Bunzzの画面に入り「Create Dapp」をクリックしてDappを作成します

![](/public/images/99-NFT-MarketPlace/section-2/2_1_1.png)

Dapp Nameに入力して「Create」をクリック
![](/public/images/99-NFT-MarketPlace/section-2/2_1_2.png)


Networkを選択して、「Next」をクリック。今回はRopsten Testnetを利用しています
![](/public/images/99-NFT-MarketPlace/section-2/2_1_3.png)


今回は「Simple NFT MarketPlace」を選択
![](/public/images/99-NFT-MarketPlace/section-2/2_1_4.png)


ここで一度Bubbleに戻り、Settingsタブ→APIタブ→Data API root URLを取得
![](/public/images/99-NFT-MarketPlace/section-2/2_1_5.png)


*   name（好きなトークン名を入力）
*   symbol（好きなシンボル名を入力）
*   baseTokenURI（先ほど取得したData API root URLをペーストし以下のように変更）
    *   https://test-nft0011.bubbleapps.io/version-test/api/1.1/ ~~obj~~
    *   [https://test-nft0011.bubbleapps.io/version-test/api/1.1/contents/](https://test-nft0011.bubbleapps.io/version-test/api/1.1/contents/)

※BubbleのData APIは、有料プランから使える機能となっております
![](/public/images/99-NFT-MarketPlace/section-2/2_1_6.png)


### 🙋‍♂️ 質問する

ここまでの作業で何かわからないことがある場合は、Discord の `#section-2` で質問をしてください。

ヘルプをするときのフローが円滑になるので、エラーレポートには下記の 4 点を記載してください ✨

    1. 質問が関連しているセクション番号とレッスン番号
    2. 何をしようとしていたか
    3. エラー文をコピー&ペースト
    4. エラー画面のスクリーンショット
    

* * *

おめでとうございます！　セクション 2 が終了しました！
デプロイが完了した画面を `#section-2` に投稿して、あなたの成功をコミュニティで祝いましょう 🎉
次のセクションに進みましょう！

