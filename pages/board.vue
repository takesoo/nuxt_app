<template>
    <section class="container">
        <div class="login" @click="doLogin">[login: {{ user_name }}]</div>
        <h1>{{ title }}</h1>
        <p class="message">{{ message }}</p>
        <div v-if="logined">
            <table>
                <tr>
                    <th>Message</th>
                    <td>
                        <input type="text" size="50" v-model="msg">
                    </td>
                    <td>
                        <button @click="add">投稿</button>
                    </td>
                </tr>
            </table>
            <hr>
            <ul v-for="(data, key) in json_data" :key="key">
                <li>
                    <div class="list1">{{ data.msg }}</div>
                    <div class="list2">{{ data.user }} ({{ data.posted }})</div>
                </li>
            </ul>
        </div>
    </section>
</template>

<script>
import firebase from 'firebase';

const axios = require('axios');

let config = {
    apiKey: "AIzaSyB9CrsQCqIVVDipQEMHpit6mX3PefLpZn8",
    authDomain: "nuxttest-8e035.firebaseapp.com",
    databaseURL: "https://nuxttest-8e035.firebaseio.com",
    projectId: "nuxttest-8e035",
    storageBucket: "nuxttest-8e035.appspot.com",
    messagingSenderId: "81676775969",
    appId: "1:81676775969:web:e9f5a2531c034b4b35a960",
    measurementId: "G-F6S3L36L6D"
};

firebase.initializeApp(config);

export default {
    data: function () {
        return {
            title: 'Board',
            message: 'ミニ伝言板。最新の投稿を表示します。',
            user_name: '',
            logined: true,
            msg: '',
            page: 0,
            num_per_page: 10,
            json_data: {},
        };
    },
    methods: {
        login: function () {
            let provider = new firebase.auth.GoogleAuthProvider();
            let self = this;
            firebase.auth().signInWithPopup(provider).then(function(result) {
                // console.log('result.user', result.user);
                // This gives you a Google Access Token. You can use it to access the Google API.
                var token = result.credential.accessToken;
                // The signed-in user info.
                self.user = result.user;
                self.user_name = result.user.displayName;
                self.message = 'ログインしました';
                let db = firebase.database();
                let ref = db.ref('board');
                ref.orderByKey().limitToLast(self.num_per_page).on('value', snapshot => {
                    if (firebase.auth().currentUser != null) {
                        let arr = [];
                        let data = snapshot.val();
                        Object.keys(data).forEach(key=>{
                            const item = data[key];
                            arr.unshift(item);
                        });
                        // console.log('arr', arr);
                        self.json_data = arr;
                    } else {
                        self.json_data = {};
                    }
                });

                }).catch(function(error) {
                // Handle Errors here.
                var errorCode = error.code;
                var errorMessage = error.message;
                // The email of the user's account used.
                var email = error.email;
                // The firebase.auth.AuthCredential type that was used.
                var credential = error.credential;
            // ...
            });
        },
        logout: function () {
            firebase.auth().signOut();
            this.user_name = '';
            this.json_data = {};
            this.message = 'ログアウトしました。';
        },
        doLogin: function () {
            if (firebase.auth().currentUser == null) {
                this.login();
            } else {
                this.logout();
            }
        },
        add: function () {
            if (firebase.auth().currentUser == null) {
                alert('ログインしないと投稿できません。');
                return;
            }
            let db = firebase.database();
            let user = firebase.auth().currentUser;
            console.log('user', user);
            let ref = db.ref('board');
            let self = this;
            let d = new Date();
            let dstr = `${d.getFullYear()}-${d.getMonth()+1}-${d.getDate()} ${d.getHours()}:${d.getMinutes()}:${d.getSeconds()}`;
            let id = d.getTime();
            let data = {
                msg: this.msg,
                user: user.displayName,
                posted: dstr,
            };
            firebase.database().ref(`board/${id}`).set(data);
            this.msg = '';
            this.message = '投稿しました。';
        }
    },
    created: function () {
        if (firebase.auth().currentUser == null) {
            this.login();
        }
        console.log('firebase.auth().currentUser', firebase.auth().currentUser);
    }
}
</script>

<style>
    .login {
        font-weight: bold;
        font-size: 12pt;
        cursor: pointer;
    }

    .list1 {
        text-align: left;
        font-size: 16pt;
    }

    .list2 {
        text-align: right;
        font-size: 10pt;
    }

    .container {
        padding: 5px 10px;
    }

    h1 {
        font-size: 60pt;
        color: #345980;
    }

    p {
        padding-top: 5px;
        margin: 10px 0;
        font-size: 20pt;
    }

    .message {
        font-size: 16pt;
    }

    div {
        font-size: 14pt;
    }

    ul {
        margin: 0 10px;
        background-color: aliceblue;
    }

    li {
        padding: 10px;
        font-size: 14pt;
    }

    tr th {
        width: 120px;
        background-color: darkblue;
        color: white;
        font-size: 16pt;
    }

    tr td {
        padding: 5px 10px;
        background-color: #eef;
        font-size: 14pt;
    }

    hr {
        margin: 10px 0;
    }

    input {
        font-size: 14pt;
    }

    button {
        font-size: 14pt;
    }
</style>
