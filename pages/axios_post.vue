<template>
    <div class="container">
        <h1>{{title}}</h1>
        <p>{{message}}</p>
        <hr>
        <table>
            <tr>
                <th>Email</th>
                <td>
                    <input type="mail" v-model="email">
                </td>
            </tr>
            <tr>
                <th>name</th>
                <td>
                    <input type="text" v-model="name">
                </td>
            </tr>
            <tr>
                <th>age</th>
                <td>
                    <input type="number" v-model="age">
                </td>
            </tr>
            <tr>
                <th>tel</th>
                <td>
                    <input type="tel" v-model="tel">
                </td>
            </tr>
            <tr>
                <th></th>
                <td>
                    <input type="submit" value="Click" @click="postData">
                </td>
            </tr>
        </table>
        <ul>
            <li v-for="(data, key) in jsonData" :key="key">
                <strong>{{ key }}</strong><br>
                {{ data }}
            </li>
        </ul>
    </div>
</template>

<script>
const axios = require('axios')
let url = 'https://nuxttest-8e035.firebaseio.com/person'
export default {
    data: ()=>{
        return {
            title: 'Axios',
            email: '',
            name: '',
            age: 0,
            tel: '',
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
        postData: function(event){
            let post_url = `${url}/${this.email}.json`
            console.log(post_url)
            let data = {
                name: this.name,
                age: this.age,
                tel: this.tel
            }
            axios.put(post_url, data).then((res)=>{
                console.log(res)
                this.name = ''
                this.email = ''
                this.age = 0
                this.tel = ''
                this.getData()
            }).catch((err)=>{
                // console.log(err)
                this.message = err
                this.jsonData = {}
            })
        },
        getData: function(){
            axios.get(`${url}.json`).then((res)=>{
                console.log(res)
                this.message = 'Got all data'
                this.jsonData = res.data
            }).catch((err)=>{
                console.log(err)
                this.message = err
                this.jsonData = {}
            })
        }
    },
    created: function(){
        this.getData()
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
