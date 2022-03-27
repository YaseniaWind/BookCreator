<template>
  <div>
       <ul class="nav nav-pills flex-column mb-auto">
      
      <sidebar-item v-for="post in postitems" :key="post.title" :item="post"  @selectitem="$emit('selectitem', post)" />

     
     <div class="input-group mb-3 mt-3">
  <input v-model="newitem" placeholder="Название главы" type="text" class="form-control">
  <button class="btn btn-outline-secondary" type="button" id="button-addon2" @click="handleAdd">Добавить</button>
</div>
     
    </ul>
  </div>
</template>

<script>
import SidebarItem from './SidebarItem.vue'
export default { 
  components: { SidebarItem },
  data(){
		return{
		newitem : ''
		}
},
  props: {
     postitems:{
            type: Array,
            required: true
        }
},
methods:{
  handleAdd(){
    this.$emit('additem', this.newitem);
    this.newitem = " ";
  }
}
}


</script>

<style>

</style>