###  ð¥ ãã®ã¬ãã¹ã³ã®åèåç»URL
[Dapp University](https://youtu.be/CgXQC4dbGUE?t=5089)

### ðª ã¹ãã¼ã­ã³ã°ã«å¿è¦ãªæ®ãã®æ©è½ãå®è£ããã

ååã®ã¬ãã¹ã³ã§ã¯ã`TokenFarm.sol` ã«ã¹ãã¼ã­ã³ã°æ©è½ãå®è£ãã¾ããã
ä»åã¯ãYield Farming ãå®æãããããã«ãæ®ãäºã¤ã®æ©è½ãå®è£ãã¦ããã¾ãã
1. ã³ãã¥ããã£ã®ãã¼ã¯ã³çºè¡æ©è½
2. ã¹ãã¼ã­ã³ã°ãããã¼ã¯ã³ãæåã«æ»ãæ©è½ï¼ã¢ã³ã¹ãã¼ã­ã³ã°æ©è½ï¼

ããã§ã¯æ©é `TokenFarm.sol` ãããã®ããã«æ´æ°ãã¦ããã¾ãããï¼

```javascript
// TokenFarm.sol
pragma solidity ^0.5.0;

import "./DappToken.sol";
import "./MockDaiToken.sol";

contract TokenFarm{
    string public name = "Dapp Token Farm";
    address public owner;
    DappToken public dappToken;
    DaiToken public daiToken;

    address[] public stakers;
    mapping (address => uint) public stakingBalance;
    mapping (address => bool) public hasStaked;
    mapping (address => bool) public isStaking;

    constructor(DappToken _dappToken, DaiToken _daiToken) public {
        dappToken = _dappToken;
        daiToken = _daiToken;
        owner = msg.sender;
    }


    //1.ã¹ãã¼ã­ã³ã°æ©è½
    function stakeTokens(uint _amount) public {
        require(_amount > 0, "amount can't be 0");
        daiToken.transferFrom(msg.sender, address(this), _amount);

        stakingBalance[msg.sender] = stakingBalance[msg.sender] + _amount;

        if(!hasStaked[msg.sender]){
            stakers.push(msg.sender);
        }

        isStaking[msg.sender] = true;
        hasStaked[msg.sender] = true;
    }

    // ----- è¿½å ããæ©è½ ------ //
    //2.ãã¼ã¯ã³ã®çºè¡æ©è½
    function issueTokens() public {
        // Dapp ãã¼ã¯ã³ãçºè¡ã§ããã®ã¯ããªãã®ã¿ã§ãããã¨ãç¢ºèªãã
        require(msg.sender == owner, "caller must be the owner");

        // æè³å®¶ãé ããå½Daiãã¼ã¯ã³ã®æ°ãç¢ºèªããåéã®Dappãã¼ã¯ã³ãçºè¡ãã
        for(uint i=0; i<stakers.length; i++){
            // recipient ã¯ Dapp ãã¼ã¯ã³ãåãåãæè³å®¶
            address recipient = stakers[i];
            uint balance = stakingBalance[recipient];
            if(balance > 0){
                dappToken.transfer(recipient, balance);
            }
        }
    }

    //ã3.ã¢ã³ã¹ãã¼ã­ã³ã°æ©è½
    // * æè³å®¶ã¯ãé ãå¥ãã Dai ãå¼ãåºããã¨ãã§ãã
    function unstakeTokens() public {
        // æè³å®¶ãã¹ãã¼ã­ã³ã°ããéé¡ãåå¾ãã
        uint balance = stakingBalance[msg.sender];
        // æè³å®¶ãã¹ãã¼ã­ã³ã°ããéé¡ã0ä»¥ä¸ã§ãããã¨ãç¢ºèªãã
        require(balance > 0, "staking balance cannot be 0");
        // å½ã® Dai ãã¼ã¯ã³ãæè³å®¶ã«è¿éãã
        daiToken.transfer(msg.sender, balance);
        // æè³å®¶ã®ã¹ãã¼ã­ã³ã°æ®é«ã0ã«æ´æ°ãã
        stakingBalance[msg.sender] = 0;
        // æè³å®¶ã®ã¹ãã¼ã­ã³ã°ç¶æãæ´æ°ãã
        isStaking[msg.sender] = false;
    }
}
```

ä»¥ä¸ã§ã`TokenFarm.sol` ã« Yield Farming ãå®è£ããä¸ã§å¿è¦ãªæ©è½ãå¨ã¦åããã¾ããï¼
### ð ãã¹ãã³ã¼ããæ´æ°ããã

ããã§ã¯ã`TokenFarm_test.js` ãæ´æ°ãã¦ããã¹ããè¡ã£ã¦ããã¾ãããã

- `è¿½å ãããã¹ãã³ã¼ã` ãæ°ããè¿½å ããããã¹ãã³ã¼ãã§ãã

```javascript
// TokenFarm_test.js
const DappToken = artifacts.require(`DappToken`)
const DaiToken = artifacts.require(`DaiToken`)
const TokenFarm = artifacts.require(`TokenFarm`)

require(`chai`).use(require('chai-as-promised')).should()

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
    // ãã¹ã1
    describe('Mock DAI deployment', async () => {
        it('has a name', async () => {
            const name = await daiToken.name()
            assert.equal(name, 'Mock DAI Token')
        })
    })
    // ãã¹ã2
    describe('Dapp Token deployment', async () => {
        it('has a name', async () => {
            const name = await dappToken.name()
            assert.equal(name, 'DApp Token')
        })
    })

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

    describe('Farming tokens', async () => {
        it('rewords investors for staking mDai tokens', async () => {
            let result

            // ãã¹ã5. ã¹ãã¼ã­ã³ã°ã®åã«æè³å®¶ã®æ®é«ãç¢ºèªãã
            result = await daiToken.balanceOf(investor)
            assert.equal(result.toString(), tokens('100'), 'investor Mock DAI wallet balance correct before staking')

            // ãã¹ã6. å½ã®DAIãã¼ã¯ã³ãç¢ºèªãã
            await daiToken.approve(tokenFarm.address, tokens('100'), {from: investor})
            await tokenFarm.stakeTokens(tokens('100'), {from: investor})

            // ãã¹ã7. ã¹ãã¼ã­ã³ã°å¾ã®æè³å®¶ã®æ®é«ãç¢ºèªãã
            result = await daiToken.balanceOf(investor)
            assert.equal(result.toString(), tokens('0'), 'investor Mock DAI wallet balance correct after staking')

            // ãã¹ã8. ã¹ãã¼ã­ã³ã°å¾ã®TokenFarmã®æ®é«ãç¢ºèªãã
            result = await daiToken.balanceOf(tokenFarm.address)
            assert.equal(result.toString(), tokens('100'), 'Token Farm Mock DAI balance correct after staking')

            // ãã¹ã9. æè³å®¶ãTokenFarmã«ã¹ãã¼ã­ã³ã°ããæ®é«ãç¢ºèªãã
            result = await tokenFarm.stakingBalance(investor)
            assert.equal(result.toString(), tokens('100'), 'investor staking balance correct after staking')

            // ãã¹ã10. ã¹ãã¼ã­ã³ã°ãè¡ã£ãæè³å®¶ã®ç¶æãç¢ºèªãã
            result = await tokenFarm.isStaking(investor)
            assert.equal(result.toString(), 'true', 'investor staking status correct after staking')

            // ----- è¿½å ãããã¹ãã³ã¼ã ------ //

            // ãã¼ã¯ã³ãçºè¡ãã
            await tokenFarm.issueTokens({from: owner})

            // ãã¼ã¯ã³ãçºè¡ããå¾ã®æè³å®¶ã® Dapp æ®é«ãç¢ºèªãã
            result = await dappToken.balanceOf(investor)
            assert.equal(result.toString(), tokens('100'), 'investor DApp Token wallet balance correct after staking')

            // ããªãï¼ownerï¼ã®ã¿ããã¼ã¯ã³ãçºè¡ã§ãããã¨ãç¢ºèªããï¼ããããªãä»¥å¤ã®äººããã¼ã¯ã³ãçºè¡ãããã¨ããå ´åãå´ä¸ãããï¼
            await tokenFarm.issueTokens({from: investor}).should.be.rejected

            //ãã¼ã¯ã³ãã¢ã³ã¹ãã¼ã­ã³ã°ãã
            await tokenFarm.unstakeTokens({from: investor})

            //ãã¹ã11. ã¢ã³ã¹ãã¼ã­ã³ã°ã®çµæãç¢ºèªãã
            result = await daiToken.balanceOf(investor)
            assert.equal(result.toString(), tokens('100'), 'investor Mock DAI wallet balance correct after staking')

            //ãã¹ã12.æè³å®¶ãã¢ã³ã¹ãã¼ã­ã³ã°ããå¾ã® Token Farm åã«å­å¨ããå½ã® Dai æ®é«ãç¢ºèªãã
            result = await daiToken.balanceOf(tokenFarm.address)
            assert.equal(result.toString(), tokens('0'), 'Token Farm Mock DAI balance correct after staking')

            //ãã¹ã13. æè³å®¶ãã¢ã³ã¹ãã¼ã­ã³ã°ããå¾ã®æè³å®¶ã®æ®é«ãç¢ºèªãã
            result = await tokenFarm.stakingBalance(investor)
            assert.equal(result.toString(), tokens('0'), 'investor staking status correct after staking')

            //ãã¹ã14. æè³å®¶ãã¢ã³ã¹ãã¼ã­ã³ã°ããå¾ã®æè³å®¶ã®ç¶æãç¢ºèªãã
            result = await tokenFarm.isStaking(investor)
            assert.equal(result.toString(), 'false', 'investor staking status correct after staking')

        })
    })
})
```

ã§ã¯ã¿ã¼ããã«ãéãã¦ `yield-farm-starter-project` ãã£ã¬ã¯ããªã«ãããã¨ãç¢ºèªãã¦ããä¸è¨ã®ã³ã¼ããå®è¡ãã¦ã¿ã¦ãã ããã

```bash
truffle test
```

ä¸ã®ãããªçµæãã¿ã¼ããã«ã«åºåããã¦ããã°ãæåã§ãã

```bash
Contract: TokenFarm
    Mock DAI deployment
      â has a name
    Dapp Token deployment
      â has a name
    Token Farm deployment
      â has a name
      â contract has tokens (38ms)
    Farming tokens
      â rewords investors for staking mDai tokens (947ms)


  5 passing (2s)

```

ä¸è¨ã®ãããªçµæãã¿ã¼ããã«ã«åºåãããã°ããã¹ãã¯æåã§ãï¼

### ðª Dapp ãã¼ã¯ã³ãçºè¡ãã

ãã¦ã`TokenFarm.sol` ã®ä¸­ã« Yield Farming ãè¡ãã®ã«å¿è¦ãªæ©è½ã¯ãã¹ã¦æãã¾ãããããããæå¾ã«ãããã¨ãä¸ã¤ããã¾ãã

ããã¯ã`issue` é¢æ°ãå¼ã³åºãããã® `js` ãã¡ã¤ã«ã®ä½æã§ãã

Token Farm ã®WEBã¢ããªã« Dai ãã¼ã¯ã³ãã¹ãã¼ã­ã³ã°ããæè³å®¶ã« Dapp ãã¼ã¯ã³ãèªåçã«çºè¡ãããã®ãçæ³ã§ãããä»å Dapp ãã¼ã¯ã³ã¯å¬éããã¦ãããã®ã§ã¯ãªããããæåã§çºè¡ãã¦ããã¾ãã

ã¿ã¼ããã«ãéãã¦ã`yield-farm-starter-project` ã«ãããã¨ãç¢ºèªããä»¥ä¸ã®ã³ã¼ããå®è¡ãã¦ã`scripts` ãã£ã¬ã¯ããªã« `issue-token.js` ãä½æãã¾ãããã

```bash
mkdir scripts
```
```bash
touch scripts/issue-token.js
```

ã§ã¯æ©éä»¥ä¸ãã`issue-token.js` ã«æ¸ãè¾¼ãã§ããã¾ãããã

```javascript
// issue-token.js
const TokenFarm = artifacts.require(`TokenFarm`)

module.exports = async function(callback) {
    let tokenFarm = await TokenFarm.deployed()
    await tokenFarm.issueTokens()
    console.log('Tokens issued!')
    callback()
}
```

ããã§ `issueTokens` ãæåã§å¼ã³åºããããã«ãªãã¾ãããã§ã¯ã¿ã¼ããã«ãéãã¦ `yield-farm-starter-project` ã«ãããã¨ãç¢ºèªãã¦ããä¸è¨ã®ã³ã¼ããé çªã«å®è¡ãã¦ã¿ã¾ãããã

```bash
truffle migrate --reset
```
```bash
truffle exec scripts/issue-token.js
```

ä»¥ä¸ã®ãããªåºåçµæãã¿ã¼ããã«ã«åæ ããã¦ããã°ãã¼ã¯ã³ã®çºè¡ã¯æåã§ãã

```bash
issue-token.js
Using network 'development'.

Tokens issued!
```

ç¾å¨ãèª°ã Token Farm ã«ã¹ãã¼ã­ã³ã°ãè¡ã£ã¦ããªãã®ã§ãå®éã«ã¯ãã¼ã¯ã³ãçºè¡ãããã¨ã¯ããã¾ããããæ¬çªã§å¤±æããªãããã«å®è¡ã ãã¯ãã¦ããã¾ãã
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
ããã§ããã¯ã¨ã³ãã®ä½æã¯å®äºã§ãï¼ãç²ãæ§ã§ããð
ã¿ã¼ããã«ã®åºåçµæã `#section-2` ã«ã·ã§ã¢ãã¾ãããï¼
æ¬¡ã®ã`section-3`ãã§ã¯ãã­ã³ãã¨ã³ãã®ä½æã«åãæãã£ã¦ããã¾ããToken Farm ã®å®æã¾ã§å¾å°ãã§ãï¼é å¼µã£ã¦ããã¾ãããðª
