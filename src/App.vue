<template>
  <div class="container">
    <button class="btn" @click="toggleModal">
      Добавить
    </button>    
    <UserList :userData="userDataChildren"/>
    <UserModal @close="isShowModal = false" v-if="isShowModal">
      <UserCreateForm @addUser="saveUser($event)" :userData="userData"/>  
    </UserModal>
  </div>
</template>

<script>
import UserList from './components/UserList.vue';
import UserModal from './components/Ui/UserModal.vue';
import UserCreateForm from './components/Forms/UserCreateForm.vue';

export default {
  name: 'App',
  components: {
    UserList,
    UserModal,
    UserCreateForm  
  },
  data() {
    return {
      isShowModal: false,
      userData: []
    }
  },
  created() {
    this.getUsers()
  },
  methods: {
    toggleModal() {
      this.isShowModal = !this.isShowModal
    },
    saveUser(data) {
      const main = this.userData.find(user => user.id === data.mainId)
      const result = {
        id: this.userData.length + 1,
        name: data.name,
        phone: data.phone,
        parentId: main ? main.id : null,
        level: main ? main.level + 1 : 1
      }
      this.userData.push(result)
      localStorage.setItem('userData', JSON.stringify(this.userData))
    },
    getUsers() {
      this.userData = JSON.parse(localStorage.getItem('userData'))  || []
    }
  },
  computed: {
    userDataChildren() {
      const userData = this.userData
        .map(elem => Object.assign(elem, { children: [] }))
        .sort((a, b) => (a.level < b.level ? 1 : -1))

      const res = userData.slice()

      userData.forEach(user => {
        res.forEach(item => {
          if (user.parentId === item.id) {
            item.children.push(user)
          }
        })
      })

      return res.filter(node => node.level === 1)
    }
  }
}
</script>

<style lang="scss">
.container {
  max-width: 1200px;
  width: 100%;
  padding: 35px;
  margin: 0;
}

.btn {
  margin-bottom: 40px;
}
</style>
