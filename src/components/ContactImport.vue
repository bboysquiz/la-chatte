<template>
  <div class="contact-import">
    <h3>Импорт контактов</h3>
    <input type="file" accept=".csv" @change="importContacts" ref="fileInput" class="file-input"/>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import { v4 as uuidv4 } from 'uuid';

export default defineComponent({
  methods: {
    importContacts(event) {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = (e) => {
        const contents = e.target.result;
        const lines = contents.split('\n');
        const importedContacts = [];

        for (let line of lines) {
          const [firstName, lastName, phone, email] = line.split(',');
          if (firstName && lastName && phone && email) {
            importedContacts.push({
              id: uuidv4(),
              firstName,
              lastName,
              phone,
              email,
              editStatus: false,
            });
          }
        }

        if (importedContacts.length > 0) {
          this.$emit('importContacts', importedContacts);
          alert('Контакты успешно импортированы!');
        } else {
          alert('Некорректный формат CSV-файла.');
        }
      };

      reader.readAsText(file);
      this.$refs.fileInput.value = '';
    },
  },
});
</script>

<style scoped>
.contact-import {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  width: 100%;
  flex-wrap: wrap;
}
h3{
  width: 100%;
  text-align: center;
}

.title {
  font-size: 20px;
  margin-bottom: 10px;
}

.file-input {
  padding: 5px 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}
</style>

