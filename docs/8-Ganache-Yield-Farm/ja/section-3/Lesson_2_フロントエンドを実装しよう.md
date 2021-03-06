###  ð¥ ãã®ã¬ãã¹ã³ã®åèåç»URL
[Dapp University](https://youtu.be/CgXQC4dbGUE?t=6809)

### ð¤ ãã­ã³ãã¨ããã¯ã®æ¥ç¶é¨åãä½æãã

UIã®ä½æã®åã«ãã­ã³ãã¨ããã¯ã¨ã³ããã¤ãªããå¿è¦ãããã®ã§ããã®é¨åãä»ä¸ãã¦ãããã¨æãã¾ãã

`App.js` ãã¡ã¤ã«ãä»¥ä¸ã®ããã«æ´æ°ãã¦ãã ããã

```javascript
// App.js
// ãã­ã³ãã¨ã³ããæ§ç¯ããä¸ã§å¿è¦ãªãã¡ã¤ã«ãã©ã¤ãã©ãªãã¤ã³ãã¼ããã
import React, { Component } from 'react'
import Web3 from 'web3'
import DaiToken from '../abis/DaiToken.json'
import Navbar from './Navbar'
import './App.css'

class App extends Component {
  // componentWillMount(): ä¸»ã«ãµã¼ãã¼ã¸ã®APIã³ã¼ã«ãè¡ããªã©ãå®éã®ã¬ã³ããªã³ã°ãè¡ãããåã«ãµã¼ãã¼ãµã¤ãã®ã­ã¸ãã¯ãå®è£ããããã«ä½¿ç¨ã
  async componentWillMount() {
    await this.loadWeb3()
    await this.loadBlockchainData()
  }
  // loadBlockchainData(): ãã­ãã¯ãã§ã¼ã³ä¸ã®ãã¼ã¿ã¨ããåãããããã®é¢æ°
  // MetaMask ã¨ã®æ¥ç¶ã«ãã£ã¦å¾ãããæå ±ã¨ã³ã³ãã©ã¯ãã¨ã®æå ±ãä½¿ã£ã¦æç»ã«ä½¿ãæå ±ãåå¾ã
  async loadBlockchainData() {
    const web3 = window.web3
    // ã¦ã¼ã¶ã¼ã® Metamask ã®ä¸çªæåã®ã¢ã«ã¦ã³ãï¼è¤æ°ã¢ã«ã¦ã³ããå­å¨ããå ´åï¼åå¾
    const accounts = await web3.eth.getAccounts()
    // ã¦ã¼ã¶ã¼ã® Metamask ã¢ã«ã¦ã³ããè¨­å®
    // ãã®æ©è½ã«ãããApp.js ã«è¨è¼ããã¦ãã constructor() åã® accountï¼ããã©ã«ã: '0x0'ï¼ãæ´æ°ããã
    this.setState({ account: accounts[0]})
    // ã¦ã¼ã¶ã¼ã Metamask ãä»ãã¦æ¥ç¶ãã¦ãããããã¯ã¼ã¯IDãåå¾
    const networkId = await web3.eth.net.getId()
    // DaiToken ã®ãã¼ã¿ãåå¾
    const daiTokenData = DaiToken.networks[networkId]
    if(daiTokenData){
      // DaiToken ã®æå ±ã daiToken ã«æ ¼ç´ãã
      const daiToken = new web3.eth.Contract(DaiToken.abi, daiTokenData.address)
      // constructor() åã® daiToken ã®æå ±ãæ´æ°ãã
      this.setState({daiToken})
      // ã¦ã¼ã¶ã¼ã® Dai ãã¼ã¯ã³ã®æ®é«ãåå¾ãã
      let daiTokenBalance = await daiToken.methods.balanceOf(this.state.account).call()
      // daiTokenBalanceï¼ã¦ã¼ã¶ã¼ã® Dai ãã¼ã¯ã³ã®æ®é«ï¼ãã¹ããªã³ã°åã«å¤æ´ãã
      this.setState({daiTokenBalance: daiTokenBalance.toString()})
      // ã¦ã¼ã¶ã¼ã® Dai ãã¼ã¯ã³ã®æ®é«ããã­ã³ãã¨ã³ãã® Console ã«åºåãã
      console.log(daiTokenBalance.toString())
    }else{
      window.alert('DaiToken contract not deployed to detected network.')
    }

  }

  // loadWeb3(): ã¦ã¼ã¶ã¼ã Metamask ã¢ã«ã¦ã³ããæã£ã¦ãããç¢ºèªããé¢æ°
  async loadWeb3() {
    // ã¦ã¼ã¶ã¼ã Metamask ã®ã¢ã«ã¦ã³ããæã£ã¦ããå ´åã¯ãã¢ãã¬ã¹ãåå¾
    if (window.etheruem) {
      window.web3 = new Web3(window.etheruem)
      await window.etheruem.enable()
    }
    else if (window.web3) {
      window.web3 = new Web3(window.web3.currentProvider)
    }
    // ã¦ã¼ã¶ã¼ã Metamask ã®ã¢ã«ã¦ã³ããæã£ã¦ããªãã£ãå ´åã¯ãã¨ã©ã¼ãè¿ã
    else {
      window.alert('Non etheruem browser detected. You should consider trying to install metamask')
    }

    this.setState({ loading: false})
  }

  // constructor(): ãã­ãã¯ãã§ã¼ã³ããèª­ã¿è¾¼ãã ãã¼ã¿ + ã¦ã¼ã¶ã¼ã®ç¶æãæ´æ°ããé¢æ°
  constructor(props) {
    super(props)
    this.state = {
      account: '0x0',
      daiToken: {},
      dappToken: {},
      tokenFarm: {},
      daiTokenBalance: '0',
      dappTokenBalance: '0',
      stakingBalance: '0',
      loading: true
    }
  }
  // ãã­ã³ãã¨ã³ãã®ã¬ã³ããªã³ã°ãä»¥ä¸ã§å®è¡ããã
  render() {
    return (
      <div>
        <Navbar account={this.state.account} />
        <div className="container-fluid mt-5">
          <div className="row">
            <main role="main" className="col-lg-12 ml-auto mr-auto" style={{ maxWidth: '600px' }}>
              <div className="content mr-auto ml-auto">
                <a
                  href="http://www.dappuniversity.com/bootcamp"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                </a>

                <h1>Hello, World!</h1>

              </div>
            </main>
          </div>
        </div>
      </div>
    );
  }
}

export default App;
```

ãã­ã³ãã¨ã³ãã®ã³ã¼ãã®èª¬æã¯ãã³ã¡ã³ãæ¬ã«è¨è¼ããã¦ãã¾ãã

ããã¤ãéè¦ãªã³ã³ã»ãããããã®ã§ãã¿ã¦ããã¾ãããã

ã¾ãã¯ã`App.js` ã®åé ­ã«è¨è¼ããã¦ããã¤ã³ãã¼ãã®é¨åã«æ³¨ç®ãã¦ãã ããã

```javascript
// App.js
import React, { Component } from 'react'
import Web3 from 'web3'
import DaiToken from '../abis/DaiToken.json'
import Navbar from './Navbar'
import './App.css'
```

ããã§ã¯ããã­ã³ãã¨ã³ããå®è£ããã®ã«å¿è¦ãªãã¡ã¤ã«ãã¤ã³ãã¼ããã¦ãã¾ãã

ãã¤ã³ãã¨ãªãã®ã¯ãWalletã¨ãã­ã³ãã¨ã³ãã®æ¥ç¶ãå¯è½ã«ããããã® `web3` ã©ã¤ãã©ãªã¨ãã­ã³ãã¨ã³ããã `DaiToken` ã³ã³ãã©ã¯ãã¨ããã¨ãããããã® `DaiToken.json` ãã¤ã³ãã¼ããã¦ããç¹ã§ãã
- `web3` / `web3.js` ã¯ãã¯ã©ã¤ã¢ã³ããµã¤ãã¢ããªã±ã¼ã·ã§ã³ããã­ãã¯ãã§ã¼ã³ã¨å¯¾è©±ããããã®JavaScriptã©ã¤ãã©ãªã§ãã

æ¬¡ã¯ `constructor()` ãè¦ã¦ããã¾ããããã§ã¯ãMetamask ãä»ãã¦ã¦ã¼ã¶ã¼ãã¢ããªã«ã­ã°ã¤ã³ããéã«ãæ´æ°ãããå¤æ°ãå®£è¨ãã¦ãã¾ãã

```javascript
// App.js
constructor(props) {
  super(props)
  this.state = {
    account: '0x0',
    daiToken: {},
    dappToken: {},
    tokenFarm: {},
    daiTokenBalance: '0',
    dappTokenBalance: '0',
    stakingBalance: '0',
    loading: true
  }
}
```

`account` ã `daiTokenBalance` ãªã©ã¦ã¼ã¶ã¼ã®æä½ã«ãã£ã¦å¤åããããããªãã®ã¯ããã§å®£è¨ãã¦ããã¾ãã


ã§ã¯ããã­ã³ãã¨ã³ããç¢ºèªãã¦ã¿ã¾ãããããã¡ã¤ã«ãä¸æ¸ããã¦ä¿å­ããã¨ããã­ã³ãã¨ã³ãã«åæ ããã¾ãã

å®éã«ã¯ãã­ã³ãã¨ã³ãã®UIã¯å¤æ´ããã¦ããªãã®ã§ãå³ã¯ãªãã¯ãã¦ã`Inspect`ããé¸æãããã©ã¦ã¶å´ã®Console ãç¢ºèªãã¾ãã

ä»¥ä¸ã®ãããªç»åãåºã¦ããã°ããã­ã³ãã¨ã³ãã®æ´æ°ã¯æåã§ãï¼

![](/public/images/8-Ganache-Yield-Farm/section-1/12_3_8.png)

ãããã­ã³ãã¨ã³ãã«å¤æ´ãåæ ããã¦ããªãã£ããããã¼ã¸ããªãã¬ãã·ã¥ãã¦ã¿ãããã¿ã¼ããã«ã§ `^C` ãå¥åãã¦ããä¸åº¦ `npm run start` ãå®è¡ãã¦ã¿ã¦ãã ããã

### ðââï¸ è³ªåãã

ããã¾ã§ã®ä½æ¥­ã§ä½ãããããªããã¨ãããå ´åã¯ãDiscord ã® `#section-3` ã§è³ªåããã¦ãã ããã

ãã«ããããã¨ãã®ãã­ã¼ãåæ»ã«ãªãã®ã§ãã¨ã©ã¼ã¬ãã¼ãã«ã¯ä¸è¨ã® 3 ç¹ãè¨è¼ãã¦ãã ãã â¨

```
1. è³ªåãé¢é£ãã¦ããã»ã¯ã·ã§ã³çªå·ã¨ã¬ãã¹ã³çªå·
2. ä½ããããã¨ãã¦ããã
3. ã¨ã©ã¼æãã³ãã¼&ãã¼ã¹ã
4. ã¨ã©ã¼ç»é¢ã®ã¹ã¯ãªã¼ã³ã·ã§ãã
```

---
ããã§ãã­ã³ãã¨ããã¯ã¨ã³ãã®æ¥ç¶é¨åã®å¤§åãå®æãã¾ãããæ¬¡ã®ã¬ãã¹ã³ã§ã¯æ®ãã®æ¥ç¶é¨åã¨ãã­ã³ãã¨ã³ãã®ãã¶ã¤ã³ãã³ã¼ãã£ã³ã°ãã¦Token Farmãå®æããã¾ãããï¼
