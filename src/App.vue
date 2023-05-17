<template>
  <section>
    <h1>IronContacts</h1>
    <div>
      <button @click="addRandomContact">Add Random Contact</button>
      <button @click="sortByPopularity">Sort by popularity</button>
      <button @click="sortByName">Sort by name</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won Oscar</th>
          <th>Won Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts"
        :key="contact.id" >
          <td><img :src="contact.pictureUrl" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity }}</td>
          <td v-if="contact.wonOscar">🏆</td>
          <td v-else></td>
          <td v-if="contact.wonEmmy">🏆</td>
          <td v-else></td>
          <button @click="deleteContacts">Delete</button>
        </tr>
      </tbody>
    </table>
  </section>

</template>

<script>
import IronContacts from '../src/contacts.json'

export default {
  name: "App",
  data() {
    return {
      contactList: IronContacts,
      contacts: [],
      numberContacts: 5,
      remainingContacts: []
    }
  },
  created() {
    this.fiveContacts()
  },
  methods: {
    fiveContacts() {
      const contactsArrLength = this.contactList.length;
      this.contacts = this.contactList.slice(0,this.numberContacts);
      this.remainingContacts = this.contactList.slice(this.numberContacts,contactsArrLength)
    },
 
    addRandomContact() {
      const randomContact = Math.floor(Math.random() * this.remainingContacts.length)
      this.contacts.push(this.remainingContacts[randomContact]);
      this.remainingContacts.splice(randomContact,1)
      console.log(this.remainingContacts.length)
    },

    sortByPopularity() {
      this.contacts.sort(function (a,b){
        if(a.popularity < b.popularity){
          return 1;
        }
        if (a.popularity > b.popularity) {
          return -1;
        }
        return 0
      })
    },

    sortByName() {
      this.contacts.sort(function (a,b){
        if(a.name < b.name){
          return -1;
        }
        if (a.name > b.name) {
          return 1;
        }
        return 0
      })
    },

    deleteContacts(removeContact) {
      const contactIndex = this.contacts.indexOf(removeContact)
      this.contacts.splice(contactIndex, 1)
      this.remainingContacts.push(removeContact)
    }
  },
}

</script>

<style>
td img {
  width: 60px;
}
</style>
