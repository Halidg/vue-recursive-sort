<template>
  <div class="modal">
    <div class="modal__backdrop">
      <div class="modal__container">
        <div class="modal__content">
          <header class="modal__header">
            <button @click="closeModal">
              закрыть
            </button>
          </header>  
         
          <section class="modal__body">
            <slot/>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserModal',
  methods: {
    closeModal() {
      this.$emit('close');
    },
  },
};
</script>

<style lang="scss">
  .modal {
    &__backdrop {
      position: fixed;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.8);
      transition: opacity 0.3s ease;
      z-index: 9999;
    }

    &__container {
      position: fixed;
      top: 0;
      right: 0;
      left: 0;
      bottom: 0;
      width: auto;
      margin: 16px;
    }

    &__content {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      width: 100%;
      max-width: 500px;
      margin: 1.75rem auto;
      padding: 20px 30px;
      border-radius: 5px;
      color: #000;
      background-color: #fff;
      transform: translate(0, 0);
      transition: all 0.3s ease;
      box-sizing: border-box;
    }

    &__header {
      display: flex;
      justify-content: right;
      text-align: center;
    }
  }
</style>