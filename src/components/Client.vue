<template>
  <div class="container">
    <form action="" @submit.prevent="checkForm">

      <h1 class="title">Форма создания клиента</h1>

      <!-- Персональные данные -->

      <h2 class="subtitle">Персональные данные</h2>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Фамилия <span class="star">*</span></label>
            <input 
            type="text" 
            class="form__input" 
            :class="$v.form.lastname.$error ? 'is-invalid' : ''"
            v-model.trim="form.lastname"
            placeholder="Иванов"
            >
            <small v-if="$v.form.lastname.$dirty && !$v.form.lastname.required" class="invalid-feedback">
              Обязательное поле
            </small>
          </div>
        </div>

        <div class="col">
          <div class="form-group">
            <label class="form__label">Имя <span class="star">*</span></label>
            <input 
            type="text" 
            class="form__input"
            :class="$v.form.firstname.$error ? 'is-invalid' : ''"
            v-model.trim="form.firstname"
            placeholder="Иван"
            >
            <small v-if="$v.form.firstname.$dirty && !$v.form.firstname.required" class="invalid-feedback">
              Обязательное поле
            </small>
          </div>
        </div>
      </div>

      <div class="form-group">
        <label class="form__label">Отчество</label>
        <input 
        type="text" 
        class="form__input"
        v-model.trim="form.patronymic"
        placeholder="Иванович"
        >
      </div>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Дата рождения <span class="star">*</span></label>
            <input 
            type="date" 
            class="form__input"
            :class="$v.form.date.$error ? 'is-invalid' : ''"
            v-model="form.date">
            <small v-if="$v.form.date.$dirty && !$v.form.date.required" class="invalid-feedback">
              Обязательное поле
            </small>
          </div>
        </div>

        <div class="col">
          <label class="form__label">Пол</label>
          <div class="form-group">
            <div class="form-check">
              <input class="form__input_check" type="radio" name="gender" value="male" v-model="form.gender">
              <label class="form__label form__label_check" for="male">Мужчина</label>
            </div>
            <div class="form-check">
              <input class="form__input_check" type="radio" name="gender" value="female" v-model="form.gender">
              <label class="form__label form__label_check" for="female">Женщина</label>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Номер телефона <span class="star">*</span></label>
            <input 
            type="tel" 
            class="form__input"
            :class="$v.form.phone.$error ? 'is-invalid' : ''"
            v-model="form.phone"
            placeholder="79999999999"
            >
            <small v-if="$v.form.phone.$dirty && !$v.form.phone.required" class="invalid-feedback">
              Обязательное поле
            </small>
            <small v-if="$v.form.phone.$dirty && !$v.form.phone.numeric" class="invalid-feedback">
              Допустимы только числа
            </small>
            <small v-if="$v.form.phone.$dirty && !$v.form.phone.minLength" class="invalid-feedback">
              Номер должен содержать 11 цифр
            </small>
          </div>
        </div>
        <div class="col">
        <div class="form-group">
          <div class="form-check">
            <input class="form__input_check" type="checkbox" v-model="form.agreeWithSendSMS">
            <label class="form__label form__label_check">Не отправлять СМС</label>
          </div>
        </div>
        </div>
      </div>


      <div class="form-group">
        <label class="form__label">Группа клиентов <span class="star">*</span></label>
        <select 
        class="form__input" 
        v-model="form.client" 
        :class="$v.form.client.$error ? 'is-invalid' : ''"
        multiple
        >
          <option
          v-for="(client, index) in clientsGroup" 
          :value="client.value"
          :key="index"
          >
            {{ client.label }}
          </option>
        </select>
        <small v-if="$v.form.client.$dirty && !$v.form.client.required" class="invalid-feedback">
          Обязательное поле
        </small>       
      </div>

      <div class="form-group">
        <label class="form__label">Лечащий врач</label>
        <select class="form__input" v-model="form.doctor">
          <option
          v-for="(doctor, index) in doctors" 
          :value="doctor.value"
          :key="index"
          >
            {{ doctor.label }}
          </option>
        </select>
      </div>

      <!-- Адрес -->

      <h2 class="subtitle">Адрес</h2>
      
      <div class="form-group">
        <label class="form__label">Страна</label>
        <input 
        type="text" 
        class="form__input"
        v-model="form.country">
      </div>

      <div class="form-group">
        <label class="form__label">Область</label>
        <input 
        type="text" 
        class="form__input"
        v-model="form.region"
        >
      </div>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Город <span class="star">*</span></label>
            <input 
            type="text" 
            class="form__input"
            :class="$v.form.city.$error ? 'is-invalid' : ''"
            v-model="form.city"
            >
            <small v-if="$v.form.city.$dirty && !$v.form.city.required" class="invalid-feedback">
              Обязательное поле
            </small>
          </div>
        </div>
        <div class="col">
          <div class="form-group">
            <label class="form__label">Индекс</label>
            <input 
            type="text" 
            class="form__input"
            :class="$v.form.postcode.$error ? 'is-invalid' : ''"
            v-model="form.postcode"
            >
            <small v-if="$v.form.postcode.$dirty && !$v.form.postcode.numeric" class="invalid-feedback">
              Допустимы только числа
            </small>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Улица</label>
            <input 
            type="text" 
            class="form__input"
            v-model="form.street"
            >
          </div>
        </div>
        <div class="col">
          <div class="form-group">
            <label class="form__label">Дом</label>
            <input 
            type="text" 
            class="form__input"
            v-model="form.building"
            >
          </div>
        </div>
      </div>

      <!-- Паспорт -->

      <h2 class="subtitle">Паспортные данные</h2>

      <div class="form-group">
        <label class="form__label">Тип документа <span class="star">*</span></label>
        <select 
        class="form__input" 
        v-model="form.doctype" 
        :class="$v.form.doctype.$error ? 'is-invalid' : ''"
        >
          <option
          v-for="(doctype, index) in doctypes" 
          :value="doctype.value"
          :key="index"
          >
            {{ doctype.label }}
          </option>
        </select>
        <small v-if="$v.form.doctype.$dirty && !$v.form.doctype.required" class="invalid-feedback">
          Обязательное поле
        </small>
      </div>

      <div class="row">
        <div class="col">
          <div class="form-group">
            <label class="form__label">Серия</label>
            <input
            type="text" 
            class="form__input"
            :class="$v.form.passportSeries.$error ? 'is-invalid' : ''"
            v-model="form.passportSeries"
            placeholder="9999"
            >
            <small v-if="$v.form.passportSeries.$dirty && !$v.form.passportSeries.numeric" class="invalid-feedback">
              Допустимы только числа
            </small>
            <small v-if="$v.form.passportSeries.$dirty && !$v.form.passportSeries.minLength" class="invalid-feedback">
              Серия должна содержать 4 цифры
            </small>
          </div>

        </div>
        <div class="col">
          <div class="form-group">
            <label class="form__label">Номер</label>
            <input 
            type="text" 
            class="form__input"
            :class="$v.form.passportNumber.$error ? 'is-invalid' : ''"
            v-model="form.passportNumber"
            placeholder="999999"
            >
          <small v-if="$v.form.passportNumber.$dirty && !$v.form.passportNumber.numeric" class="invalid-feedback">
            Допустимы только числа
          </small>
          <small v-if="$v.form.passportNumber.$dirty && !$v.form.passportNumber.minLength" class="invalid-feedback">
            Номер должен содержать 6 цифр
          </small>
          </div>
        </div>
      </div>

      <div class="form-group">
        <label class="form__label">Кем выдан</label>
        <input type="text" class="form__input">
      </div>

      <div class="form-group">
        <label class="form__label">Дата выдачи <span class="star">*</span></label>
        <input 
        type="date" 
        class="form__input"
        :class="$v.form.issueDate.$error ? 'is-invalid' : ''"
        v-model="form.issueDate">
        <small v-if="$v.form.issueDate.$dirty && !$v.form.issueDate.required" class="invalid-feedback">
          Обязательное поле
        </small>
      </div>

      <div v-if="message" class="message">
        {{ message }}
      </div>

      <button class="btn" type="submit">Отправить</button>

    </form>
  </div>
