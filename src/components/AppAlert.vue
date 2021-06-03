<template>
  <div class="alert" :class="{'alert__succsess': unuqueMessage.length === 0}">
    <div v-if="unuqueMessage.length > 0">
      <p>Необходмо указать следующие данные:</p>
      <div id="messages">
        <span
        v-for="item in unuqueMessage"
        :key="item">
          {{item}}
        </span>
      </div>
    </div>
    <div v-else id="succsess">
      <p>Создан новый клиент:</p>
      <div class="wrap">

        <div class="icon">
          <img src="../assets/client-logo.png" alt="">
        </div>

        <div class="info">
          <div v-for="item in message.clientData" :key="item">
            <div class="title">
              {{item[0].replace(/\*/, '')}} :
            </div>
            <div>
            {{item[1]}}
            </div>
          </div>
        </div>

      </div>
    </div>
    <button @click="$emit('closeAlert', unuqueMessage.length === 0)">Закрыть</button>
  </div>
</template>

<script>
export default {
  emits: ['closeAlert'],
  props: {
    message: Object
  },
  data () {
    return {
      messageText: {
        surname: 'Фамилия',
        name: 'Имя',
        birthdate: 'Дата рождения',
        phone: 'Номер телефона',
        group: 'Группа клиентов',
        city: 'Город',
        document: 'Тип документа',
        issuedDate: 'Дата выдачи'
      }
    }
  },
  computed: {
    unuqueMessage () {
      const res = new Set()
      this.message.error.forEach(item => res.add(this.messageText[item]))
      return [...res]
    }
  }
}
</script>

<style>

</style>
