### ð ã¦ã¼ã¶ã¼ã« ETH ãè´ã

ã³ã³ãã©ã¯ãã®é­åã® 1 ã¤ã¨ãã¦ãã³ã³ãã©ã¯ãã«é¢ä¸ããã¦ã¼ã¶ã¼ã«å ±é¬ãæ¯æããæ©è½ãå®è£ã§ãããã¨ãæããã¾ãã

ãã£ã¦ããã®ã¬ãã¹ã³ã§ã¯ãETH ãã¦ã¼ã¶ã¼ã«éãæ©è½ãã³ã³ãã©ã¯ãã«å®è£ããæ¹æ³ãå­¦ã³ã¾ãã

ã¾ããããªãã® Web ã¢ããªã±ã¼ã·ã§ã³ã§ãðï¼waveï¼ããéã£ã¦ãããäººã«ã0.0001ETHï¼â$0.30ï¼ããæä¾ããã¹ã¯ãªãããä½æãã¦ããã¾ãã

å¼ãç¶ããRinkeby Test Network ä¸ã«ã³ã³ãã©ã¯ããããã­ã¤ããã®ã§ãã¦ã¼ã¶ã¼ã«éãã®ã¯å½ã® ETH ã«ãªãã¾ãã

`WavePortal.sol` ã® `wave` é¢æ°ãä¸è¨ã®ããã«æ´æ°ãã¦ããã¾ãã

```solidity
// WavePortal.sol
function wave(string memory _message) public {
	totalWaves += 1;
	console.log("%s waved w/ message %s", msg.sender, _message);
	/*
	* ãðï¼waveï¼ãã¨ã¡ãã»ã¼ã¸ãéåã«æ ¼ç´ã
	*/
	waves.push(Wave(msg.sender, _message, block.timestamp));
	/*
	* ã³ã³ãã©ã¯ãå´ã§emitãããã¤ãã³ãã«é¢ããéç¥ããã­ã³ãã¨ã³ãã§åå¾ã§ããããã«ããã
	*/
	emit NewWave(msg.sender, block.timestamp, _message);
	/*
	* ãðï¼waveï¼ããéã£ã¦ãããã¦ã¼ã¶ã¼ã«0.0001ETHãéã
	*/
	uint256 prizeAmount = 0.0001 ether;
	require(
		prizeAmount <= address(this).balance,
		"Trying to withdraw more money than the contract has."
	);
	(bool success, ) = (msg.sender).call{value: prizeAmount}("");
	require(success, "Failed to withdraw money from contract.");
}
```

ã³ã¼ããè¦ã¦ããã¾ãããã

> ã¾ããä¸è¨ã§ `prizeAmount` ã¨ããå¤æ°ãå®ç¾©ãã`0.0001` ETH ãæå®ãã¦ãã¾ãã
>
> ```solidity
> // WavePortal.sol
> uint256 prizeAmount = 0.0001 ether;
> ```
>
> ããã¦ãä¸è¨ã§ã¯ãã¦ã¼ã¶ã¼ã«éã ETH ã®é¡ã**ã³ã³ãã©ã¯ããæã¤æ®é«**ããä¸åã£ã¦ãããã¨ãç¢ºèªãã¦ãã¾ãã
>
> ```solidity
> // WavePortal.sol
> require(
> 	prizeAmount <= address(this).balance,
> 	"Trying to withdraw more money than the contract has."
> );
> ```
>
> `address(this).balance` ã¯**ã³ã³ãã©ã¯ããæã¤ã®è³éã®æ®é«**ãç¤ºãã¦ãã¾ãã
>
> `require` ã¯ãä½ããã®æ¡ä»¶ã `true` ãããã¯ `false` ã§ãããã¨ãç¢ºèªãã `if` æã®ãããªå½¹å²ãæããã¾ãã
> ãã `require` ã®çµæã `false` ã®å ´åï¼ï¼ã³ã³ãã©ã¯ããæã¤è³éãè¶³ããªãå ´åï¼ã¯ããã©ã³ã¶ã¯ã·ã§ã³ãã­ã£ã³ã»ã«ãã¾ãã
>
> ã³ã³ãã©ã¯ãã«è³éãæä¾ããæ¹æ³ã«ã¤ãã¦ã¯ãæ¬¡ã®ã¬ãã¹ã³ã§èª¬æãã¾ãã
>
> ä¸è¨ã®ã³ã¼ãã¯ã¦ã¼ã¶ã¼ã«ééãè¡ãããã«å®è£ããã¦ãã¾ãã
>
> ```solidity
> // WavePortal.sol
> (bool success, ) = (msg.sender).call{valueï¼prizeAmount}("")
> ```
>
> ä¸è¨ã®ã³ã¼ãã¯ããã©ã³ã¶ã¯ã·ã§ã³ï¼ï¼ééï¼ãæåãããã¨ãç¢ºèªãã¦ãã¾ãã
>
> ```solidity
> // WavePortal.sol
> require(success, "Failed to withdraw money from contract.");
> ```
>
> æåããå ´åã¯ééãè¡ããæåããªãã£ãå ´åã¯ãã¨ã©ã¼æãåºåãã¦ãã¾ãã

