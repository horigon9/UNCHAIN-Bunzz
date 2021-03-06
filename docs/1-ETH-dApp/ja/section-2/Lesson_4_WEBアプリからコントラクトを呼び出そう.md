### ð Web ã¢ããªã±ã¼ã·ã§ã³ããã¹ãã¼ãã³ã³ãã©ã¯ããå¼ã³åºã

ãã®ã¬ãã¹ã³ã§ã¯ãMetaMask ã®èªè¨¼æ©è½ãä½¿ç¨ãã¦ãWeb ã¢ããªã±ã¼ã·ã§ã³ããå®éã«ããªãã®ã³ã³ãã©ã¯ããå¼ã³åºãæ©è½ãå®è£ãã¾ãã

`WavePortal.sol` ã«å®è£ãã `getTotalWaves` é¢æ°ãè¦ãã¦ãã¾ããï¼

```javascript
// WavePortal.sol
  function getTotalWaves() public view returns (uint256) {
      console.log("We have %d total waves!", totalWaves);
      return totalWaves;
  }
```

`App.js` ãä»¥ä¸ã®ããã«æ´æ°ãã¦ããã­ã³ãã¨ã³ããã `getTotalWaves` é¢æ°ã¸ã¢ã¯ã»ã¹ã§ããããã«ãã¾ãã

```javascript
// App.js
import React, { useEffect, useState } from "react";
import "./App.css";
/* ethers å¤æ°ãä½¿ããããã«ãã*/
import { ethers } from "ethers";
const App = () => {
  // ã¦ã¼ã¶ã¼ã®ãããªãã¯ã¦ã©ã¬ãããä¿å­ããããã«ä½¿ç¨ããç¶æå¤æ°ãå®ç¾©ãã¾ãã
  const [currentAccount, setCurrentAccount] = useState("");
  console.log("currentAccount: ", currentAccount);
  // window.ethereumã«ã¢ã¯ã»ã¹ã§ãããã¨ãç¢ºèªãã¾ãã
  const checkIfWalletIsConnected = async () => {
    try {
      const { ethereum } = window;
      if (!ethereum) {
        console.log("Make sure you have MetaMask!");
        return;
      } else {
        console.log("We have the ethereum object", ethereum);
      }
      // ã¦ã¼ã¶ã¼ã®ã¦ã©ã¬ããã¸ã®ã¢ã¯ã»ã¹ãè¨±å¯ããã¦ãããã©ãããç¢ºèªãã¾ãã
      const accounts = await ethereum.request({ method: "eth_accounts" });
      if (accounts.length !== 0) {
        const account = accounts[0];
        console.log("Found an authorized account:", account);
        setCurrentAccount(account);
      } else {
        console.log("No authorized account found");
      }
    } catch (error) {
      console.log(error);
    }
  };
  // connectWalletã¡ã½ãããå®è£
  const connectWallet = async () => {
    try {
      const { ethereum } = window;
      if (!ethereum) {
        alert("Get MetaMask!");
        return;
      }
      const accounts = await ethereum.request({
        method: "eth_requestAccounts",
      });
      console.log("Connected: ", accounts[0]);
      setCurrentAccount(accounts[0]);
    } catch (error) {
      console.log(error);
    }
  };
  // waveã®åæ°ãã«ã¦ã³ãããé¢æ°ãå®è£
  const wave = async () => {
    try {
      const { ethereum } = window;
      if (ethereum) {
        const provider = new ethers.providers.Web3Provider(ethereum);
        const signer = provider.getSigner();
        const wavePortalContract = new ethers.Contract(
          contractAddress,
          contractABI,
          signer
        );
        let count = await wavePortalContract.getTotalWaves();
        console.log("Retrieved total wave count...", count.toNumber());
        console.log("Signer:", signer);
      } else {
        console.log("Ethereum object doesn't exist!");
      }
    } catch (error) {
      console.log(error);
    }
  };
  // WEBãã¼ã¸ãã­ã¼ããããã¨ãã«ä¸è¨ã®é¢æ°ãå®è¡ãã¾ãã
  useEffect(() => {
    checkIfWalletIsConnected();
  }, []);
  return (
    <div className="mainContainer">
      <div className="dataContainer">
        <div className="header">
          <span role="img" aria-label="hand-wave">
            ð
          </span>{" "}
          WELCOME!
        </div>
        <div className="bio">
          ã¤ã¼ãµãªã¢ã ã¦ã©ã¬ãããæ¥ç¶ãã¦ãã
          <span role="img" aria-label="hand-wave">
            ð
          </span>
          (wave)ããéã£ã¦ãã ãã
          <span role="img" aria-label="shine">
            â¨
          </span>
        </div>
        {/* waveãã¿ã³ã«waveé¢æ°ãé£åãããã*/}
        <button className="waveButton" onClick={wave}>
          Wave at Me
        </button>
        {/* ã¦ã©ã¬ããã³ãã¯ãã®ãã¿ã³ãå®è£ */}
        {!currentAccount && (
          <button className="waveButton" onClick={connectWallet}>
            Connect Wallet
          </button>
        )}
        {currentAccount && (
          <button className="waveButton" onClick={connectWallet}>
            Wallet Connected
          </button>
        )}
      </div>
    </div>
  );
};
export default App;
```

