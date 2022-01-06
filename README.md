# For Math Wallet DAPP Developer

## Using Math Wallet Cosmos JS API


```
// login
if (!window.offlineSigner) {
    throw new Error("Please install MathWallet first!")
}
const [firstAccount] = await window.offlineSigner.getAccounts();

// logout
window.offlineSigner.forgetAccounts()

// sign transaction
import { SigningStargateClient } from "@cosmjs/stargate";
const rpcEndpoint = "https://rpc.cosmos.network";
const client = await SigningStargateClient.connectWithSigner(rpcEndpoint, window.offlineSigner);

const recipient = "cosmos1sgm024edkylksafun7pkx7z7z9cl28yukjhf8k";
const amount = {
    denom: "uatom",
    amount: "1000"
};

const fee = {
    amount: [{
        denom: "uatom",
        amount: "2000",
    }],
    gas: "80000"
}
await client.sendTokens(this.account.address, recipient, [amount], fee, "memo");
```

For details, please find the sample in this repo.

### Download Math Wallet 麦子钱包下载

[http://mathwallet.org](http://mathwallet.org)


