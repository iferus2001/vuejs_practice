<template>
  <div class="container">
    <p>
      <button class="btn primary"  @click="handleButtonClick">{{ hasComments ? 'Hide comments' : 'Load comments'}}</button>
    </p>
    <div class="card" v-if="hasComments && showComments">
      <h2>Comments:</h2>
      <ul class="list" v-for="comment in comments" :key="comment.id">
        <li class="list-item">
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>
  </div>

</template>

<script>
  export default {
    data() {
      return {
        showComments: true,
      }
    },
    emits: ['load', 'hide'],
    props: ['comments'],

    computed: {
      hasComments() {
        return this.comments.length > 0;
      }
    },

    methods: {
      handleButtonClick(){
        if (this.comments.length > 0) {
          this.showComments = !this.showComments;
          if (!this.showComments) {
            this.$emit('hide')
          }
        }
          else {
            this.$emit('load');
            this.showComments = true;
          }
        }
      }
    }
</script>

<style lang="scss" scoped>

</style>