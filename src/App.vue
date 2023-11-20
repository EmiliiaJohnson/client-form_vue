<template>
  <div id="app">
    <h1 class="title">Форма создания нового клиента</h1>
    <p>*Обязательные для заполнения поля</p>
    <form @submit.prevent="handleSubmit()">
      <div class="group">
        <h3>Личные данные</h3>
        <div class="input-group">
          <label for="last-name">Фамилия*</label>
          <input
            id="last-name"
            type="text"
            v-model="lastName"
            :class="{ invalid: $v.lastName.$error }"
          />
          <p class="error-message" v-if="$v.lastName.$error">
            {{ requiredField }}
          </p>
        </div>
        <div class="input-group">
          <label for="first-name">Имя*</label>
          <input
            id="first-name"
            type="text"
            v-model="firstName"
            :class="{ invalid: $v.firstName.$error }"
          />
          <p class="error-message" v-if="$v.firstName.$error">
            {{ requiredField }}
          </p>
        </div>
        <div class="input-group">
          <label for="surname">Отчество</label>
          <input id="surname" type="text" v-model="surname" />
        </div>
        <div class="input-group">
          <label for="bday">Дата рождения*</label>
          <input
            id="bday"
            type="date"
            v-model="bday"
            :class="{ invalid: $v.bday.$error }"
          />
          <p class="error-message" v-if="submitted && !$v.bday.required">
            {{ requiredField }}
          </p>
          <p class="error-message" v-if="!$v.bday.maxValue">
            {{ mustBePast }}
          </p>
        </div>
        <div class="input-group">
          <label for="phone">Номер телефона*</label>
          <input
            placeholder="7___-___-__-__"
            id="phone"
            type="tel"
            v-model="phone"
            maxlength="11"
            minlength="11"
            :class="{ invalid: $v.phone.$error }"
          />
          <p class="error-message" v-if="submitted && !$v.phone.required">
            {{ requiredField }}
          </p>
          <p class="error-message" v-if="!$v.phone.number">
            Номер должен начинаться с 7
          </p>
        </div>
        <div class="input-group">
          <label for="doctors">Лечащий врач</label>
          <select id="doctors" v-model="doctor">
            <option
              v-for="(doctor, index) in doctors"
              :value="doctor.value"
              :key="index"
            >
              {{ doctor.label }}
            </option>
          </select>
        </div>
        <div class="input-group">
          <span>Группа клиентов*</span>
          <div class="section-wrap" :class="{ invalid: $v.group.$error }">
            <label class="checkbox-label"
              ><input type="checkbox" value="oms" v-model="group" />ОМС</label
            >
            <label class="checkbox-label"
              ><input
                type="checkbox"
                value="problematic"
                v-model="group"
              />Проблемные</label
            >
            <label class="checkbox-label"
              ><input type="checkbox" value="vip" v-model="group" />VIP</label
            >
          </div>
          <p class="error-message" v-if="$v.group.$error">
            {{ requiredField }}
          </p>
        </div>
        <div class="input-group">
          <span>Пол</span>
          <div class="section-wrap">
            <label class="checkbox-label"
              ><input type="radio" value="male" v-model="sex" />Мужской</label
            >
            <label class="checkbox-label"
              ><input type="radio" value="female" v-model="sex" />Женский</label
            >
          </div>
        </div>
        <div class="input-group">
          <label class="checkbox-label"
            ><input type="checkbox" v-model="noSms" />Не отправлять СМС</label
          >
        </div>
      </div>
      <div class="group">
        <h3>Паспортные данные</h3>
        <div class="input-group">
          <label for="index">Индекс</label>
          <input
            id="index"
            type="number"
            maxlength="6"
            minlength="6"
            v-model="index"
          />
        </div>
        <div class="input-group">
          <label for="country">Страна</label>
          <input id="country" type="text" v-model="country" />
        </div>
        <div class="input-group">
          <label for="region">Область</label>
          <input id="region" type="text" v-model="region" />
        </div>
        <div class="input-group">
          <label for="city">Город*</label>
          <input
            id="city"
            type="text"
            v-model="city"
            :class="{ invalid: $v.city.$error }"
          />
          <p class="error-message" v-if="submitted && !$v.phone.required">
            {{ requiredField }}
          </p>
        </div>
        <div class="input-group">
          <label for="street">Улица</label>
          <input id="street" type="text" v-model="street" />
        </div>
        <div class="input-group">
          <label for="building">Дом</label>
          <input id="building" type="text" v-model="building" />
        </div>

        <div class="input-group">
          <label for="documents">Тип документа*</label>
          <select id="documents" v-model="documentType">
            <option
              v-for="(document, index) in documents"
              :value="document.value"
              :key="index"
            >
              {{ document.label }}
            </option>
          </select>
        </div>
        <div class="input-group">
          <label for="serie">Серия</label>
          <input id="serie" type="text" v-model="serie" />
        </div>
        <div class="input-group">
          <label for="number">Номер</label>
          <input id="number" type="number" v-model="number" />
        </div>
        <div class="input-group">
          <label for="issued">Кем выдан</label>
          <input id="issued" type="text" v-model="issued" />
        </div>
        <div class="input-group">
          <label for="date">Дата выдачи*</label>
          <input
            id="date"
            type="date"
            v-model="date"
            v-bind:class="{ invalid: $v.date.$error }"
          />
          <p class="error-message" v-if="submitted && !$v.date.required">
            {{ requiredField }}
          </p>
          <p class="error-message" v-if="!$v.date.maxValue">
            {{ mustBePast }}
          </p>
        </div>
        <button type="submit">Сохранить</button>
      </div>
    </form>
  </div>
</template>

<script>
import {
  required,
  maxLength,
  minLength,
  helpers,
} from "vuelidate/lib/validators";
import "@/styles.scss";
const number = helpers.regex("serial", /^7\d{10}$/);
export default {
  name: "App",
  data() {
    return {
      submitted: false,
      lastName: "",
      firstName: "",
      surname: "",
      bday: "",
      phone: "",
      sex: "male",
      group: [],
      doctor: "",
      doctors: [
        { label: "Иванов", value: "Иванов" },
        { label: "Захаров", value: "Захаров" },
        { label: "Чернышева", value: "Чернышева" },
      ],
      noSms: false,
      index: "",
      country: "",
      region: "",
      city: "",
      street: "",
      building: "",
      documentType: "Паспорт",
      documents: [
        { label: "Паспорт", value: "Паспорт" },
        {
          label: "Свидетельство о рождении",
          value: "Свидетельство о рождении",
        },
        { label: "Вод. удостоверение", value: "Вод. удостоверение" },
      ],
      serie: "",
      number: "",
      issued: "",
      date: "",
      requiredField: "Обязательное поле",
      mustBePast: "Дата должна быть в прошлом",
    };
  },

  validations() {
    return {
      lastName: { required },
      firstName: { required },
      bday: {
        required,
        maxValue: (value) => value < new Date().toISOString(),
      },
      phone: {
        required,
        number,
      },
      group: { required },
      city: { required },
      date: {
        required,
        maxValue: (value) => value < new Date().toISOString(),
      },
      index: {
        maxLength: maxLength(6),
        minlength: minLength(6),
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitted = true;
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      } else {
        alert("Новый клиент успешно создан");
      }
    },
  },
};
</script>

<style></style>
