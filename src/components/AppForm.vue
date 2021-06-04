<template>
  <div class="form-bg scroll" @click.self="closeForm">
    <form class="form" @submit.prevent="checkForm">
      <div class="form-header">
        <div>
          <span>MEDODS</span>
          <span>Медицина</span>
        </div>
        <p>Заполните форму для записи!</p>
        <span class="close" @click="closeForm">&times;</span>
      </div>

      <app-alert
        v-if="showMessage"
        :message="message"
        @closeAlert="closeAlert">
      </app-alert>

      <div class="atributes">
        <div class="personalInfo">
          <div
            v-for="(key, value) in personalInfo"
            :key="value"
          >
            <input
              type="text"
              :id="value"
              :placeholder="key"
              class="input input__text"
              v-model="formData[value]"
              @input="changeProp(value, $event)"
            >
          </div>
        </div>
        <div>
          <div class="sex">
            <span for="sex">Пол</span>
            <div>
              <label class="input__radio"><input type="radio" v-model="formData.sex" name="sex" value="male"/><span></span>Муж.</label>
              <label class="input__radio"><input type="radio" v-model="formData.sex" name="sex" value="female"/><span></span>Жен.</label>
            </div>
          </div>
          <div class="group">
            <span for="group">Группа клиентов*</span>
            <div>
              <label class="input__radio"
                v-for="item in ['VIP', 'Проблемные', 'ОМС']"
                :key="item">
                <input
                  type="checkbox"
                  v-model="formData.group"
                  name="group"
                  :value="item"
                  @input="deleteClass"/>
                <span></span>
                <!-- VIP -->
                {{item}}
              </label>
            </div>
          </div>
          <div class="doctor">
            <span for="doctor">Лечащий врач</span>
            <select v-model="formData.doctor" class="input input__select" >
              <option value selected disabled >Выбрать</option>
              <option
                v-for="item in ['Иванов', 'Захарова', 'Чернышева']"
                :value="item"
                :key="item">
                {{item}}
              </option>
            </select>
          </div>
        </div>
        <div class="send">
          <label class="input__radio" for="send">
            <input
              type="checkbox"
              id="send"
              v-model="formData.send"
              true-value="Не отправлять СМС"
              false-value="Отправить СМС"
              >
            <span></span>
            Не отправлять СМС
          </label>
        </div>
      </div>

      <div class="atributes">
        <div class="adress">
          <div
            v-for="(key, value) in adress"
            :key="value"
          >
            <input
              type="text"
              :id="value"
              :placeholder="key"
              class="input input__text"
              v-model="formData[value]"
              @input="changeProp(value, $event)"
            >
          </div>
        </div>
        <div class="passport">
            <div class="document-type">
              <span>Тип документа*</span>
              <div>
                <div class="input__radio"
                  v-for="item in ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение']"
                  :key="item"
                >
                  <input
                    type="radio"
                    name="document"
                    :value="item"
                    v-model="formData.document"
                    @input="deleteClass"/>
                  <span></span>
                  <label>{{item}}</label>
                </div>
              </div>
            </div>
            <div
              v-for="(key, value) in document"
              :key="value"
              :id="`document-${value}`"
            >
              <input
                type="text"
                :id="value"
                :placeholder="key"
                class="input input__text"
                v-model="formData[value]"
                @input="changeProp(value, $event)"
              >
            </div>
        </div>
      </div>

      <div class="btn">
        <button>Отправить</button>
      </div>
    </form>
  </div>
</template>

<script>
import AppAlert from './AppAlert'

