<template>
  <div class="alert">
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
      <img src="../assets/logo.png" alt="">
      <p> Новый клиент успешно создан! </p>
    </div>
    <button @click="$emit('closeAlert', unuqueMessage.length === 0)">Закрыть</button>
  </div>
</template>

<script>
export default {
  emits: ['closeAlert'],
  props: {
    message: Array
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
      this.message.forEach(item => res.add(this.messageText[item]))
      return [...res]
    }
  }
}
</script>

<style>

</style>
