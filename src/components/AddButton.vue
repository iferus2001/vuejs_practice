<template>
  <div>
    <button @click="$emit('add-block')" :disabled="isDisabled" class="btn primary" >Add</button>  
    </div>
</template>

<script>
  export default {
    props: {
      input: {
        type: String,
        required: true
      },
      isTypeAlreadyAdded: {
        type: Boolean,
        required: true
      }

    },

    computed: {
      isDisabled() {
      return this.input.length < 3 || this.isTypeAlreadyAdded 
      },

    },

  }
  
</script>

<style>

</style>