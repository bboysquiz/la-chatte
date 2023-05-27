<template>
  <div>
    <div class="contact-list">
      <h2>Контакты</h2>
      <input class="search-input" type="text" v-model="searchQuery" placeholder="Поиск по имени или фамилии">
    </div>
    <ContactList :contacts="filteredContacts" @deleteContact="deleteContact" @editContact="editContact"
      @saveEdition="saveEdition" />
    <ContactForm @addContact="addContact" />
    <ContactImport @importContacts="importContacts" />
    <ContactExport :contacts="contacts" />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import ContactList from './components/ContactList.vue';
import ContactForm from './components/ContactForm.vue';
import ContactImport from './components/ContactImport.vue';
import ContactExport from './components/ContactExport.vue';
import { v4 as uuidv4 } from 'uuid';


export default defineComponent({
  components: {
    ContactList,
    ContactForm,
    ContactImport,
    ContactExport,
  },
  data() {
    return {
      contacts: [],
      searchQuery: '',
    };
  },
  created() {
    const savedContacts = localStorage.getItem('contacts');
    if (savedContacts) {
      this.contacts = JSON.parse(savedContacts);
    }
  },
  computed: {
    filteredContacts() {
      return this.contacts.filter(contact => {
        const fullName = contact.firstName + ' ' + contact.lastName;
        return fullName.toLowerCase().includes(this.searchQuery.toLowerCase());
      });
    },
  },
  methods: {
    addContact(contact) {
      contact.id = uuidv4();
      contact.editStatus = false;
      this.contacts.push(contact);
      this.saveContacts();
    },
    editContact(contact) {
      const index = this.contacts.findIndex(c => c.id === contact.id);
      this.contacts[index].editStatus = true;
      this.saveContacts();
    },
    saveEdition(contact) {
      const index = this.contacts.findIndex(c => c.id === contact.id);
      this.contacts[index].editStatus = false;
      this.saveContacts();
    },
    deleteContact(contact) {
      this.contacts = this.contacts.filter(c => c.id !== contact.id);
      this.saveContacts();
    },
    importContacts(importedContacts) {
      this.contacts = [...this.contacts, ...importedContacts];
      this.saveContacts();
    },
    saveContacts() {
      localStorage.setItem('contacts', JSON.stringify(this.contacts));
    },
  },
});
</script>

<style scoped>
.app {
  width: 90vw;
  margin: 0 auto;
  padding: 20px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

h2 {
  text-align: center;
  margin-bottom: 10px;
}

.search-input {
  display: block;
  width: 100%;
  padding: 8px 12px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 20px;
  box-sizing: border-box;
}

</style>
