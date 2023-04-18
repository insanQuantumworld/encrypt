<template>
  <div>
    <button @click="onClick">Click</button>
  </div>
</template>

<script>
import Web3 from "web3";
import { RSAKey } from 'jsrsasign';

export default {
  data() {
    return {
      publicKey: null,
      privateKey: null,
      message: 'I love you',
      encryptedMessage: null,
      decryptedMessage: null
    };
  },
  methods: {
    async onClick() {
      window.web3 = new Web3(window.ethereum);

      const chainId = await ethereum.request({ method: "eth_chainId" });
      
      const address = await ethereum.request({ method: "eth_requestAccounts" })

      console.log('[address]', address);
      this.encryptMessage()
    },
    generateKeyPair() {
      const key = new RSAKey();
      key.generate(2048, '10001');
      
      this.publicKey = key;
      this.privateKey = key;
      this.privateKey.isPrivate = true;
    },
    encryptMessage() {
      if (!this.publicKey) {
        this.generateKeyPair();
      }
      const ciphertext = this.publicKey.encrypt(this.message);
      
      console.log('[ciphertext]', ciphertext);
      this.encryptedMessage = ciphertext;
      this.decryptMessage()
    },
    decryptMessage() {
      if (!this.privateKey) {
        return;
      }
      const plaintext = this.privateKey.decrypt(this.encryptedMessage);
      console.log('[plaintext]', plaintext);
      this.decryptedMessage = plaintext;
    }
  },
};
</script>

<style scoped></style>
