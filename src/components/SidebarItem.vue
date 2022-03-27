<template>
  <li class="nav-item"  @click=" $emit('selectitem', item)" >
        <a href="#" class="nav-link " aria-current="page">
          <svg class="bi me-2" width="16" height="16"><use xlink:href="#home"></use></svg>
        {{item.title}}
        </a>
      </li>
     
</template>

<script>
export default {

  props: {
    item:{
            type: Object,
            required: true
        }
  },
  methods:{
      add(){
          console.log(this.item);
      }
  }

}
</script>

<style>

</style>