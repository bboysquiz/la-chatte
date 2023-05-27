<template>
    <table>
        <thead>
            <tr>
                <th>Имя</th>
                <th>Фамилия</th>
                <th>Телефон</th>
                <th>Email</th>
                <th>Действия</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="contact in contacts" :key="contact.id">
                <template v-if="!contact.editStatus">
                    <td>{{ contact.firstName }}</td>
                    <td>{{ contact.lastName }}</td>
                    <td>{{ contact.phone }}</td>
                    <td>{{ contact.email }}</td>
                </template>
                <template v-else>
                    <td><input type="text" v-model="contact.firstName" /></td>
                    <td><input type="text" v-model="contact.lastName" /></td>
                    <td><input type="text" v-model="contact.phone" /></td>
                    <td><input type="text" v-model="contact.email" /></td>
                </template>
                <td>
                    <button class="edit-button" v-if="!contact.editStatus" @click="editContact(contact)">Редактировать</button>
                    <button class="save-button" v-else @click="saveEdition(contact)">Сохранить</button>
                    <button class="delete-button" @click="deleteContact(contact)">Удалить</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>
  
<script>
import { defineComponent } from 'vue';

export default defineComponent({
    props: {
        contacts: {
            type: Array,
            required: true,
        },
    },
    methods: {
        editContact(contact) {
            this.$emit('editContact', contact);
        },
        saveEdition(contact) {
            this.$emit('saveEdition', contact);
        },
        deleteContact(contact) {
            this.$emit('deleteContact', contact);
        },
    },
});
</script>
  
<style scoped>

.search-input {
    margin-bottom: 10px;
    padding: 5px;
    width: 200px;
}
input{
    border-radius: 50px;
    padding-left: 15px;
}
table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    width: 200px;
    padding: 10px;
    text-align: center;
    border-bottom: 1px solid #ccc;

}

th {
    background-color: #f0f0f0;
}

.edit-button,
.save-button,
.delete-button {
    padding: 5px 10px;
    margin-right: 5px;
    border: none;
    background-color: #f0f0f0;
    color: #333;
    cursor: pointer;
}

.edit-button:hover,
.save-button:hover,
.delete-button:hover {
    background-color: #ccc;
}
</style>
  