æ¬¡ã«ã`WavePortal.sol` ã® `constructor` ãä¸è¨ã®ããã«å¤æ´ãã¾ãã

```solidity
// WavePortal.sol
constructor() payable {
  console.log("We have been constructed!");
}
```

`payable` ãå ãããã¨ã§ãã³ã³ãã©ã¯ãã«ééæ©è½ãå®è£ãã¾ãã

### ð¦ ã³ã³ãã©ã¯ãã«è³éãæä¾ãã

ä¸è¨ã§ãã¦ã¼ã¶ã¼ã« ETH ãééããããã®ã³ã¼ããå®è£ãã¾ããã
æ¬¡ã«ããã®è³éæºã¨ãªã ETH ãã³ã³ãã©ã¯ãã«ä»ä¸ããä½æ¥­ãè¡ãã¾ãã

ãã¹ããå®æ½ããããã«ã `run.js` ãæ´æ°ãã¾ãã

- `run.js` ã¯ã³ã³ãã©ã¯ãã®ã³ã¢æ©è½ã®ãã¹ããè¡ãããã®ã¹ã¯ãªããã§ãã

```javascript
// run.js
const main = async () => {
  const waveContractFactory = await hre.ethers.getContractFactory("WavePortal");
  /*
   * 0.1ETHãã³ã³ãã©ã¯ãã«æä¾ãã¦ããã­ã¤ãã
   */
  const waveContract = await waveContractFactory.deploy({
    value: hre.ethers.utils.parseEther("0.1"),
  });
  await waveContract.deployed();
  console.log("Contract deployed to:", waveContract.address);

  /*
   * ã³ã³ãã©ã¯ãã®æ®é«ãåå¾ããçµæãåºåï¼0.1ETHã§ãããã¨ãç¢ºèªï¼
   */
  let contractBalance = await hre.ethers.provider.getBalance(
    waveContract.address
  );
  console.log(
    "Contract balance:",
    hre.ethers.utils.formatEther(contractBalance)
  );

  /*
   * Waveãããã©ã³ã¶ã¯ã·ã§ã³ãå®äºããã¾ã§å¾æ©
   */
  let waveTxn = await waveContract.wave("A message!");
  await waveTxn.wait();

  /*
   * Waveããå¾ã®ã³ã³ãã©ã¯ãã®æ®é«ãåå¾ããçµæãåºåï¼0.0001ETHå¼ããã¦ãããã¨ãç¢ºèªï¼
   */
  contractBalance = await hre.ethers.provider.getBalance(waveContract.address);
  console.log(
    "Contract balance:",
    hre.ethers.utils.formatEther(contractBalance)
  );

  let allWaves = await waveContract.getAllWaves();
  console.log(allWaves);
};

const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.log(error);
    process.exit(1);
  }
};

runMain();
```

è©³ããè¦ã¦ããã¾ãããã

**1 \. 0.1ETH ãã³ã³ãã©ã¯ãã«æä¾ãã**

```javascript
// run.js
const waveContract = await waveContractFactory.deploy({
  value: hre.ethers.utils.parseEther("0.1"),
});
```

`hre.ethers.utils.parseEther("0.1")` ã«ãã£ã¦ãã³ã³ãã©ã¯ããããã­ã¤ãããéã«ãã³ã³ãã©ã¯ãã« 0.1 ETH ã®è³éãæä¾ãããã¨ãå®£è¨ãã¦ãã¾ãã

