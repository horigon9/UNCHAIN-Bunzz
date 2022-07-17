### 👩‍💻 **DappのID/API Keyをアプリ側に設定**

ここでは、BunzzSDKをアプリケーションサイドにセットアップしていきます。

**今回行うこと**

1.  Bubbleの各画面にBunzzSDKを設定


### **Bunzz SDKを各画面に設定**

#### **Bunzzを設定するページ**

*   NFTコンテンツ作成ページ
*   マイNFTコンテンツページ
*   NFTコンテンツページ

#### **各ページで設定する必要があるSetup方法からご紹介します**

Bubbleにて設定する前に「Dapp ID」と「API Key」を取得するためBunzzに戻り、画面左側の「Client SDK」をクリックします
![](/public/images/99-NFT-MarketPlace/section-3/3_2_1.png)


再び、Bubbleに移り、各ページ毎に「Bunzz SDK」を設定していきます。

まず、画面左側のVisual elements より「Bunzz SDK」を画面に設置し、先ほど取得した「Dapp ID」と「API Key」をそれぞれ入力します
![](/public/images/99-NFT-MarketPlace/section-3/3_2_2.png)


次にWorkflow画面に移り、Setupを行いますが、ページが読み込まれた際にSetupが行われるように「Page is loaded」のactionに「Setup BunzzSDK」を設定します
![](/public/images/99-NFT-MarketPlace/section-3/3_2_3.png)


Setupが完了したら、addressを取得するのですが、Bunzzの方で「Bunzz SDK Setup Completed」というSetupが完了した際に設定できるEventを用意してくれているので、これを使い、actionに「Get Signer Address」を設定します
![](/public/images/99-NFT-MarketPlace/section-3/3_2_4.png)


### 🙋‍♂️ 質問する

ここまでの作業で何かわからないことがある場合は、Discord の `#section-3` で質問をしてください。

ヘルプをするときのフローが円滑になるので、エラーレポートには下記の 4 点を記載してください ✨

    1. 質問が関連しているセクション番号とレッスン番号
    2. 何をしようとしていたか
    3. エラー文をコピー&ペースト
    4. エラー画面のスクリーンショット
    

* * *

おめでとうございます！　セクション 3 が終了しました！

各画面でBunzzSDKのセットアップが完了したらプレビュー画面でページをロードしてコンソールでセットアップできているか確認しましょう！

そちらのスクリーンショットを `#section-3` に投稿して、あなたの成功をコミュニティで祝いましょう 🎉次のセクションに進みましょう！
