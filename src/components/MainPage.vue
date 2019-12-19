<template >
    <div>
        <div>
            <table>
                <thead>
                    <tr>
                        <th v-for="key in lists" :key="key.id"> 
                            {{ key.text }}
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in users" :key="user.id">
                        <td>
                            {{ user.id }}
                        </td>
                        <td>
                           <img class="image--profile" v-bind:src="user.photoUrl" alt="avatar--block">
                        </td>
                        <td>
                            {{ user.firstName }}
                        </td>
                        <td>
                            {{ user.lastName }}
                        </td>
                        <td>
                            {{ user.jobTitle }}
                        </td>
                        <td>
                            <button @click="getProfile(user)">View candidate</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="container" v-show="active">
            <div class="content">
                <i @click="closeModal()" class="material-icons">clear</i>   
                <div class="information">
                    <img class="image--profile" v-bind:src="userId.photoUrl" alt="avatar--block">
                    <p>ID: {{ userId.id }}</p>
                    <p>NAME: {{ userId.firstName }}</p>
                    <p>SURNAME: {{ userId.lastName }}</p>
                    <p>LAST WORK: {{ userId.jobTitle }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
    data(){
        return{
            url: 'https://cors-anywhere.herokuapp.com/https://fakedata.dev/users/v1/get_users',
            users: 0,
            userId: 0,
            active: false,
            lists: [
                { text: '' },
                { text: '' },
                { text: 'Name' },
                { text: 'Surname' },
                { text: 'Last Work' },
                { text: '' },
            ]

        }
    },

    methods: {
        getUsers(){
            axios.get(this.url)
                .then((response) => {
                    this.users = response.data.filter(function(number) {
                        return number.id % 2 == 0;
                    });
                })
        },

        getProfile(user){
            this.userId = user
            this.active = true
        },
        
        closeModal(){
        this.active = false
        },
    },

    created: function(){
        this.getUsers();
    }
}
</script>

<style scoped>
    table{
        margin: 5% auto;
        position: relative;
        width: 60%;
    }

    th,td{
        text-align: center
    }

    .image--profile{
        width: 50px;
        height: 50px;
        position: relative;
        border: 1px solid #00dfb7;
        border-radius: 50%;
    }

    button{
        padding: 5% 15%;
        background-color: #00dfb7;
        border-radius: 10px;
        border: none;
        color: #fff;
        cursor: pointer;
    }

    .container{
        width: 35%;
        height: 35vh;
        position: fixed;
        margin: auto;
        text-align: center;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 100;
        background-color: #fff;
        box-shadow: 0 0 10px rgba(0, 0, 0, .5);
    }

    .content{
        width: 100%;
        height: 100%;
        position: relative;
    }

    i{
        left: 45%;
        margin: 2%;
        position: relative;
        transition: 1s;
        cursor: pointer;
    }

    i:hover{
        color: #fff;
        border-radius: 50%;
        background-color: rgba(34, 34, 33, 0.603);
    }
</style>
