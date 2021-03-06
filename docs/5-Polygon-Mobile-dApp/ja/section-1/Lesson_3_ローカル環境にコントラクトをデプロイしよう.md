### ð£ ã­ã¼ã«ã«ç°å¢ã§ã³ã³ãã©ã¯ããã³ã³ãã¤ã«ãã¦ããã­ã¤ãã

åã®ã»ã¯ã·ã§ã³ã§ã¹ãã¼ãã³ã³ãã©ã¯ããæ¸ããã®ã§ãæ¬¡ã¯ãããã³ã³ãã¤ã«ãã¾ãã

ã¿ã¼ããã«ã `todo-dApp-contract` ãã£ã¬ã¯ããªã§éããä»¥ä¸ã®ã³ãã³ããå®è¡ãã¾ãã

```bash
truffle compile
```

`truffle compile` ã§ã¨ã©ã¼ãåºãå ´åã¯ä¸è¨ããè©¦ããã ããã

```bash
npx truffle compile
```

ä¸ã®ããã«ã¿ã¼ããã«ã«è¡¨ç¤ºããã¦ããã°æåã§ãã

![](/public/images/5-Polygon-Mobile-dApp/section-1/1_3_01.png)

ãã­ãã¯ãã§ã¼ã³ã«ã³ã³ãã©ã¯ããç§»è¡ãã¾ãã`migrations` ãã£ã¬ã¯ããªã«ç§»åãã`2_todo_contract_migration.js` ã¨ãããã¡ã¤ã«ãæ°è¦ä½æããä»¥ä¸ã®ã³ã¼ããè¿½å ãã¦ãã ããã

```js
// 2_todo_contract_migration.js
const TodoContract = artifacts.require("TodoContract");

module.exports = function (deployer) {
    deployer.deploy(TodoContract);
};
```

ç§»è¡ä½æ¥­ãéå§ããåã«ã`Ganache` ãã¤ã³ã¹ãã¼ã«ããã¦ãããã¨ãç¢ºèªãã¦ãã ãããã·ã¹ãã ã§ `Ganache GUI` ã¢ããªãèµ·åãã¾ãã

`truffle-config.js` ã®æ¢å­ã®åå®¹ããã¹ã¦åé¤ããä»¥ä¸ã®ã³ã¼ãã«ç½®ãæãã¦ãã ããã

```js
//truffle-config.js
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 7545,
      network_id: "*",
    },
  },
  contracts_directory: "./contracts",
  compilers: {
    solc: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },

  db: {
    enabled: false,
  },
};
```

å°ãèª¬æãã¾ãã

`truffle-config.js` ã§ã¯ãTruffleã®åºæ¬çãªæ§æãå®ç¾©ãã¦ãã¾ãã

```js
//truffle-config.js
  networks: {
    development: {
      host: "localhost",
      port: 7545,
      network_id: "*",
    },
  },
```

ç¾å¨ãGanacheãã­ãã¯ãã§ã¼ã³ãç¨¼åãã¦ãã `localhost:7545` ã«ã¹ãã¼ãã³ã³ãã©ã¯ããããã­ã¤ããããæå®ãã¦ãã¾ãã

ããã§ã¯ãGanacheä¸ã«ã¹ãã¼ãã³ã³ãã©ã¯ããããã­ã¤ããããã«ãã³ãã³ããå®è¡ãã¦ããã¾ãã

```bash
truffle migrate
```

`truffle migrate` ã§ã¨ã©ã¼ãåºãå ´åã¯ä¸è¨ããè©¦ããã ããã

```bash
npx truffle migrate
```

ãã¡ãã§ãã¨ã©ã¼ãåºãå ´åã¯ã`truffle-config.js` ãã¡ã¤ã«ãä¸è¨ã«å¤æ´ãã¦ãããä¸åº¦ä¸è¨ã®æä½ãè¡ã£ã¦ãã ããã

```js
//truffle-config.js
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 7545,
      network_id: "*",
    },
  },
  contracts_directory: "./contracts",

  compilers: {
    solc: {
      version: "0.8.11",
      optimizer: {
        enabled: true,
        runs: 200,
      },
    }
  },
  db: {
    enabled: false,
  },
};
```
ä¸ã®ããã«ã¿ã¼ããã«ã«è¡¨ç¤ºããã¦ããã°æåã§ãã

![](/public/images/5-Polygon-Mobile-dApp/section-1/1_3_02.png)

ã­ã¼ã«ã«ãããã¯ã¼ã¯ã¸ã®ã³ã³ãã¤ã«ã¨ããã­ã¤ã®ä½æ¥­ã¯ä»¥ä¸ã§å®äºã§ãã
æ¬¡ã¯ãFlutterã¢ããªã±ã¼ã·ã§ã³ã¸æ¥ç¶ãã¦ããã¾ãããã
### ðââï¸ è³ªåãã

ããã¾ã§ã®ä½æ¥­ã§ä½ãããããªããã¨ãããå ´åã¯ãDiscord ã® `#section-1` ã§è³ªåããã¦ãã ããã

ãã«ããããã¨ãã®ãã­ã¼ãåæ»ã«ãªãã®ã§ãã¨ã©ã¼ã¬ãã¼ãã«ã¯ä¸è¨ã® 3 ç¹ãè¨è¼ãã¦ãã ãã â¨

```
1. è³ªåãé¢é£ãã¦ããã»ã¯ã·ã§ã³çªå·ã¨ã¬ãã¹ã³çªå·
2. ä½ããããã¨ãã¦ããã
3. ã¨ã©ã¼æãã³ãã¼&ãã¼ã¹ã
4. ã¨ã©ã¼ç»é¢ã®ã¹ã¯ãªã¼ã³ã·ã§ãã
```

---
ã¿ã¼ããã«ã®åºåçµæã Discord ã® `#section-1` ã«æç¨¿ãã¦ãã³ãã¥ããã£ã«ã·ã§ã¢ãã¦ãã ãã!

æ¬¡ã®ã»ã¯ã·ã§ã³ã«é²ãã§ãFlutterã¢ããªã±ã¼ã·ã§ã³ã¸ã®æ¥ç¶ãéå§ãã¾ããã ð
