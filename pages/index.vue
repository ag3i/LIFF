// pages/index.vue

<template>
  <section class="container">
    <p class="line-id">LINE ID：{{ lineId }}</p>
    <div class="form">
      <div class="control">
        <input class="input" type="text" placeholder="お名前" v-model="formData.name">
      </div>
      <button class="button is-info is-fullwidth" @click="onSubmit()">送信する</button>
      <button class="button is-light is-fullwidth" @click="handleCancel()">キャンセル</button>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: ''
      },
      lineId: null
    }
  },
  mounted() {
    // 最初に必ず実行する
    liff
      .init({ liffId: 'myLiffId' }) // LIFF IDを貼る
      .then(() => {
        this.loggedIn = liff.isLoggedIn()
        this.getProfile()
      })
      .catch((err) => {
        // Error happens during initialization
        this.occoredError = 'error:' + err
      })
  },
  methods: {
    onSubmit() {
      if (!this.canUseLIFF()) {
        return
      }

      liff
        .sendMessages([
          {
            type: 'text',
            text: `お名前：\n${this.formData.name}`
          },
          {
            type: 'text',
            text: '送信が完了しました'
          }
        ])
        .then(() => {
          liff.closeWindow()
        })
        .catch(e => {
          alert('Error sending message: ' + e)
        })
    },
    handleCancel() {
      if (!this.canUseLIFF()) {
        return
      }
      liff.closeWindow()
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf('Line') !== -1 && liff
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  padding: 20px;
  min-height: 100vh;
}

.line-id {
  margin-bottom: 30px;
}

.form > * {
  margin-bottom: 10px;
}
</style>