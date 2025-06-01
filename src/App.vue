<template>
  <div class="container column">
    <form class="card card-w30">
      <div class="form-control">
        <label for="type">Type of block</label>
        <select id="type" v-model="selectedType">
          <option value="title" >Title</option>
          <option value="subtitle">Subtitle</option>
          <option value="avatar">Avatar</option>
          <option value="text">Text</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Value</label>
        <textarea id="value" v-model="input" rows="3" type="text"></textarea>
      </div>

      <add-button 
      :input="input"
      :isTypeAlreadyAdded="isTypeAlreadyAdded"
      @add-block="handleAddBlock"
      

      ></add-button>



    </form>
    <div class="card card-w70">
    <div v-for="(block, index) in sortedBlocks" :key="index" >
      <h1 v-if="block.type === 'title'">{{ block.value }}</h1>
      <div v-else-if="block.type === 'avatar'" class="avatar">
        <img :src="block.value">
      </div>
      <h2 v-else-if="block.type === 'subtitle'"> {{ block.value }}</h2>

      <p v-else-if="block.type === 'text'">{{ block.value }}</p>

      <h3></h3>
    </div>
  </div>
  </div>

  <app-loader v-if="loading"></app-loader>

  <app-comments-list 
  :comments="comments"
  @load="loadComments"
  
  >
</app-comments-list>


</template>

<script>
import AddButton from './components/AddButton.vue'
import AppCommentsList from './components/AppCommentsList.vue'
import AppLoader from './components/AppLoader.vue'
import axios from 'axios'


export default {
  data() {
    return {
      input: '',
      selectedType: 'title',
      blocks: [],
      loading: false,
      comments: [],
      error: null,
      
  
    }
    },

  methods: {
    handleAddBlock() {
      this.blocks.push({
        type: this.selectedType,
        value: this.input
      })
      this.input = ''
    },

    async loadComments() {
      this.loading = true;
      this.error = null;
      try {
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42');
        this.comments = data.map(comment => ({
        email: comment.email,
        body: comment.body,
        id: comment.id
      }));
      console.log('Comments were uploaded:', this.comments.length);

    } catch (error) {
      console.log('Failed', error);
    } finally {
      this.loading = false;
    }
  }
  },



  computed: {
    isTypeAlreadyAdded() {
      return this.blocks.some(block => block.type === this.selectedType)
    },

    sortedBlocks() {
      const PriorityMap = {
        title: 1,
        avatar: 2,
        subtitle: 3,
        text: 4,
      }

      return this.blocks.slice().sort((a, b) => {
        return PriorityMap[a.type] - PriorityMap[b.type]
      })
    }
  },
  


components: {AddButton, AppCommentsList, AppLoader},


}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>