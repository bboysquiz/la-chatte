<template>
  <form @submit.prevent="addContact" class="contact-form">
    <h3>Добавить контакт</h3>
    <div class="form-row">
      <label for="firstName">Имя:</label>
      <input type="text" v-model="newContact.firstName" id="firstName" required>
    </div>
    <div class="form-row">
      <label for="lastName">Фамилия:</label>
      <input type="text" v-model="newContact.lastName" id="lastName" required>
    </div>
    <div class="form-row">
      <label for="phone">Телефон:</label>
      <input type="tel" v-model="newContact.phone" id="phone" required>
    </div>
    <div class="form-row">
      <label for="email">Email:</label>
      <input type="email" v-model="newContact.email" id="email" required>
    </div>
    <button type="submit">Добавить</button>
  </form>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  data() {
    return {
      newContact: {
        firstName: '',
        lastName: '',
        phone: '',
        email: '',
        editStatus: false,
      },
    };
  },
  methods: {
    addContact() {
      this.$emit('addContact', { ...this.newContact });
      this.resetForm();
    },
    resetForm() {
      this.newContact = {
        firstName: '',
        lastName: '',
        phone: '',
        email: '',
        editStatus: false,
      };
    },
  },
});
</script>

<style scoped>
.contact-form {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
h3 {
  width: 100%;
  text-align: center;
}

.form-row {
  margin-bottom: 10px;
  width: 100%;
  display: flex;
  justify-content: center;
}

label {
  display: inline-block;
  width: 80px;
}

input[type='text'],
input[type='tel'],
input[type='email'] {
  padding: 5px;
  width: 200px;
}

button {
  padding: 5px 10px;
  border: none;
  background-color: #f0f0f0;
  color: #333;
  cursor: pointer;
}

button:hover {
  background-color: #ccc;
}
</style>
