<template>
<div class="signup mt-5">
    <div class='mx-auto text-centor' style="width:600px; height:600px" >
        <p class='title text-center'>회원가입</p>
        <form @submit.prevent="signup">
            <v-text-field
            v-model="username"
            placeholder="아이디"
            required>
            </v-text-field>
            <p class='font-weight-bold' style='color:#D32F2F;' v-for="error in errormessage.username" :key="error">{{ error }}</p>
            <v-text-field
            v-model="password"
            placeholder="비밀번호"
            required
            type="password">
            </v-text-field>
            <p class='font-weight-bold' style='color:#D32F2F;' v-for="error in errormessage.password" :key="error">{{ error }}</p>
            <v-text-field
            v-model="password2"
            placeholder="비밀번호 재확인"
            required
            type="password">
            </v-text-field>
            <v-btn class="mr-4" @click="clear">다시 입력하기</v-btn>
            <v-btn @click="signup">회원가입</v-btn>
        </form>
    </div>
</div>
</template>

<script>
import axios from 'axios'
import router from '../router'
export default {
    name: 'signup',
    data() {
        return {
            signUpUser: {},
            username : '',
            password : '',
            password2 : '',
            errormessage : {username: [], password: []}
        }
    },
    methods: {
        signup() {
            this.signUpUser ={
                'username' : this.username,
                'password' : this.password,
                'password2' : this.password2,
                'like_movies' : [],
            }
            axios.post('http://127.0.0.1:8000/api/v1/accounts/', this.signUpUser)
                .then(() => {
                    this.errormessage = {username: [], password: []}
                    router.push({name:'home'})
                })
                .catch(error => {
                    this.errormessage.username = error.response.data.username
                    this.errormessage.password = error.response.data.password
                })
                this.signUpUser = {}
        },
        clear() {
            this.credentials.username = ''
            this.credentials.password = ''
        },
    }
}
</script>

<style>

</style>