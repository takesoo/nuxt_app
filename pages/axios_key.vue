<template>
    <div class="container">
        <h1>{{title}}</h1>
        <p>{{message}}</p>
        <div>
            <input type="text" v-model="find">
            <input type="submit" value="Click" @click="doClick">
        </div>
        <hr>
        <ul>
            <li v-for="data of jsonData" :key="data.key">{{ data }}</li>
        </ul>
    </div>
</template>

<script>
const axios = require('axios')
let url = 'https://nuxttest-8e035.firebaseio.com/person.json?orderBy="age"'
export default {
    data: ()=>{
        return {
            title: 'Axios',
            find: '',
            message: 'axios sample.',
            jsonData: {}
        }
    },
    // asyncData: async function(){
    //         let result = await axios.get(url)
    //         // console.log(result)
    //         return {jsonData: result.data}
    // },
    methods: {
        doClick: function(event){
            let renge = this.find.split('~')
            let age_url = `${url}&startAt=${renge[0]}&endAt=${renge[1]}`
            // let id_url = `${url}${this.find}"`
            console.log(age_url)
            axios.get(age_url).then((res)=>{
                console.log(res)
                this.message = `Got Id is ${this.find}`
                this.jsonData = res.data
            }).catch((err)=>{
                // console.log(err)
                this.message = err
                this.jsonData = {}
            })
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
        padding: 5px;
        font-size: 20pt;
    }

    pre {
        padding: 10px;
        font-size: 18pt;
        background-color: #efefef;
        white-space: pre-wrap;
    }

    hr {
        margin: 10px 0;
    }
    div {
        font-size: 20pt;
    }
</style>
