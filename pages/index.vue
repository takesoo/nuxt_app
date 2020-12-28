<template>
  <div class="container">
    <div>
      <h1>{{ title }}</h1>
      <p>{{ $store.state.message }}</p>
      <hr>
      <div class="link" @click="$store.dispatch('doit')">
        <a @click.stop="$store.commit('reset')">
          clicked: {{$store.state.counter}}
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  asyncData(){
    return {
      title: 'Hello',
      message: 'This is message.',
      now: 'wait...'
    }
  },
  // methods: {
  //   doAction: function () {
  //     this.$store.state.counter++
  //   }
  // }
}
</script>

<style>
.container {
  padding: 5px 10px;
}

h1 {
  font-size: 60pt;
  color: #345980;
}

p {
  padding-top: 5px;
  font-size: 20pt;
}

pre {
  padding: 10px;
  font-size: 18pt;
  background-color: #efefef;
}

hr {
  margin: 10px 0;
}

a {
  font-size: 16pt;
}

.link {
  background-color: #def;
  padding: 10px;
}

</style>