export default {
  components: {
    'app-alert': AppAlert
  },
  emits: ['close'],
  data () {
    return {
      personalInfo: {
        surname: 'Фамилия*',
        name: 'Имя*',
        patronymic: 'Отчетсво',
        birthdate: 'Дата рождения*',
        phone: 'Номер телефона*'
      },
      adress: {
        index: 'Индекс',
        country: 'Страна',
        state: 'Область',
        city: 'Город*',
        street: 'Улица',
        house: 'Дом'
      },
      document: {
        series: 'Серия',
        number: 'Номер',
        issued: 'Кем выдан',
        issuedDate: 'Дата выдачи*'
      },
      formData: {
        surname: '',
        name: '',
        patronymic: '',
        birthdate: '',
        phone: '',
        sex: '',
        group: [],
        doctor: '',
        send: 'Отправить СМС',
        index: '',
        country: '',
        state: '',
        city: '',
        street: '',
        house: '',
        document: '',
        series: '',
        number: '',
        issued: '',
        issuedDate: ''
      },
      mask: {
        phone: '7 (xxx) xxx-xx-xx',
        birthdate: 'xx/xx/xxxx'
      },
      showMessage: false,
      message: {
        error: [],
        clientData: []
      }
    }
  },
  methods: {
    changeProp (value, event) {
      event.target.classList.remove('not__valid__input')
      if (value === 'phone' || value === 'birthdate') {
        this.checkPhoneOrBirthdate(event, value)
      } else if (['surname', 'name', 'patronymic', 'country', 'state', 'city', 'street', 'issued'].includes(value)) {
        this.formData[value] = this.formData[value].replace(/\d/, '')
      } else if (value === 'index') {
        this.formData[value] = this.formData[value].replace(/\D/, '')
      }
    },
    closeForm () {
      this.$emit('close')
    },
    checkPhoneOrBirthdate (event, value) {
      this.formData[value] = this.formData[value].replace(/(\D|\s)/ig, '')
      const pattern = value === 'phone' ? '7 (xxx) xxx-xx-xx' : '__/__/____'

      if (!isNaN(+event.data)) {
        if (event.data && event.data !== ' ') {
          this.mask[value] = this.mask[value].replace(/x/, event.data)
        } else if (event.data !== ' ') {
          const match = this.mask[value].match(/\d\D{0,}$/g)
          const newMatch = match[0] === pattern ? pattern : match[0].replace(/\d/, 'x')
          const rep = match[0].length > 1 ? match[0] : /\d$/
          this.mask[value] = this.mask[value].replace(rep, newMatch)
        }
        this.formData[value] = this.mask[value].replace(/x/g, '_')
      }
    },
    deleteClass (event) {
      event.target.parentElement.parentElement.children.forEach(item => item.classList.remove('not__valid__check'))
    },
    checkForm (event) {
      this.message.error.length = 0
      const evt = event.target
      const requiredItems = ['surname', 'name', 'group', 'city', 'document', 'issuedDate']
      for (let i = 0; i < event.target.length - 1; i++) {
        if (requiredItems.includes(evt[i].id) && this.formData[evt[i].id].length === 0) {
          evt[i].classList.add('not__valid__input')
          this.message.error.push(evt[i].id)
        } else if (requiredItems.includes(evt[i].name) && this.formData[evt[i].name].length === 0) {
          evt[i].parentElement.classList.add('not__valid__check')
          this.message.error.push(evt[i].name)
        } else if (evt[i].id === 'phone') {
          const res = this.formData.phone.replace(/_|-|\(|\)|\s/g, '').length < 11
          if (res) {
            evt[i].classList.add('not__valid__input')
            this.message.error.push('phone')
          }
        } else if (evt[i].id === 'birthdate') {
          const arr = this.formData.birthdate.split('/')
          if (+arr[0] > 31 || +arr[1] > 12 || +arr[2] > new Date().getFullYear() || arr.length < 3) {
            evt[i].classList.add('not__valid__input')
            this.message.error.push('birthdate')
          }
        }
      }
      this.showClientData()
      this.showMessage = true
    },
    closeAlert (flag) {
      this.showMessage = false
      if (flag) {
        this.closeForm()
      }
    },
    showClientData () {
      console.group('Данные клиента')
      for (const key in this.formData) {
        let title = this.personalInfo?.[key] || this.adress?.[key] || this.document?.[key]
        if (!title) {
          switch (key) {
            case 'sex':
              title = 'Пол'
              break
            case 'group':
              title = 'Группа клиентов'
              break
            case 'doctor':
              title = 'Лечащий врач'
              break
            case 'send':
              title = 'CMC'
              break
            case 'document':
              title = 'Тип документа'
              break
          }
        }
        if (this.formData[key].length !== 0) {
          console.log(`${title} : ${this.formData[key]}`)
          this.message.clientData.push([title, this.formData[key]])
          if (this.message.error.length !== 0) this.message.clientData = []
        }
      }
      console.groupEnd()
    }
  }

}
</script>

<style>

</style>