ããã§å®è£ããæ°ããæ©è½ã¯ä¸è¨ã® 3 ã¤ã§ãã

**1 \. ethers å¤æ°ãä½¿ããããã«ãã**

```javascript
// App.js
import { ethers } from "ethers";
```

`ethers` ã®ãã¾ãã¾ãªã¯ã©ã¹ãé¢æ°ã¯ã[ethersproject](https://docs.ethers.io/v5/getting-started/) ãæä¾ãããµãããã±ã¼ã¸ããã¤ã³ãã¼ãã§ãã¾ããããã¯ãWeb ã¢ããªã±ã¼ã·ã§ã³ããã³ã³ãã©ã¯ããå¼ã³åºãéã«å¿é ã¨ãªãã®ã§ãè¦ãã¦ããã¾ãããã

**2 \. wave ã®åæ°ãã«ã¦ã³ãããé¢æ°ãå®è£ãã**

```javascript
// App.js
const wave = async () => {
  try {
    // ã¦ã¼ã¶ã¼ãMetaMaskãæã£ã¦ãããç¢ºèª
    const { ethereum } = window;
    if (ethereum) {
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      const wavePortalContract = new ethers.Contract(
        contractAddress,
        contractABI,
        signer
      );
      let count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count...", count.toNumber());
      console.log("Signer:", signer);
    } else {
      console.log("Ethereum object doesn't exist!");
    }
  } catch (error) {
    console.log(error);
  }
};
```

è¿½å ãããã³ã¼ããè¦ãªãããæ°ããæ¦å¿µã«ã¤ãã¦å­¦ã³ã¾ãããã

**I\. `provider`**

> ```javascript
> // App.js
> const provider = new ethers.providers.Web3Provider(ethereum);
> ```
>
> ããã§ã¯ã`provider` (= MetaMask) ãè¨­å®ãã¦ãã¾ãã
> `provider` ãä»ãã¦ãã¦ã¼ã¶ã¼ã¯ãã­ãã¯ãã§ã¼ã³ä¸ã«å­å¨ããã¤ã¼ãµãªã¢ã ãã¼ãã«æ¥ç¶ãããã¨ãã§ãã¾ãã
> MetaMask ãæä¾ããã¤ã¼ãµãªã¢ã ãã¼ããä½¿ç¨ãã¦ãããã­ã¤ãããã³ã³ãã©ã¯ããããã¼ã¿ãéåä¿¡ããããã«ä¸è¨ã®å®è£ãè¡ãã¾ããã
>
> `ethers` ã®ã©ã¤ãã©ãªã«ãã `provider` ã®ã¤ã³ã¹ã¿ã³ã¹ãæ°è¦ä½æãã¦ãã¾ãã

**II\. `signer`**

> ```javascript
> // App.js
> const signer = provider.getSigner();
> ```
>
> `signer` ã¯ãã¦ã¼ã¶ã¼ã®ã¦ã©ã¬ããã¢ãã¬ã¹ãæ½è±¡åãããã®ã§ãã
>
> `provider` ãä½æãã`provider.getSigner()` ãå¼ã³åºãã ãã§ãã¦ã¼ã¶ã¼ã¯ã¦ã©ã¬ããã¢ãã¬ã¹ãä½¿ç¨ãã¦ãã©ã³ã¶ã¯ã·ã§ã³ã«ç½²åãããã®ãã¼ã¿ãã¤ã¼ãµãªã¢ã ãããã¯ã¼ã¯ã«éä¿¡ãããã¨ãã§ãã¾ãã
>
> `provider.getSigner()` ã¯æ°ãã `signer` ã¤ã³ã¹ã¿ã³ã¹ãè¿ãã®ã§ããããä½¿ã£ã¦ç½²åä»ããã©ã³ã¶ã¯ã·ã§ã³ãéä¿¡ãããã¨ãã§ãã¾ãã

**III\. ã³ã³ãã©ã¯ãã¤ã³ã¹ã¿ã³ã¹**

> ```javascript
> // App.js
> const wavePortalContract = new ethers.Contract(
>   contractAddress,
>   contractABI,
>   signer
> );
> ```
>
> ããã§ã**ã³ã³ãã©ã¯ãã¸ã®æ¥ç¶ãè¡ã£ã¦ãã¾ãã**
>
> ã³ã³ãã©ã¯ãã®æ°ããã¤ã³ã¹ã¿ã³ã¹ãä½æããã«ã¯ãä»¥ä¸ 3 ã¤ã®å¤æ°ã `ethers.Contract` é¢æ°ã«æ¸¡ãå¿è¦ãããã¾ãã
>
> 1. ã³ã³ãã©ã¯ãã®ããã­ã¤åã®ã¢ãã¬ã¹ï¼ã­ã¼ã«ã«ããã¹ãããããã¾ãã¯ã¤ã¼ãµãªã¢ã ã¡ã¤ã³ãããï¼
> 2. ã³ã³ãã©ã¯ãã® ABI
> 3. `provider`ããããã¯ `signer`
>
> ã³ã³ãã©ã¯ãã¤ã³ã¹ã¿ã³ã¹ã§ã¯ãã³ã³ãã©ã¯ãã«æ ¼ç´ããã¦ãããã¹ã¦ã®é¢æ°ãå¼ã³åºããã¨ãã§ãã¾ãã
>
> ãããã®ã³ã³ãã©ã¯ãã¤ã³ã¹ã¿ã³ã¹ã« `provider` ãæ¸¡ãã¨ããã®ã¤ã³ã¹ã¿ã³ã¹ã¯**èª­ã¿åãå°ç¨ã®æ©è½ããå®è¡ã§ããªããªãã¾ã**ã
>
> ä¸æ¹ã`signer` ãæ¸¡ãã¨ããã®ã¤ã³ã¹ã¿ã³ã¹ã¯**èª­ã¿åãã¨æ¸ãè¾¼ã¿ã®ä¸¡æ¹ã®æ©è½ãå®è¡ã§ããããã«ãªãã¾ã**ã
>
> â» ABI ã«ã¤ãã¦ã¯ãã®ã¬ãã¹ã³ã®çµç¤ã«ã¦è©³ããèª¬æãã¾ãã

**3 \. wave ãã¿ã³ã« wave é¢æ°ãé£åããã**

```html
// App.js <button className="waveButton" onClick={wave}>Wave at Me</button>
```

`onClick` ãã­ããã `null` ãã `wave` ã«æ´æ°ãã¦ã`wave()` é¢æ°ã `waveButton` ã«æ¥ç¶ãã¦ãã¾ãã

### ð§ª ãã¹ããå®è¡ãã

ä»åã®ã¬ãã¹ã³ã§ã¯å®è£ããæ©è½ãå¤ãã®ã§ãè¿½å ããæ©è½ 3 ã¤ã«å¯¾ãã¦ãã¹ããè¡ãã¾ãã

`App.js` ãæ´æ°ããããã¿ã¼ããã«ä¸ã§ `dApp-starter-project` ã«ç§»åãã`npm run start` ãå®è¡ãã¦ãã ããã

ã­ã¼ã«ã«ãµã¼ããä»ãã¦è¡¨ç¤ºããã¦ãã Web ã¢ããªã±ã¼ã·ã§ã³ããå³ã¯ãªãã¯ â `Inspect` ãé¸æããConsole ã®åºåçµæãç¢ºèªãã¦ã¿ã¾ãããã

ä¸è¨ã®ãããªã¨ã©ã¼ãè¡¨ç¤ºããã¦ããã°ããã¹ãã¯æåã§ãã
![](/public/images/1-ETH-dApp/section-2/2_4_1.png)

ãããã `contractAddress` ã¨ `contractABI` ãè¨­å®ãã¦ããã¾ãã

### ð  `contractAddress` ã®è¨­å®

Rinkeby Test Network ã«ã³ã³ãã©ã¯ããããã­ã¤ããã¨ããä¸è¨ãã¿ã¼ããã«ã«åºåããã¦ãããã¨ãè¦ãã¦ã¾ããï¼

```
Deploying contracts with account:  0x821d451FB0D9c5de6F818d700B801a29587C3dCa
Account balance:  324443375262705541
Contract deployed to:  0x3610145E4c6C801bBf2F926DFd8FDd2cE1103493
```

`App.js` ã« `contractAddress` ãè¨­å®ããããã«ã`Contract deployed to` ã®åºåçµæï¼`0x..`ï¼ãå¿è¦ã§ãã

`Contract deployed to` ã«ç¶ãåºåçµæãã©ããã«ã¡ã¢ãã¦ããå ´åã¯ããã®ã¾ã¾ã¬ãã¹ã³ãé²ãã¾ãããã

ååº¦ãã®çµæãåºåããå ´åã¯ãã¿ã¼ããã«ä¸ã§ `my-wave-portal` ãã£ã¬ã¯ããªã«ç§»åããä¸è¨ãå®è¡ãã¦ãã ããã

```
npx hardhat run scripts/deploy.js --network rinkeby
```

ã³ã³ãã©ã¯ãã®ããã­ã¤åã®ã¢ãã¬ã¹ãåå¾ã§ãããã`App.js` ã« `contractAddress` ã¨ããæ°è¦ã®å¤æ°ãè¿½å ãã¾ãããã`Contract deployed to` ã®åºåçµæï¼`0x..`ï¼ãè¨­å®ãã¦ããã¾ãã

`const [currentAccount, setCurrentAccount] = useState("")` ã®ç´ä¸ã«`contractAddress` ãä½æãã¾ããããä»¥ä¸ã®ããã«ãªãã¾ãã

```javascript
// App.js
const [currentAccount, setCurrentAccount] = useState("");
/*
 * ããã­ã¤ãããã³ã³ãã©ã¯ãã®ã¢ãã¬ã¹ãä¿æããå¤æ°ãä½æ
 */
const contractAddress = "ããªãã® WavePortal ã® address ãè²¼ãä»ãã¦ãã ãã";
```

`App.js` ãæ´æ°ããããã­ã¼ã«ã«ãµã¼ãã«ãã¹ãããã¦ãã Web ã¢ããªã±ã¼ã·ã§ã³ãã Console ãç¢ºèªãã¦ã¿ã¾ãããã

`contractAddress` ã«é¢ããã¨ã©ã¼ãæ¶ãã¦ããã°ãæåã§ãã
![](/public/images/1-ETH-dApp/section-2/2_4_2.png)

### ð ABI ãã¡ã¤ã«ãåå¾ãã

ABI (Application Binary Interface) ã¯ã³ã³ãã©ã¯ãã®åãæ±ãèª¬ææ¸ã®ãããªãã®ã§ãã

Web ã¢ããªã±ã¼ã·ã§ã³ãã³ã³ãã©ã¯ãã¨éä¿¡ããããã«å¿è¦ãªæå ±ããABI ãã¡ã¤ã«ã«å«ã¾ãã¦ãã¾ãã

ã³ã³ãã©ã¯ãä¸ã¤ä¸ã¤ã«ã¦ãã¼ã¯ãª ABI ãã¡ã¤ã«ãç´ã¥ãã¦ããããã®ä¸­ã«ã¯ä¸è¨ã®æå ±ãå«ã¾ãã¦ãã¾ãã

1. ãã®ã³ã³ãã©ã¯ãã«ä½¿ç¨ããã¦ããé¢æ°ã®åå
2. ããããã®é¢æ°ã«ã¢ã¯ã»ã¹ããããå¿è¦ãªãã©ã¡ã¼ã¿ã¨ãã®å
3. é¢æ°ã®å®è¡çµæã«å¯¾ãã¦è¿ããã¼ã¿åã®ç¨®é¡

ABI ãã¡ã¤ã«ã¯ãã³ã³ãã©ã¯ããã³ã³ãã¤ã«ãããæã«çæããã`artifacts` ãã£ã¬ã¯ããªã«èªåçã«æ ¼ç´ããã¾ãã

ã¿ã¼ããã«ã§ `my-wave-portal` ãã£ã¬ã¯ããªã«ç§»åãã`ls` ãå®è¡ãã¾ãããã

`artifacts` ãã£ã¬ã¯ããªã®å­å¨ãç¢ºèªãã¦ãã ããã

ABI ãã¡ã¤ã«ã®ä¸­èº«ã¯ã`WavePortal.json` ã¨ãããã¡ã¤ã«ã«æ ¼ç´ããã¦ãã¾ãã

ä¸è¨ãå®è¡ãã¦ãABI ãã¡ã¤ã«ãã³ãã¼ãã¾ãããã

1. ã¿ã¼ããã«ä¸ã§ `my-wave-portal` ã«ãããã¨ãç¢ºèªããï¼ãããã¯ç§»åããï¼ã

2. ã¿ã¼ããã«ä¸ã§ä¸è¨ãå®è¡ãã`WavePortal.json` ãéãã¾ããããâ» ãã¡ã¤ã³ãã¼ããç´æ¥éããã¨ãå¯è½ã§ãã

   > ```
   > code artifacts/contracts/WavePortal.sol/WavePortal.json
   > ```

3. VS Code ã§ `WavePortal.json` ãã¡ã¤ã«ãéãããã®ã§ãä¸­èº«ããã¹ã¦ã³ãã¼ãã¾ããããâ» VS Code ã®ãã¡ã¤ã³ãã¼ãä½¿ã£ã¦ãç´æ¥ `WavePortal.json` ãéããã¨ãå¯è½ã§ãã

æ¬¡ã«ãä¸è¨ãå®è¡ãã¦ãABI ãã¡ã¤ã«ã Web ã¢ããªã±ã¼ã·ã§ã³ããå¼ã³åºããããã«ãã¾ãããã

1. ã¿ã¼ããã«ä¸ã§ `dApp-starter-project/src` ã«ç§»åããã

2. ä¸è¨ãå®è¡ãã¦ã`dApp-starter-project/src/` ã®ä¸­ã« `utils` ãã£ã¬ã¯ããªãä½æããã

> ```bash
> mkdir utils
> ```

1. `utils` ãã£ã¬ã¯ããªã«ç§»åãã¦ `WavePortal.json` ãã¡ã¤ã«ãä½æããã

> ```bash
> touch WavePortal.json
> ```

1. ä¸è¨ãå®è¡ãã¦ã`WavePortal.json` ãã¡ã¤ã«ã VS Code ã§éãã

> ```bash
> code dApp-starter-project/src/utils/WavePortal.json
> ```

5. **åã»ã©ã³ãã¼ãã `my-wave-portal/artifacts/contracts/WavePortal.sol/WavePortal.json` ã®ä¸­èº«ãæ°ããä½æãã `dApp-starter-project/src/utils/WavePortal.json` ã®ä¸­ã«è²¼ãä»ãã¦ãã ããã**

ABI ãã¡ã¤ã«ã®æºåãã§ããã®ã§ã`App.js` ã«ã¤ã³ãã¼ããã¾ãããã

ä¸è¨ã®ããã« `App.js` ãæ´æ°ãã¾ãã

```javascript
import React, { useEffect, useState } from "react";
import "./App.css";
/* ethers å¤æ°ãä½¿ããããã«ãã*/
import { ethers } from "ethers";
/* ABIãã¡ã¤ã«ãå«ãWavePortal.jsonãã¡ã¤ã«ãã¤ã³ãã¼ããã*/
import abi from "./utils/WavePortal.json";
const App = () => {
  /*
   * ã¦ã¼ã¶ã¼ã®ãããªãã¯ã¦ã©ã¬ãããä¿å­ããããã«ä½¿ç¨ããç¶æå¤æ°ãå®ç¾©ãã¾ãã
   */
  const [currentAccount, setCurrentAccount] = useState("");
  console.log("currentAccount: ", currentAccount);
  /*
   * ããã­ã¤ãããã³ã³ãã©ã¯ãã®ã¢ãã¬ã¹ãä¿æããå¤æ°ãä½æ
   */
  const contractAddress = "ããªãã®ã³ã³ãã©ã¯ãã¢ãã¬ã¹ãè²¼ãä»ãã¦ãã ãã";
  /*
   * ABIã®åå®¹ãåç§ããå¤æ°ãä½æ
   */
  const contractABI = abi.abi;

  /*
   * window.ethereumã«ã¢ã¯ã»ã¹ã§ãããã¨ãç¢ºèªãã¾ãã
   */
  const checkIfWalletIsConnected = async () => {
    try {
      const { ethereum } = window;
      if (!ethereum) {
        console.log("Make sure you have MetaMask!");
        return;
      } else {
        console.log("We have the ethereum object", ethereum);
      }
      /*
       * ã¦ã¼ã¶ã¼ã®ã¦ã©ã¬ããã¸ã®ã¢ã¯ã»ã¹ãè¨±å¯ããã¦ãããã©ãããç¢ºèªãã¾ãã
       */
      const accounts = await ethereum.request({ method: "eth_accounts" });
      if (accounts.length !== 0) {
        const account = accounts[0];
        console.log("Found an authorized account:", account);
        setCurrentAccount(account);
      } else {
        console.log("No authorized account found");
      }
    } catch (error) {
      console.log(error);
    }
  };
  /*
   * connectWalletã¡ã½ãããå®è£
   */
  const connectWallet = async () => {
    try {
      const { ethereum } = window;
      if (!ethereum) {
        alert("Get MetaMask!");
        return;
      }
      const accounts = await ethereum.request({
        method: "eth_requestAccounts",
      });
      console.log("Connected: ", accounts[0]);
      setCurrentAccount(accounts[0]);
    } catch (error) {
      console.log(error);
    }
  };
  /*
   * waveã®åæ°ãã«ã¦ã³ãããé¢æ°ãå®è£
   */
  const wave = async () => {
    try {
      const { ethereum } = window;
      if (ethereum) {
        const provider = new ethers.providers.Web3Provider(ethereum);
        const signer = provider.getSigner();
        /*
         * ABIãåç§
         */
        const wavePortalContract = new ethers.Contract(
          contractAddress,
          contractABI,
          signer
        );
        let count = await wavePortalContract.getTotalWaves();
        console.log("Retrieved total wave count...", count.toNumber());
        /*
         * ã³ã³ãã©ã¯ãã«ðï¼waveï¼ãæ¸ãè¾¼ãã
         */
        const waveTxn = await wavePortalContract.wave();
        console.log("Mining...", waveTxn.hash);
        await waveTxn.wait();
        console.log("Mined -- ", waveTxn.hash);
        count = await wavePortalContract.getTotalWaves();
        console.log("Retrieved total wave count...", count.toNumber());
      } else {
        console.log("Ethereum object doesn't exist!");
      }
    } catch (error) {
      console.log(error);
    }
  };

  /*
   * WEBãã¼ã¸ãã­ã¼ããããã¨ãã«ä¸è¨ã®é¢æ°ãå®è¡ãã¾ãã
   */
  useEffect(() => {
    checkIfWalletIsConnected();
  }, []);
  return (
    <div className="mainContainer">
      <div className="dataContainer">
        <div className="header">
          <span role="img" aria-label="hand-wave">
            ð
          </span>{" "}
          WELCOME!
        </div>
        <div className="bio">
          ã¤ã¼ãµãªã¢ã ã¦ã©ã¬ãããæ¥ç¶ãã¦ãã
          <span role="img" aria-label="hand-wave">
            ð
          </span>
          (wave)ããéã£ã¦ãã ãã
          <span role="img" aria-label="shine">
            â¨
          </span>
        </div>
        {/*
         * waveãã¿ã³ã«waveé¢æ°ãé£åãããã
         */}
        <button className="waveButton" onClick={wave}>
          Wave at Me
        </button>
        {/*
         * ã¦ã©ã¬ããã³ãã¯ãã®ãã¿ã³ãå®è£
         */}
        {!currentAccount && (
          <button className="waveButton" onClick={connectWallet}>
            Connect Wallet
          </button>
        )}
        {currentAccount && (
          <button className="waveButton" onClick={connectWallet}>
            Wallet Connected
          </button>
        )}
      </div>
    </div>
  );
};
export default App;
```

ã³ã³ãã©ã¯ãã¢ãã¬ã¹ããèªèº«ã®ãã®ã«æ´æ°ããã®ããå¿ããªã!

```javascript
// App.js
const contractAddress = "ããªãã®ã³ã³ãã©ã¯ãã¢ãã¬ã¹ãè²¼ãä»ãã¦ãã ãã";
```

æ°ããå®è£ããããæ©è½ã¯ä¸è¨ã® 3 ã¤ã§ãã

**1 \. ABI ãã¡ã¤ã«ãå«ã WavePortal.json ãã¡ã¤ã«ãã¤ã³ãã¼ããã**

```javascript
// App.js
import abi from "./utils/WavePortal.json";
```

**2 \. ABI ã®åå®¹ãåç§ããå¤æ°ãä½æ**

```javascript
// App.js
const contractABI = abi.abi;
```

ABI ã®åç§åãç¢ºèªãã¾ãããã`wave` é¢æ°ã®ä¸­ã«å®è£ããã¦ãã¾ãã

```javascript
// App.jss
const wave = async () => {
  try {
    const { ethereum } = window;
    if (ethereum) {
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      /*
       * ABIãããã§åç§
       */
      const wavePortalContract = new ethers.Contract(
        contractAddress,
        contractABI,
        signer
      );
      let count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count...", count.toNumber());
    } else {
      console.log("Ethereum object doesn't exist!");
    }
  } catch (error) {
    console.log(error);
  }
};
```

ABI ãã¡ã¤ã«ã `App.js` ã«è¿½å ããã¨ããã­ã³ãã¨ã³ãã§ `Wave` ãã¿ã³ãã¯ãªãã¯ãããã¨ãã**ãã­ãã¯ãã§ã¼ã³ä¸ã®ã³ã³ãã©ã¯ãããæ­£å¼ã«ãã¼ã¿ãèª­ã¿åããã¨ãã§ãã¾ã**ã

**3 \. ãã¼ã¿ããã­ãã¯ãã§ã¼ã³ã«æ¸ãè¾¼ã**

ã³ã³ãã©ã¯ãã«ãã¼ã¿ãæ¸ãè¾¼ãããã®ã³ã¼ããå®è£ãã¾ããã

```javascript
// App.js
const wave = async () => {
  try {
    const { ethereum } = window;
    if (ethereum) {
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      const wavePortalContract = new ethers.Contract(
        contractAddress,
        contractABI,
        signer
      );
      let count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count...", count.toNumber());
      /*
       * ã³ã³ãã©ã¯ãã«ðï¼waveï¼ãæ¸ãè¾¼ãããããã...
       */
      const waveTxn = await wavePortalContract.wave();
      console.log("Mining...", waveTxn.hash);
      await waveTxn.wait();
      console.log("Mined -- ", waveTxn.hash);
      count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count...", count.toNumber());
      /*-- ããã¾ã§ --*/
    } else {
      console.log("Ethereum object doesn't exist!");
    }
  } catch (error) {
    console.log(error);
  }
};
```

ã³ã³ãã©ã¯ãã«ãã¼ã¿ãæ¸ãè¾¼ãã³ã¼ãã¯ããã¼ã¿ãèª­ã¿è¾¼ãã³ã¼ãã«ä¼¼ã¦ãã¾ãã

ä¸»ãªéãã¯ãã³ã³ãã©ã¯ãã«æ°ãããã¼ã¿ãæ¸ãè¾¼ãã¨ãã¯ããã¤ãã¼ã«éç¥ãéããããã®ãã©ã³ã¶ã¯ã·ã§ã³ã®æ¿èªãæ±ãããããã¨ã§ãã

ãã¼ã¿ãèª­ã¿è¾¼ãã¨ãã¯ããã®ãããªãã¨ãããå¿è¦ã¯ããã¾ããã
ãã£ã¦ããã­ãã¯ãã§ã¼ã³ããã®ãã¼ã¿ã®èª­ã¿åãã¯ç¡æã§ãã

### ð ãã¹ããå®è¡ãã

ã¿ã¼ããã«ä¸ã§ `dApp-starter-project` ã«ç§»åããä¸è¨ãå®è¡ãã¾ãããã

```
npm run start
```

ã­ã¼ã«ã«ãµã¼ãä¸ã§è¡¨ç¤ºããã¦ãã Web ã¢ããªã±ã¼ã·ã§ã³ã§ `Inspect` ãå®è¡ããä»¥ä¸ãè©¦ãã¦ã¿ã¾ãããã

1 \. `Connect Wallet` ããã¿ã³ãæ¼ãã¦ãWeb ã¢ããªã±ã¼ã·ã§ã³ã«ããªãã® MetaMask ã®ã¦ã©ã¬ããã¢ãã¬ã¹ãæ¥ç¶ããã

2 \. `Wave at Me` ãã¿ã³ãæ¼ãã¦ãå®éã«ãã­ãã¯ãã§ã¼ã³ä¸ã«ããªãã®ãðï¼waveï¼ããåæ ããã¦ãããç¢ºèªããã

ãã¤ãã®ããã«ã­ã¼ã«ã«ãµã¼ãã«ãã¹ãããã¦ãã Web ã¢ããªã±ã¼ã·ã§ã³ã `Inspect` ããConsole ãç¢ºèªãã¾ãããã

ä¾ï¼`Wave at Me` ãã¿ã³ã 2 åæ¼ããéã«åºåããã  Console ã®çµæã

![](/public/images/1-ETH-dApp/section-2/2_4_3.png)

ããããã® `Wave` ãã«ã¦ã³ããããæ¿èªããã¦ãããã¨ãç¢ºèªã§ããããæ¬¡ã®ã¹ãããã«é²ã¿ã¾ãããã

ã¿ã¼ããã«ãéããã¨ãã¯ãä»¥ä¸ã®ã³ãã³ããä½¿ãã¾ã âï¸

- Mac: `ctrl + c`
- Windows: `ctrl + shift + w`

### ð± Etherscan ã§ãã©ã³ã¶ã¯ã·ã§ã³ãç¢ºèªãã

ããªãã® Console ã«åºåããã¦ããä»¥ä¸ã®ã¢ãã¬ã¹ãããããã³ãã¼ãã¦ã[Etherscan](https://rinkeby.etherscan.io/) ã«è²¼ãä»ãã¦ã¿ã¾ãããã

- Connected: `0x..` â ãããã³ãã¼ãã¦ Etherscan ã«è²¼ãä»ãã

  ð ããªãã® Rinkeby Test Network ä¸ã®ãã©ã³ã¶ã¯ã·ã§ã³ã®å±¥æ­´ãåç§ã§ãã¾ãã

- Mined -- `0x..` â ãããã³ãã¼ãã¦ Etherscan ã«è²¼ãä»ãã

  ð ããªãã® Web ã¢ããªã±ã¼ã·ã§ã³ãä»ãã¦ Rinkeby Test Network ä¸ã«æ¸ãè¾¼ã¾ãããðï¼waveï¼ãã«å¯¾ãããã©ã³ã¶ã¯ã·ã§ã³ã®å±¥æ­´ãåç§ã§ãã¾ãã

### ðââï¸ è³ªåãã

ããã¾ã§ã®ä½æ¥­ã§ä½ãããããªããã¨ãããå ´åã¯ãDiscord ã® `#section-2` ã§è³ªåããã¦ãã ããã

ãã«ããããã¨ãã®ãã­ã¼ãåæ»ã«ãªãã®ã§ãã¨ã©ã¼ã¬ãã¼ãã«ã¯ä¸è¨ã® 3 ç¹ãè¨è¼ãã¦ãã ãã â¨

```
1. è³ªåãé¢é£ãã¦ããã»ã¯ã·ã§ã³çªå·ã¨ã¬ãã¹ã³çªå·
2. ä½ããããã¨ãã¦ããã
3. ã¨ã©ã¼æãã³ãã¼&ãã¼ã¹ã
4. ã¨ã©ã¼ç»é¢ã®ã¹ã¯ãªã¼ã³ã·ã§ãã
```

---

ããã§ã¨ããããã¾ãï¼ãã»ã¯ã·ã§ã³ 2 ãçµäºãã¾ããï¼ `#section-2` ã«ããªãã® Etherscan ã®ãªã³ã¯ãè²¼ãä»ãã¦ãã³ãã¥ããã£ã§é²æãç¥ãã¾ããã ð
Etherscan ã§ãã©ã³ã¶ã¯ã·ã§ã³ã®ç¢ºèªãããããæ¬¡ã®ã¬ãã¹ã³ã«é²ãã§ãã ãã ð
