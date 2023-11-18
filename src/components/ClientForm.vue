<template>
  <div class="client-form">
    <h2>Форма создания клиента</h2>
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label for="lastName">
          Фамилия<span class="required-field">* </span>
          <span class="validation-text" v-if="this.v$.formData.lastName.$error">Фамилия обязательна</span>
        </label>
        <input placeholder="Фамилия" v-model.trim="formData.lastName" type="text" id="lastName" />
      </div>

      <div class="form-group">
        <label for="firstName">
          Имя<span class="required-field">* </span>
          <span class="validation-text" v-if="this.v$.formData.firstName.$error">Имя обязательно</span>
        </label>
        <input placeholder="Имя" v-model.trim="formData.firstName" type="text" id="firstName" />
      </div>

      <div class="form-group">
        <label for="surname">Отчество</label>
        <input placeholder="Отчество" v-model.trim="formData.surname" type="text" id="surname" />
      </div>

      <div class="grid">
        <div class="form-group">
          <label for="dob">Дата рождения<span class="required-field">*</span>
            <span class="validation-text" v-if="this.v$.formData.dob.$error">Дата рождения обязательна</span>
          </label>
          <input v-model="formData.dob" type="date" id="dob" class="input-form" />
        </div>

        <div class="form-group">
          <label for="phoneNumber">Номер телефона<span class="required-field">*</span>
            <span class="validation-text" v-if="this.v$.formData.phoneNumber?.startsWithSeven.$invalid">
          Номер телефона должен начинаться с 7
        </span>
          </label>
          <input
              class="input-form"
              placeholder="+7********"
              v-model.trim="formData.phoneNumber"
              type="tel"
              id="phoneNumber"
              @input="validatePhoneNumber"
          />
        </div>
      </div>

      <div class="grid">
        <div class="form-group">
          <label>Пол<span class="required-field">* </span>
            <span class="validation-text" v-if="this.v$.formData.firstName.$error">Выберите пол</span></label>
          <div class="gender-block">
            <div class="gender-content">
              <label class="gender-text" for="male">Мужской</label>
              <input id="male" v-model="formData.gender" type="radio" value="male" />
            </div>
            <div class="gender-content">
              <label class="gender-text" for="female">Женский</label>
              <input id="female" v-model="formData.gender" type="radio" value="female" />
            </div>
          </div>
        </div>

        <div class="form-group">
          <label>Группа клиентов<span class="required-field">* </span>
            <span class="validation-text" v-if="this.v$.formData.selectedGroups?.$error">Выберите хотя бы одну группу</span>
          </label>
          <select multiple v-model="formData.selectedGroups">
            <option value="VIP">VIP</option>
            <option value="Проблемные">Проблемные</option>
            <option value="ОМС">ОМС</option>
          </select>
        </div>
      </div>

      <div class="form-group">
        <div class="doNotSendSMS">
          <label for="doNotSendSMS" class="doNotSendSMS-text">Не отправлять СМС</label>
          <input id="doNotSendSMS" class="doNotSendSMS-input" v-model="formData.doNotSendSMS" type="checkbox" />
        </div>
      </div>
      <hr>
      <h3>Адрес</h3>
      <div class="grid">
        <div class="form-group">
          <label for="index">Индекс</label>
          <input v-model.trim="formData.address.index" type="text" id="index" />
        </div>

        <div class="form-group">
          <label for="country">Страна</label>
          <input v-model.trim="formData.address.country" type="text" id="country" />
        </div>
      </div>

      <div class="grid">
        <div class="form-group">
          <label for="region">Область</label>
          <input v-model.trim="formData.address.region" type="text" id="region" />
        </div>

        <div class="form-group">
          <label for="city">Город<span class="required-field">* </span>
            <span class="validation-text" v-if="this.v$.formData.address.city.$error">Город обязателен</span>
          </label>
          <input v-model.trim="formData.address.city" type="text" id="city" />
        </div>
      </div>

      <div class="grid">
        <div class="form-group">
          <label for="street">Улица</label>
          <input v-model.trim="formData.address.street" type="text" id="street" />
        </div>

        <div class="form-group">
          <label for="house">Дом</label>
          <input v-model.trim="formData.address.house" type="text" id="house" />
        </div>
      </div>
      <hr>
      <h3>Паспорт</h3>
      <div class="form-group">
        <label for="documentType">Тип документа<span class="required-field">* </span>
          <span class="validation-text" v-if="this.v$.formData.passport.documentType.$error">Выберите тип документа</span>
        </label>
        <select v-model="formData.passport.documentType">
          <option value="Паспорт">Паспорт</option>
          <option value="Свидетельство о рождении">Свидетельство о рождении</option>
          <option value="Вод. удостоверение">Вод. удостоверение</option>
        </select>
      </div>

      <div class="grid">
        <div class="form-group">
          <label for="series">Серия</label>
          <input v-model.trim="formData.passport.series" type="text" id="series" />
        </div>

        <div class="form-group">
          <label for="number">Номер</label>
          <input v-model.trim="formData.passport.number" type="text" id="number" />
        </div>
      </div>

      <div class="grid">
        <div class="form-group">
          <label for="issuedBy">Кем выдан</label>
          <input v-model.trim="formData.passport.issuedBy" type="text" id="issuedBy" />
        </div>

        <div class="form-group">
          <label for="issueDate">Дата выдачи<span class="required-field">* </span>
            <span class="validation-text" v-if="this.v$.formData.passport.issueDate.$error">Дата выдачи обязательна</span>
          </label>
          <input v-model="formData.passport.issueDate" type="date" id="issueDate" />
        </div>
      </div>


      <button type="submit">Создать клиента</button>
      <div v-if="formSubmitted">Новый клиент успешно создан!</div>
      <div v-if="formSubmittedNone">Заполните обязательные поля!</div>
    </form>
  </div>
