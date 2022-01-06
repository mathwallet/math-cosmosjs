<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <h2>Cosmos DApp</h2>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <v-card>
          <v-card-title>
            发送
          </v-card-title>
          <v-card-subtitle>
            {{ account ? account.address : ""}}
          </v-card-subtitle>
          <v-card-actions>
            <v-btn @click="loginAction()" v-if="!account">登录</v-btn>
            <v-btn @click="sendAction()" v-else>发送</v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { SigningStargateClient } from "@cosmjs/stargate";

export default {
  name: 'App',
  data: () => ({
    account: null
  }),
  methods: {
    async getAccount() {
        if (!window.offlineSigner) {
          throw new Error("Please install MathWallet first!")
        }
        const [firstAccount] = await window.offlineSigner.getAccounts();
        return firstAccount;
    },
    async send() {
      if (!this.account) {
        throw new Error("Account Not Found!")
      }
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
        const result = await client.sendTokens(this.account.address, recipient, [amount], fee, "memo");
        return result;
    },
    loginAction() {
      this.getAccount().then(account => {
        console.log(account);
        this.account = account;
      }).catch(e => {
        alert(e.message || "Unknown error");
      });
    },
    sendAction() {
      this.send().then(resp => {
        console.log(resp);
      }).catch(e => {
        alert(e.message || "Unknown error");
      });
    }
  },
};
</script>
