<script setup lang="ts">
import AddContactDialog from './AddContactDialog.vue';
import ContactsList from './ContactsList.vue';
import { Ref, ref } from 'vue';

const addContactDialogRef = ref();
const isDialogOpen = ref(false);
let contactList: StoredContact[] = [];

interface StoredContact {
    name: string;
    email: string;
    address: string;
    phoneNumber: string;
}


interface NewContact {
    name: Ref<string>;
    email: Ref<string>;
    address: Ref<string>;
    phoneNumber: Ref<string>;
}

let newContact: NewContact = {
    name: ref(''),
    email: ref(''),
    address: ref(''),
    phoneNumber: ref('')
}

const isNotEmpty = (text: string) => text.trim().length > 0;

const isValidEmail = (email: string) => {
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,2}\.[0-9]{1,2}\.[0-9]{1,2}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
};

const validateInputs = () => {
    if (!isNotEmpty(newContact.name.value) || !isNotEmpty(newContact.address.value) || !isNotEmpty(newContact.phoneNumber.value) || !isValidEmail(newContact.email.value)) {
        alert('Please fill in all fields correctly.');
        return false;
    }
    addContact();
    toggleDialog();
    return true;
};



function toggleDialog() {
    isDialogOpen.value = !isDialogOpen.value;
    if (addContactDialogRef.value) {
        isDialogOpen.value ? addContactDialogRef.value.open() : addContactDialogRef.value.close();
    }
}


function addContact() {
    const contactToAdd: StoredContact = {
        name: newContact.name.value,
        email: newContact.email.value,
        address: newContact.address.value,
        phoneNumber: newContact.phoneNumber.value
    };
    contactList.push(contactToAdd);
    resetFormFields();
}


function resetFormFields() {
    Object.keys(newContact).forEach((key) => {
        const refKey = key as keyof NewContact;
        console.log(newContact[refKey].value);
        newContact[refKey].value = '';
    });
}

</script>

<template>
    <h1>Contact-List</h1>

    <button class="basic-btn" @click="toggleDialog">{{ isDialogOpen ? 'Close New Dialog' : 'Open New Dialog' }}</button>

    <AddContactDialog ref="addContactDialogRef">
        <form class="add-contact" @submit.prevent="validateInputs">
            <div class="btn-container">
                <button class="close-btn" @click="toggleDialog"><i class="mdi mdi-close"></i></button>
            </div>
            <input v-model="newContact.name.value" required class="basic-input" placeholder="Enter a name">
            <input v-model="newContact.email.value" required type="email" class="basic-input" placeholder="Enter an email">
            <input v-model="newContact.address.value" required class="basic-input" placeholder="Enter an address">
            <input v-model="newContact.phoneNumber.value" required class="basic-input" placeholder="Enter a phone number">
            <button type="submit" class="basic-btn">Add new contact</button>
        </form>
    </AddContactDialog>
    <ContactsList />
</template>

<style scoped>
.btn-container {
    display: flex;
    justify-content: flex-end;
}

.close-btn {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: 1px solid transparent;
    cursor: pointer;
    transition: 0.5s;
}

.close-btn:hover {
    border: 1px solid #535bf2;
}


.add-contact {
    margin: 20px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    border: 1px solid rgba(255, 255, 255, 0.87);
    padding: 20px;
    border-radius: 8px;
}

.basic-input {
    padding: 10px;
    border-radius: 8px;
}
</style>