</template>

<script>
import { required, minLength, numeric, helpers } from '@vuelidate/validators'
import { useVuelidate } from '@vuelidate/core'
const { regex } = helpers;
export default {

  data: () => {
    return {
      formData: {
        lastName: '',
        firstName: '',
        surname: '',
        dob: '',
        phoneNumber: 7,
        gender: '',
        selectedGroups: [],
        doNotSendSMS: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          documentType: "",
          series: "",
          number: "",
          issuedBy: "",
          issueDate: "",
        },
      },
      formSubmitted: false,
      formSubmittedNone: false,
    };
  },
  setup: () => ({ v$: useVuelidate() }),
  validations () {
    return {
      formData: {
        lastName: { required },
        firstName: { required },
        surname: {},
        dob: { required },
        phoneNumber: { required, minLength: minLength(11), numeric, startsWithSeven: regex(/^7/) },
        gender: { required },
        selectedGroups: { required },
        address: {
          index: {},
          country: {},
          region: {},
          city: { required },
          street: {},
          house: {},
        },
        passport: {
          documentType: { required },
          series: {},
          number: {},
          issuedBy: {},
          issueDate: { required },
        },
      },
    }
  },
  methods: {
    validatePhoneNumber() {
      this.v$.formData.phoneNumber.$touch();
    },
    submitForm() {
      this.v$.formData.$touch();

      if (this.v$.formData.$pending || this.v$.formData.$invalid || this.v$.formData.phoneNumber?.startsWithSeven.$invalid) {
        this.formSubmittedNone = true;
        this.formSubmitted = false;
      } else {
        this.formSubmittedNone = false;
        this.formSubmitted = true;
      }
    },
  },
}
</script>

<style scoped>
.doNotSendSMS {
  display: flex;
  justify-content: center;
  .doNotSendSMS-text{
    margin: 0 15px 0 0;
  }
  .doNotSendSMS-input {
    width: 17px;
    height: 17px;
    display: block;

    margin: 0;
  }
}
.grid {
  width: 100%;
  display: flex;
  justify-content: space-between;
  font-family: Andale Mono, monospace;
  .form-group {
    width: 48%;
    .input-form {
      height: 20px;
      border: 1px solid #ccc
    }
    .input-form::placeholder {
      font-family: Andale Mono, monospace;
      font-size: 12px;
    }
  }
}
.gender-block {
  display: flex;
  justify-content: flex-start;
  .gender-content {
    display: flex;
    justify-content: center;
    flex-direction: row;
    margin-right: 10px;
    .gender-text {
      margin: 0 5px 0 0;
    }
    #female, #male {
      margin: 0;
    }
  }
}
.client-form {
  max-width: 800px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 15px;
  .validation-text{
    color: red;
  }
}

label {
  display: block;
  margin-bottom: 5px;
  font-size: 14px;
  font-weight: bold;
  font-family: Andale Mono, monospace;
  text-align: left;
}

input,
select,
button {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
}

button {
  background-color: #4caf50;
  color: white;
  cursor: pointer;
}

.required-field {
  color: red;
}
</style>
