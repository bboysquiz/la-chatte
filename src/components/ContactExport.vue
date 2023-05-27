<template>
    <div class="contact-export">
        <h3>Экспорт контактов</h3>
        <button @click="exportContacts">Экспорт в CSV</button>
    </div>
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
        exportContacts() {
            const csvContent = 'data:text/csv;charset=utf-8,' +
                this.contacts
                    .map(
                        contact =>
                            [contact.firstName, contact.lastName, contact.phone, contact.email].join(',')
                    )
                    .join('\n');

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement('a');
            link.setAttribute('href', encodedUri);
            link.setAttribute('download', 'contacts.csv');
            document.body.appendChild(link);

            link.click();
            document.body.removeChild(link);
        },
    },
});
</script>
  
<style scoped>
.contact-export {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}

button {
    padding: 5px 10px;
    border: none;
    background-color: #f0f0f0;
    color: #333;
    cursor: pointer;
}
h3 {
    width: 100%;
    text-align: center;
}

button:hover {
    background-color: #ccc;
}
</style>
  