<template>
    <div v-for="year in publishYears" v-bind:key="year+'y'" :class="[visibility ? visible : hidden]">
    {{randomHexCode()}}
    <div v-bind:style="{border: randomHexColor, borderRadius:'4px'}"></div>
    <button @click="changeSingleClass($event)">See More</button>
    <h3>{{year}}</h3>
    <!-- Book Cards Below -->
    <section v-for="book in allBooks" v-bind:key="book.title">
    <div v-if="year == book.latest" :class="'book-caps'">
    <h3>{{book.author}}</h3>
    <h4>{{book.title}}</h4>
    <div class="edits-published">
      <p>{{book.editions}} Editions</p>
      <p>First Published: {{book.firstYear}}</p>   
    </div>
    </div>
    <!-- Caps End --> 
    </section>
    </div>
</template>

<script>
export default {
  name: 'DisplayBooks',
  props: {
    publishYears: Array,
    allBooks: Array
  },
  methods: {
    randomHexCode(){
        let n = (Math.random() * 0xfffff * 1000000).toString(16);
        this.randomHexColor = `2px solid #${n.slice(0,6)}`
    },
    changeVisibility(){
      if(this.visibility == 'kanban-board-hidden'){
        this.visibility = 'kanban-board-visible';
      }
      else {
        this.visibility = 'kanban-board-hidden';
      }
    },
    changeSingleClass(event){
      let currentClass = event.target.parentElement.getAttribute('class')
      if(currentClass == this.hidden){
        event.target.parentElement.setAttribute('class',this.visible);
      }
      else {
        event.target.parentElement.setAttribute('class',this.hidden);
      }
    }
  },
  data: () => ({
        randomHexColor: undefined,
        visibility: false,
        hidden: 'kanban-board-hidden',
        visible: 'kanban-board-visible'
    })
}
</script>
