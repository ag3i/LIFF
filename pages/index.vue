<template>
  <section class="container">
    <div>
      <MyButton />
    </div>
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
import MyButton from "~/components/MyButton.vue";

export default {
  components: {
    MyButton
  },

  data() {
    return {
      formData: {
        name: ''
      },
      lineId: null
    }
  },
  mounted() {  
    if (!this.canUseLIFF()) {
      return
    }

    window.liff.init(data => {
      this.lineId = data.context.userId || null
    })
  },
  methods: {
    onSubmit() {
      if (!this.canUseLIFF()) {
        return
      }

      window.liff
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
          window.liff.closeWindow()
        })
        .catch(e => {
          window.alert('Error sending message: ' + e)
        })
    },
    handleCancel() {
      if (!liff.isInClient()) {
        window.alert('This button is unavailable as LIFF is currently being opened in an external browser.');
      } else {
        liff.closeWindow();
  }
    },
    canUseLIFF() {
      return navigator.userAgent.indexOf('Line') !== -1 && window.liff
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

.MyButton {
  background-color: #00a656;
  border-radius: 1.5em;
  box-shadow: 0 0.2em 0.5em rgba(0, 0, 0, 0.2);
  padding: 1em 2em;
  color: #ffffff;
  font-weight: bold;
  text-decoration: none;
}
</style>