```javascript
// run.js
let contractBalance = await hre.ethers.provider.getBalance(
  waveContract.address
);
```

ããã§ã¯ã`ethers.js` ãæä¾ãã¦ãã `getBalance` é¢æ°ãä½¿ã£ã¦ãã³ã³ãã©ã¯ãã®ã¢ãã¬ã¹ã«ç´ã¥ãã¦ããæ®é«ã `contractBalance` ã«æ ¼ç´ãã¦ãã¾ãã

`ethers.js` ã®å¬å¼ãã­ã¥ã¡ã³ãã¯ [ãã¡ãï¼è±èªï¼](https://docs.ethers.io/v5/)ã§ãã

**2 \. ã³ã³ãã©ã¯ãã®è³éãç¢ºèªãã**

```javascript
// run.js
console.log("Contract balance:", hre.ethers.utils.formatEther(contractBalance));
```

ããã§ã¯ã`hre.ethers.utils.formatEther(contractBalance)` ãä½¿ç¨ãã¦weiåä½ã®æ®é«ãETHåä½ã«å¤æããããã§åºåããã³ã³ãã©ã¯ãã« 0.1ETH ã®æ®é«ããããç¢ºèªãã¦ãã¾ãã

**3 \. `wave` ãããã¨ã®ã³ã³ãã©ã¯ãã®æ®é«ãç¢ºèªãã**

```javascript
// run.js
/*
 * Wave
 */
let waveTxn = await waveContract.wave("A message!");
await waveTxn.wait();
/*
 * Waveããå¾ã®ã³ã³ãã©ã¯ãã®æ®é«ãåå¾ããçµæãåºåï¼0.0001ETHå¼ããã¦ãããã¨ãç¢ºèªï¼
 */
contractBalance = await hre.ethers.provider.getBalance(waveContract.address);
console.log("Contract balance:", hre.ethers.utils.formatEther(contractBalance));
```

ããã§ã¯ã`wave` ãå¼ã°ããå¾ã« `0.0001 ETH` ãã³ã³ãã©ã¯ãã®è³éããå·®ãå¼ãããããç¢ºèªãã¦ãã¾ãã

### â­ï¸ ãã¹ããå®è¡ãã

ããã§ã¯ãã¿ã¼ããã«ä¸ã§ `my-wave-portal` ã«ç§»åããä¸è¨ãå®è¡ãããã¹ããè¡ãã¾ãããã

```bash
npx hardhat run scripts/run.js
```

ä¸è¨ã®ãããªçµæãã¿ã¼ããã«ã§åºåããã¦ãããç¢ºèªãã¦ãã ããã

```bash
WavePortal - Smart Contract!
Contract deployed to: 0x5FbDB2315678afecb367f032d93F642f64180aa3
Contract balance: 0.1
0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 waved w/ message A message!
Contract balance: 0.0999
[
  [
    '0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266',
    'A message!',
    BigNumber { value: "1643871888" },
    waver: '0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266',
    message: 'A message!',
    timestamp: BigNumber { value: "1643871888" }
  ]
]
```

`Contract balance: 0.1` ãã`Contract balance: 0.0999` ã«ä¸ãã£ã¦ããã°ãæåã§ã ð

### ð« `deploy.js` ãæ´æ°ãã

æ¬çªç°å¢ã§ã³ã³ãã©ã¯ãã«è³éãæä¾ãããããä¸è¨ã®ããã« `deploy.js` ãæ´æ°ãã¾ãã

```javascript
// deploy.js
const main = async () => {
  const [deployer] = await hre.ethers.getSigners();
  const accountBalance = await deployer.getBalance();

  console.log("Deploying contracts with account: ", deployer.address);
  console.log("Account balance: ", accountBalance.toString());

  const waveContractFactory = await hre.ethers.getContractFactory("WavePortal");
  /* ã³ã³ãã©ã¯ãã«è³éãæä¾ã§ããããã«ãã */
  const waveContract = await waveContractFactory.deploy({
    value: hre.ethers.utils.parseEther("0.001"),
  });

  await waveContract.deployed();

  console.log("WavePortal address: ", waveContract.address);
};

const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.error(error);
    process.exit(1);
  }
};

runMain();
```

æ´æ°ããã³ã¼ãã¯ä¸è¨ã§ãã

```javascript
// deploy.js
const waveContract = await waveContractFactory.deploy({
  value: hre.ethers.utils.parseEther("0.001"),
});
```

`value: hre.ethers.utils.parseEther("0.001")` ã§ãã³ã³ãã©ã¯ãã«è³éæä¾ãè¡ã£ã¦ãã¾ãã

ä»åã¯ãã¹ãã§ãã®ã§ãå°é¡ã® 0.001ETH ãã³ã³ãã©ã¯ãã«ä»ä¸ãã¦ãã¾ãã

ã¾ãã`await waveContract.deployed()` ãè¿½å ãã¦ãè³éãè¿½å ããã¾ã§ããã­ã¤ãå¾æ©ããããã«è¨­å®ãã¦ãã¾ãã

â» ãã® 1 è¡ãè¿½å ããã¨ãéçºèã¯ã³ã³ãã©ã¯ããããã­ã¤ããããã¨ãç°¡åã«ç¥ããã¨ãã§ãã¾ããä»»æã§æé¤ãã¦ããã³ã¼ãã¯èµ°ãã¾ã ð

### ð© ããä¸åº¦ããã­ã¤ãã

ã³ã³ãã©ã¯ããæ´æ°ããã®ã§ãä¸è¨ãå®è¡ããå¿è¦ãããã¾ãã

1. ååº¦ã³ã³ãã©ã¯ããããã­ã¤ãã

2. ãã­ã³ãã¨ã³ãã®ã³ã³ãã©ã¯ãã¢ãã¬ã¹ãæ´æ°ããï¼æ´æ°ãããã¡ã¤ã«: `App.js`ï¼

3. ãã­ã³ãã¨ã³ãã® ABI ãã¡ã¤ã«ãæ´æ°ããï¼æ´æ°ãããã¡ã¤ã«: `dApp-starter-project/src/utils/WavePortal.json`ï¼

**ã³ã³ãã©ã¯ããæ´æ°ãããã³ããããã® 3 ã¤ã®ã¹ããããå®è¡ããå¿è¦ãããã¾ãã**

å¾©ç¿ããã­ã¦ãä¸å¯§ã«å®è¡ãã¦ããã¾ãããã

1 \. ã¿ã¼ããã«ä¸ã§ `my-wave-portal` ã«ç§»åãã¾ãã

ä¸è¨ãå®è¡ããã³ã³ãã©ã¯ããååº¦ããã­ã¤ãã¾ãããã

```
npx hardhat run scripts/deploy.js --network rinkeby
```

ã¿ã¼ããã«ã«ä¸è¨ã®ãããªåºåçµæãè¡¨ç¤ºããã¦ããã°ãããã­ã¤ã¯æåã§ãã

```
Deploying contracts with account:  0x821d451FB0D9c5de6F818d700B801a29587C3dCa
Account balance:  321305319740326556
WavePortal address:  0x550925E923Cb1734de73B3a843A21b871fe2a673
```

[Etherscan](https://rinkeby.etherscan.io/) ã«ã¢ã¯ã»ã¹ãã¦ãã³ã³ãã©ã¯ãã¢ãã¬ã¹ãè²¼ãä»ãã¦ã¿ã¾ãããã

ä¸è¨ã®ããã«ã`Balance` ã `0.001 Ether` ã¨ãªã£ã¦ãããã¨ãç¢ºèªãã¦ãã ããã

![](/public/images/1-ETH-dApp/section-3/3_2_1.png)

ããã§ããã¹ããããã«ã³ã³ãã©ã¯ããããã­ã¤ããã¾ãã ð

2 \. `App.js` ã® `contractAddress` ããã¿ã¼ããã«ã§åå¾ããæ°ããã³ã³ãã©ã¯ãã¢ãã¬ã¹ã«å¤æ´ãã¾ãã

ã¿ã¼ããã«ã«åºåãããã³ã³ãã©ã¯ãï¼`WavePortal address`ï¼ã®ã¢ãã¬ã¹(`0x..`)ãã³ãã¼ãã¾ãããã

- ã³ãã¼ããã¢ãã¬ã¹ã `App.js` ã® `const contractAddress = "ãã¡ã"` ã«è²¼ãä»ãã¾ãããã

3 \. ä»¥åã¨åãããã« `artifacts` ãã ABI ãã¡ã¤ã«ãåå¾ãã¾ããä¸è¨ã®ã¹ããããå®è¡ãã¦ãã ããã

> 1\. ã¿ã¼ããã«ä¸ã§ `my-wave-portal` ã«ãããã¨ãç¢ºèªããï¼ãããã¯ç§»åããï¼ã
>
> 2\. ã¿ã¼ããã«ä¸ã§ä¸è¨ãå®è¡ããã
>
> ```
> code artifacts/contracts/WavePortal.sol/WavePortal.json
> ```
>
> 3\. VS Code ã§ `WavePortal.json` ãã¡ã¤ã«ãéãããã®ã§ãä¸­èº«ãå¨ã¦ã³ãã¼ãã¾ããããâ» VS Code ã®ãã¡ã¤ã³ãã¼ãä½¿ã£ã¦ãç´æ¥ `WavePortal.json` ãéããã¨ãå¯è½ã§ãã
>
> 4\. **ã³ãã¼ãã `my-wave-portal/artifacts/contracts/WavePortal.sol/WavePortal.json` ã®ä¸­èº«ã§ `dApp-starter-project/src/utils/WavePortal.json` ã®ä¸­èº«ãä¸æ¸ããã¦ãã ããã**

**ç¹°ãè¿ãã¾ãããã³ã³ãã©ã¯ããæ´æ°ãããã³ã«ãããè¡ãå¿è¦ãããã¾ãã**

`wave` ãéã£ãã¦ã¼ã¶ã¼ã« 0.0001ETH ãéããã¦ãããç¢ºèªãã¦ã¿ã¾ãããã

1\. ã¿ã¼ããã«ä¸ã§ `dApp-starter-project` ã«ç§»åããã

2\. ä¸è¨ãå®è¡ããã

> ```
> npm run start
> ```

3\. ã­ã¼ã«ã«ç°å¢ã§ Web ã¢ããªã±ã¼ã·ã§ã³ãéãã`wave` ãéãã

ä¾ï¼ãã®ãããªçµæã Web ã¢ããªã±ã¼ã·ã§ã³ã«åæ ããã¦ãããã¨ç¢ºèªãã¦ãã ãããã³ã³ãã©ã¯ããæ°ããããã®ã§ãæ¢å­ã® `wave` ã¯ãªã»ããããã¦ãã¾ãã

> ![](/public/images/1-ETH-dApp/section-3/3_2_2.png)

4\. [Etherscan](https://rinkeby.etherscan.io/) ã«ã¢ã¯ã»ã¹ãã¦ãã³ã³ãã©ã¯ãã¢ãã¬ã¹ãè²¼ãä»ããã

> ä¸è¨ã®ããã«ã`Balance` ã `0.0009 Ether` ã¨ãªã£ã¦ãããã¨ãç¢ºèªãã¦ãã ããã
>
> ![](/public/images/1-ETH-dApp/section-3/3_2_3.png)
>
> WEB ã¢ããªã§ `wave` ãéã£ãã¦ã¼ã¶ã¼ã« 0.0001ETH ãéã£ãã®ã§ãæ®é«ã `0.001-0.0001=0.0009 ETH` ã«ãªã£ã¦ãã¾ãã

ã¿ã¼ããã«ãéããã¨ãã¯ãä»¥ä¸ã®ã³ãã³ããä½¿ãã¾ã âï¸

- Mac: `ctrl + c`
- Windows: `ctrl + shift + w`

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

ããã§ã¨ããããã¾ãï¼ãã»ã¯ã·ã§ã³ 3 ãçµäºãã¾ããï¼
ããªãã® Etherscan ã®ã¢ãã¬ã¹ã `#section-3` ã«æç¨¿ãã¦ããªãã®æåãã³ãã¥ããã£ã§ç¥ãã¾ããã ð
ã¦ã¼ã¶ã¼ã« ETH ãéããã³ã³ãã©ã¯ãã®å®è£ãå®äºããããæ¬¡ã®ã¬ãã¹ã³ã«é²ã¿ã¾ããã ð
