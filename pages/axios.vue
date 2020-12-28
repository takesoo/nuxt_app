<template>
    <div class="container">
        <h1>{{title}}</h1>
        <p>{{message}}</p>
        <div>
            <input type="text" v-model="msg">
            <input type="submit" value="Click" @click="doClick">
        </div>
        <table>
            <thead>
                <th>key</th>
                <th>name</th>
                <th>tel</th>
                <th>age</th>
            </thead>
            <tbody>
                <tr v-for="(data, key) in jsonData" :key="key">
                    <td>{{ key }}</td>
                    <td>{{ data.name }}</td>
                    <td>{{ data.tel }}</td>
                    <td>{{ data.age }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import firebase from 'firebase'
const axios = require('axios')
let url = 'https://nuxttest-8e035.firebaseio.com/person.json'
// console.dir(firebase)
export default {
    data: ()=>{
        return {
            title: 'Axios',
            msg: '',
            message: 'axios sample.',
            jsonData: {}
        }
    },
    asyncData: async function(){
            let result = await axios.get(url)
            // console.log(result)
            return {jsonData: result.data}
    },
    methods: {
        doClick: function(event){
            axios.get(url+this.msg).then((res)=>{
                this.message = `get ID : ${res.data.id}`
                this.jsonData = res.data
            }).catch((err)=>{
                this.message = 'ERROR!'
                this.jsonData = {}
            })
        }
    },
    created: function(){
        // For Firebase JS SDK v7.20.0 and later, measurementId is optional
        const firebaseConfig = {
        apiKey: "AIzaSyB9CrsQCqIVVDipQEMHpit6mX3PefLpZn8",
        authDomain: "nuxttest-8e035.firebaseapp.com",
        databaseURL: "https://nuxttest-8e035.firebaseio.com",
        projectId: "nuxttest-8e035",
        storageBucket: "nuxttest-8e035.appspot.com",
        messagingSenderId: "81676775969",
        appId: "1:81676775969:web:e9f5a2531c034b4b35a960",
        measurementId: "G-F6S3L36L6D"
        };

        firebase.initializeApp(firebaseConfig)
        
        let provider = new firebase.auth.GoogleAuthProvider()
        let self = this
        firebase.auth().signInWithPopup(provider).then((res)=>{
            // console.log(res)
            self.message = `${res.user.displayName}/${res.user.email}`
        })
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