</template>

<script>

  import { required, minLength, maxLength, numeric } from 'vuelidate/lib/validators'

  export default {
    name: 'Client',
    data() {
      return {
        message: '',
        form: {
          firstname: '',
          lastname: '',
          patronymic: '',
          agreeWithSendSMS: false,
          gender: '',
          doctor: 'Ivanov',
          client: [],
          date: '',
          phone: '',
          country: '',
          region: '',
          city: '',
          postcode: '',
          street: '',
          building: '',
          doctype: 'Passport',
          passportSeries: '',
          passportNumber: '',
          issuedBy: '',
          issueDate: ''

        },
        doctors: [
          {
            label: 'Иванов',
            value: 'Ivanov'
          },
          {
            label: 'Захаров',
            value: 'Zakharov'
          },
          {
            label: 'Чернышева',
            value: 'Chernyshova'
          }        
        ],
        clientsGroup: [
          {
            label: 'VIP',
            value: 'VIP'
          },
          {
            label: 'Проблемные',
            value: 'Problematic'
          },
          {
            label: 'ОМС',
            value: 'OMC'
          }   
        ],
        doctypes: [
          {
            label: 'Паспорт',
            value: 'Passport'
          },
          {
            label: 'Свидетельство о рождении',
            value: 'Birth certificate'
          },
          {
            label: 'Водительское удостоверение',
            value: 'Drivers license'
          }  
        ]
      }
    },
    validations: {
      form: {
        firstname: { required },
        lastname: { required },
        date: { required },
        phone: { required, numeric, minLength: minLength(11), maxLength: maxLength(11) },
        client: { required },
        postcode: {numeric },
        city: { required },
        doctype: { required },
        passportSeries: { numeric, minLength: minLength(4), maxLength: maxLength(4) },
        passportNumber: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
        issueDate: { required }
      }
    },
    methods: {
      checkForm() {
        this.$v.form.$touch();
        if (!this.$v.form.$error) {
          this.message = 'Клиент успешно создан'
        }
      }
    }
  }
</script>

<style>
  @import '../assets/styles/style.css';
</style>