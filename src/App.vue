<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <ul>
      <li v-for="item in items"
          v-bind:class="{finished: item.isFinished}"
          v-on:click="toggleFinish(item)">
        {{ item.label }}
      </li>
    </ul>
    <input v-model="newItem" v-on:keyup.enter="addNew"/>

    <p>child tell me: {{ childWords }} </p>
    <components-a msgfromfather="you die !"
                  v-on:child-tell-me-something="listenToMyBoy"></components-a>
  </div>
</template>

<script>
import Store from './store'
import ComponentsA from './components/componentsA'

export default {
  data : function () {
    return {
      title : 'this is a todo list!',
      items : [
        // {
        //   label : 'coding',
        //   isFinished : false,
        // },{
        //   label : 'walking',
        //   isFinished : true,
        // }
        Store.fetch(),
      ],
      newItem : '',
      childWords : '',
    }
  },
  watch: {
    items : {
      handler: function (items) {
        // console.log(val, oldVal);
        Store.save(items);
      },
      deep: true,
    }
  },
  components:{ ComponentsA },
  methods: {
    toggleFinish: function (item) {
      item.isFinished = !item.isFinished;
    },
    addNew : function () {
      this.items.push({
        label : this.newItem,
        isFinished : false,
      });
      console.log(this.newItem);
      this.newItem = '';
    },
    listenToMyBoy: function (msg) {
      this.childWords = msg;
    }
  }
}
</script>

<style>
  .finished{
    text-decoration: underline;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
