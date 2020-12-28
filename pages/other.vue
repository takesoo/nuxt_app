<template>
    <div class="container">
        <h1>{{ title }}</h1>
        <p>{{ message }}</p>
        <hr>
        <router-link to="/">Go to Top</router-link>
    </div>
</template>

<script>
export default {
    asyncData(){
        return {
            title: 'Other',
            message: 'これは別のページです'
        }
    },
}
</script>

<style scoped>

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

</style>