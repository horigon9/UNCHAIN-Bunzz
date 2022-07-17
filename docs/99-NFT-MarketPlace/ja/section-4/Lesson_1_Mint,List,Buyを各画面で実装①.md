### 👩‍💻 **Mint,List,Buyを各画面で実装**

ここでは、各画面ごとにBunzzSDKを通じて、Mint,List,Buy等のFunctionを呼び出していきます。

**実装する画面**

*   NFTコンテンツ作成ページ


### **NFTコンテンツ作成ページ**

「Create」ボタンがクリックされたときに、NFTを新たに作成・発行を行いますが、その際にBunzzの「Update action」を設定します
![](/public/images/99-NFT-MarketPlace/section-4/4_1_1.png)

次に「Module Name」に「NFT(ERC721)」、「Function Name」に「mint」を入力します
![](/public/images/99-NFT-MarketPlace/section-4/4_1_2.png)

最後に「address」にSetup時に取得している「Signer Address」、「tokenId」にコンテンツの「id」を設定して完了です（この際にBubbleで生成されるuniqueIdを利用しないよう注意してください）
![](/public/images/99-NFT-MarketPlace/section-4/4_1_3.png)

これでNFTを新たに作成できました！実際にプレビューから確認してみましょう！


### 🙋‍♂️ 質問する

ここまでの作業で何かわからないことがある場合は、Discord の `#section-4` で質問をしてください。

ヘルプをするときのフローが円滑になるので、エラーレポートには下記の 4 点を記載してください ✨

    1. 質問が関連しているセクション番号とレッスン番号
    2. 何をしようとしていたか
    3. エラー文をコピー&ペースト
    4. エラー画面のスクリーンショット
    

* * *

NFTコンテンツ作成ページにてmint機能の実装が完了したら、次のレッスンに進んでください 🎉
