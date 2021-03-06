###  ð¥ ãã®ã¬ãã¹ã³ã®åèåç»URL
[Dapp University](https://youtu.be/CgXQC4dbGUE?t=2894)

### âï¸ ã¹ãã¼ãã³ã³ãã©ã¯ãã®ãã¹ããä½æãã

ã¾ããã¿ã¼ããã«ãéãã¦ã`yield-farm-starter-project` ãã£ã¬ã¯ããªã«ãããã¨ãç¢ºèªããä»¥ä¸ã®ã³ãã³ããå®è¡ãã¾ãããã

```bash
mkdir test
```
```bash
touch test/TokenFarm_test.js
```

ããã§ã¯ã`yield-farm-starter-project` ãã£ã¬ã¯ããªã®ä¸­ã« `test` ãã©ã«ããä½æãããã®ä¸­ã«ãã¹ãç¨ã®ãã¡ã¤ã« (`TokenFarm_test.js`) ãä½æãã¦ãã¾ãã

ã¹ãã¼ãã³ã³ãã©ã¯ãã¯ãä¸åº¦ãã­ãã¯ãã§ã¼ã³ä¸ã«ããã­ã¤ããã¨å¤æ´ã§ããªããããã³ã¼ããåé¡ãªãåä½ãããã¨ããã¹ããããã¨ãéè¦ã§ãã

ã§ã¯ `TokenFarm_test.js` ãéãã¦ä»¥ä¸ã®ã³ã¼ããè¿½å ãã¦ããã¾ãããã

```javascript
// TokenFarm_test.js

// ä½¿ç¨ããã¹ãã¼ãã³ã³ãã©ã¯ããã¤ã³ãã¼ããã
const DappToken = artifacts.require(`DappToken`)
const DaiToken = artifacts.require(`DaiToken`)
const TokenFarm = artifacts.require(`TokenFarm`)

// chai ã®ãã¹ãã©ã¤ãã©ãªã»ãã¬ã¼ã ã¯ã¼ã¯ãèª­ã¿è¾¼ã
const { assert } = require('chai');
require(`chai`)
    .use(require('chai-as-promised'))
    .should()

// ä»»æã®ETHã®å¤ãWeiã«å¤æããé¢æ°
function tokens(n) {
    return web3.utils.toWei(n, 'ether');
}

contract('TokenFarm', ([owner, investor]) => {
    let daiToken, dappToken, tokenFarm

    before(async () =>{
        //ã³ã³ãã©ã¯ãã®èª­ã¿è¾¼ã¿
        daiToken = await DaiToken.new()
        dappToken = await DappToken.new()
        tokenFarm = await TokenFarm.new(dappToken.address, daiToken.address)

        //å¨ã¦ã®Dappãã¼ã¯ã³ããã¡ã¼ã ã«ç§»åãã(1 million)
        await dappToken.transfer(tokenFarm.address, tokens('1000000'));

        await daiToken.transfer(investor, tokens('100'), {from: owner})
    })

    // DaiToken
    describe('Mock DAI deployment', async () => {
        // ãã¹ã1
        it('has a name', async () => {
            const name = await daiToken.name()
            assert.equal(name, 'Mock DAI Token')
        })
    })

    // DappToken
    describe('Dapp Token deployment', async () => {
         // ãã¹ã2
        it('has a name', async () => {
            const name = await dappToken.name()
            assert.equal(name, 'DApp Token')
        })
    })

    // TokenFarm
    describe('Token Farm deployment', async () => {
         // ãã¹ã3
        it('has a name', async () => {
            const name = await tokenFarm.name()
            assert.equal(name, "Dapp Token Farm")
        })

        // ãã¹ã4
        it('contract has tokens', async () => {
            let balance = await dappToken.balanceOf(tokenFarm.address)
            assert.equal(balance.toString(), tokens('1000000'))
        })
    })
})
```

ã³ã¼ããä¸ã¤ä¸ã¤ã¿ã¦ããã¾ãã

æåã«ããã¹ããããã¹ãã¼ãã³ã³ãã©ã¯ããå¼ã³åºãã¾ãã

```javascript
// TokenFarm_test.js
// ä½¿ç¨ããã¹ãã¼ãã³ã³ãã©ã¯ããã¤ã³ãã¼ããã
const DappToken = artifacts.require(`DappToken`)
const DaiToken = artifacts.require(`DaiToken`)
const TokenFarm = artifacts.require(`TokenFarm`)
```

æ¬¡ã«ããã¹ãã«ä½¿ç¨ããã©ã¤ãã©ãªãèª­ã¿è¾¼ã¿ã¾ãã

```javascript
// TokenFarm_test.js
const { assert } = require('chai');
require(`chai`)
    .use(require('chai-as-promised'))
    .should()
```

ä»åãã¹ãã«ä½¿ç¨ãã `chai` ã¯ãtruffle ãæä¾ãããã¹ãç¨ã®ã©ã¤ãã©ãªã§ãã
- `yield-farm-starter-project/package.json` ã®ä¸­ã«ãä½¿ç¨ãã `chai` ã®ãã¼ã¸ã§ã³ãè¨è¼ããã¦ãã¾ãã
- ãã¹ãã«ã¯ã`chai` ã ãã§ãªãã`mocha` ã¨ããtruffle ã«å«ã¾ãã¦ãã JavaScript ç¨ã®ãã¹ããã¬ã¼ã ã¯ã¼ã¯ãä½¿ç¨ãã¾ãã
- `chai` ã«ã¤ãã¦è©³ããç¥ãããæ¹ã¯ã[ãã¡ã](https://www.chaijs.com/)ã®å¬å¼ãã­ã¥ã¡ã³ãããè¦§ãã ããð«
- `mocha` ã«ã¤ãã¦è©³ããç¥ãããæ¹ã¯ã[ãã¡ã](https://mochajs.org/)ã®å¬å¼ãã­ã¥ã¡ã³ãããè¦§ãã ããâï¸

æ¬¡ã«ãä»»æã® ETH ã®å¤ã Wei ã«å¤æãããã«ãã¼é¢æ° `token()` ãå®ç¾©ãã¾ãã

```javascript
// TokenFarm_test.js
// ä»»æã®ETHã®å¤ãWeiã«å¤æããé¢æ°
function tokens(n) {
    return web3.utils.toWei(n, 'ether');
}
```

æ¬¡ã«ããã¤ã°ã¬ã¼ã·ã§ã³ã³ã¼ããè¨è¼ãã¾ãã

```javascript
// TokenFarm_test.js
contract('TokenFarm', ([owner, investor]) => {
    let daiToken, dappToken, tokenFarm

    before(async () =>{
        //ã³ã³ãã©ã¯ãã®èª­ã¿è¾¼ã¿
        daiToken = await DaiToken.new()
        dappToken = await DappToken.new()
        tokenFarm = await TokenFarm.new(dappToken.address, daiToken.address)

        //å¨ã¦ã®Dappãã¼ã¯ã³ããã¡ã¼ã ã«ç§»åãã(1 million)
        await dappToken.transfer(tokenFarm.address, tokens('1000000'));

        //100 Dai ãã¼ã¯ã³ããã¡ã¼ã ã«ç§»åãã
        await daiToken.transfer(investor, tokens('100'), {from: owner})
    })
```

ãããã®ã³ã¼ãã¯ãåæã«è¤æ°ã®ãã¹ããæ¸ãã®ã«ä¾¿å©ã§ãã

`contract` ã®ä¸­ã®å¼æ°ã¯ãããããä»¥ä¸ã®ãããªå¤æ°ãå«ã¾ãã¾ãã
- `owner`: Dapp ãã¼ã¯ã³ããã­ãã¯ãã§ã¼ã³ã«ããã­ã¤ããäººã®ã¢ãã¬ã¹
- `investor`: Token Farm ã« Dai ãé ããäººã®ã¢ãã¬ã¹

ã¾ãã3ã¤ã®ã¹ãã¼ãã³ã³ãã©ã¯ãã«é¢é£ä»ããã­ã¼ã«ã«å¤æ°ãããããä½æãã¾ãã

```javascript
// TokenFarm_test.js
let daiToken, dappToken, tokenFarm
```

æ¬¡ã«ã`before` ããã¯ãä½æãã¾ãã

```javascript
// TokenFarm_test.js
before(async () =>{})
```

`before` ããã¯ã¯ãåãã¹ãã®åã«å®è¡ãããé¢æ°ã§ãã

`before` ããã¯ã®åé¨ã§ã¯ã3ã¤ã®ã¹ãã¼ãã³ã³ãã©ã¯ããèª­ã¿è¾¼ã¿ã¾ãã

```javascript
// TokenFarm_test.js
//ã³ã³ãã©ã¯ãã®èª­ã¿è¾¼ã¿
daiToken = await DaiToken.new()
dappToken = await DappToken.new()
tokenFarm = await TokenFarm.new(dappToken.address, daiToken.address)
```

ããã¦ããã¹ã¦ã® Dapp ãã¼ã¯ã³ã¨ 100 Dai ãã¼ã¯ã³ããããããã¼ã¯ã³ãã¡ã¼ã ã«è»¢éãã¾ãã

```javascript
// TokenFarm_test.js
//å¨ã¦ã®Dappãã¼ã¯ã³ããã¡ã¼ã ã«ç§»åãã(1 million)
await dappToken.transfer(tokenFarm.address, tokens('1000000'));

//100 Dai ãã¼ã¯ã³ããã¡ã¼ã ã«ç§»åãã
await daiToken.transfer(investor, tokens('100'), {from: owner})
```

ä»»æã® ETH ã®å¤ã Wei ã«å¤æãã `token()` é¢æ°ãããã§ä½¿ç¨ããã¦ãã¾ãã­ð

æå¾ã«ã3ã¤ã®ã¹ãã¼ãã³ã³ãã©ã¯ããæ­£å¸¸ã«ãã­ãã¯ãã§ã¼ã³ä¸ã«ããã­ã¤ããã¦ããããã¹ããã¾ãã

`TokenFarm_test.js` ã®ãã¹ã1-3ã§ãã£ã¦ãããã¨ã¯ã»ã¼åãã§ãã

ãã¹ã1ãä¾ã«ãä½ããã£ã¦ããã®ãè©³ããè¦ã¦ããã¾ãããã

```javascript
// TokenFarm_test.js
// DaiToken
describe('Mock DAI deployment', async () => {
    // ãã¹ã1
    it('has a name', async () => {
        const name = await daiToken.name()
        assert.equal(name, 'Mock DAI Token')
    })
})
```

ãã¹ã1ã¯ãDaiToken ã³ã³ãã©ã¯ãããããã¯ã¼ã¯ã«æ­£å¸¸ã«ããã­ã¤ããããã©ããããã¹ãã¦ãã¾ãã

ãã¹ã1ãæåããå ´åããã¹ã1ã¯ ããã­ã¤ãããã³ã³ãã©ã¯ãã "Mock DAI Token" ã¨ããååãæã£ã¦ãããã¨ãç¤ºãã¾ãã

æå¾ã«ãTokenFarm ã« Dapp ãã¼ã¯ã³ãè»¢éããã¦ãããã¨ãç¢ºèªããããã«ãã¹ã4ãä½æãã¾ãã

```javascript
// TokenFarm_test.js
// ãã¹ã4
it('contract has tokens', async () => {
    let balance = await dappToken.balanceOf(tokenFarm.address)
    assert.equal(balance.toString(), tokens('1000000'))
})
```

ãã¹ã4ãæåããã¨ãToken Farm ã 100ä¸ Dapp Token ãæã£ã¦ããã¨è¡¨ç¤ºãããã¯ãã§ãã

> ð: truffle console ã¨ããã¹ãã³ã¼ãã®éã
>
>ååã®ã¬ãã¹ã³ã§ã¯ãtruffle console ãä½¿ã£ã¦ãé¸æããã³ã³ãã©ã¯ãããã­ãã¯ãã§ã¼ã³ä¸ã§æ­£ããåä½ãã¦ããããç¢ºèªãã¾ãããã `TokenFarm_test.js` ã®åå®¹ãæ¯ãè¿ã£ã¦ã¿ãã¨ããã£ã¦ãããã¨ã¯ã»ã¨ãã©åãã§ãã
>
> ãããã`TokenFarm_test.js` ã®ãããªãã¹ãã³ã¼ãã§ã¯ãã³ã³ãã©ã¯ãåã®ãã¹ã¦ã®é¢æ°ã¨ãã­ããã£ãä¸åº¦ã«ãã¹ããããã¨ãã§ãã¾ãã
>
> ãã¹ãã³ã¼ãã¯ã1ã¤ã®ãã¡ã¤ã«ã§åæ©è½ã®ãã¹ããå®è¡ã§ãããããä¾¿å©ãªã®ã§ãã©ãã©ãéçºã«åãå¥ãã¦ããã¾ãããã

ä»¥ä¸ã§ãã¹ãã®èª¬æã¯çµäºã§ããããã§ã¯ããã¹ããå®è¡ãã¦ããã¾ãããï¼
### ð§ª ãã¹ããå®è¡ãã

ããã§ã¯ããã¹ãå®è¡ãã¾ãããã

ã¿ã¼ããã«ãéãã¦ã`yield-farm-starter-project` ãã£ã¬ã¯ããªã«ãããã¨ãç¢ºèªããä»¥ä¸ã®ã³ãã³ããå®è¡ãã¦ãã ããã

```bash
truffle test
```

ã¿ã¼ããã«ã«ä»¥ä¸ã®ãããªçµæãè¿ã£ã¦ãã¦ããã°ãã¹ãæåã§ãï¼

```bash
Contract: TokenFarm
    Mock DAI deployment
      â has a name (57ms)
    Dapp Token deployment
      â has a name (63ms)
    Token Farm deployment
      â has a name (53ms)
      â contract has tokens (48ms)


  4 passing (866ms)
```
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
ã¹ãã¼ãã³ã³ãã©ã¯ãã®ãã¹ãã¯åºæ¥ã¾ãããï¼æ¬¡ããã¯ããããã¹ãã¼ã­ã³ã°ã®ã·ã¹ãã ãä½ã£ã¦ããã¾ãã
