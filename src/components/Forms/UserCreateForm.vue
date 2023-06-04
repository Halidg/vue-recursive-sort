<template>
  <form class="form" @submit.prevent='submitForm'>
    <div class="form__wrapper">
      <label>Имя</label>
      <input v-model="form.name" class="form__input" type="text">
    </div>

    <div class="form__wrapper">
      <label>Телефон</label>
      <input v-model="form.phone" class="form__input" type="number">
    </div>

    <div class="form__wrapper">
      <label>Начальник</label>
      <select v-model="form.mainId" class="form__select">
        <option 
          v-for="item in userData"
          :key="item.id"
          :label="item.name"
          :value="item.id"  
        >
        </option>
      </select>
    </div>
    
    <button class="form__btn">Сохранить</button>
  </form>  
</template>

<script>
export default {
  props: {
    userData: {
      type: Array,
    }
  },
  data() {
    return {
      form: {
        name: '',
        phone: '',
        mainId: null
      }  
    }
  },
  methods: {
    submitForm() {
      this.$emit('addUser', this.form)  
    }
  }  
}
</script>

<style lang="scss" scoped>
.form {
  display: flex;
  flex-direction: column;
  gap: 25px;

  padding: 25px;

  &__wrapper {
    display: flex;
  }

  &__wrapper label {
    flex: 0 0 100px;
  }

  &__input {
    padding: 5px;
    border-radius: 5px;
  }

  &__select {
    padding: 5px;
    border-radius: 5px;  
    width: 50%;
  }

  &__btn {
    display: flex;
    justify-content: center;
    width: 35%;

    margin-top: 15px;
    padding: 5px;
    
  }
}
</style>