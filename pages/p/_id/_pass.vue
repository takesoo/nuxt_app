<template>
    <div class="container">
        <h1>{{ title }}</h1>
        <p v-html="message">no message</p>
        <hr>
        <div class="right">
            <router-link to="/">Go to Top</router-link>
        </div>
    </div>
</template>

<script>
export default {
    asyncData(){
        return {
            title: 'Login',
        }
    },
    computed: {
        message: function(){
            let id = this.$route.params.id
            let pass = this.$route.params.pass != undefined ? this.$route.params.pass : '*** no password ***'
            return 'ID: ' + id + '<br>PASS:' + pass
        }
    },
    validate({params}){
        if (params.id == undefined) {
            return false
        } else {
            return true
        }
    }
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

a {
  font-size: 16pt;
}

hr {
  margin: 10px 0;
}

.right {
    text-align: right;
}

</style>