<template>
    <div class="container">
      <div style="text-align: left; font-size: 32px; font-weight: bold; padding: 20px 20px;">{{buttTxt}}</div>
      <div class="sub">

        
            <div v-if = "!isLogin">
                <label for="">Full Name</label>
                <input type="text" v-model="fullname" name="username" placeholder="Enter full name...">
            </div>
            <div>
                <label for="">Email</label>
                <input type="text" name="email" v-model="email" placeholder="Enter email...">
            </div>
            <div>
                <label for="">Password</label>
                <input type="password" name="password" v-model="password" placeholder="Enter password...">
            </div>
            <div v-if="!success" style="color: red; font-size: 14px;">*{{errorMsg}}</div>
            <div>
                <button type="submit" class="green" @click="register()">{{buttTxt}}</button>
            </div>
            <hr>
            <div>
                {{message}}<span style="cursor: pointer; color:#008631; font-weight: bold;" @click="registerOn()" > {{span}}</span>
            </div>
        
      </div>
    </div>
  </template>
  
<script>
import axios from 'axios'
  export default {
    data (){
        return{
            type: this.$route.params.type,
            buttTxt: 'LOGIN',
            isLogin: true,
            message: "Don't have any account yet?",
            span: ' Sign Up',
            fullname:'',
            email: '',
            password:'',
            success: true,
            errorMsg: ''
        }
    },
    methods: {
        async register(){
            if(this.buttTxt == 'REGISTER'){
                try {
                    const res = await axios.post('http://localhost:1337/createUser',{
                    fullname:this.fullname,
                    email:this.email,
                    password:this.password})
                    if(res.data == 'OK'){
                        this.success = res.data.success
                        this.isLogin = false
                        this.registerOn()
                        this.email = ''
                        this.password = ''
                    }
                    this.success = true
                } catch (error) {
                    console.log(error.response.data)
                    this.errorMsg = error.response.data.err
                    this.success = false
                }
            }
            else{
                try {
                    const res = await axios.post('http://localhost:1337/login', {
                        email: this.email,
                        password: this.password
                    })
                    console.log(res)
                    if(res.data.success){
                        this.success = true
                        let user = {
                            'id': res.data.user.id,
                            'name': res.data.user.fullname
                        }
                        localStorage.clear()
                        localStorage.setItem('user', JSON.stringify(user))
                        this.$router.push('/')
                    }
                } catch (error) {
                    console.log(error)
                    this.errorMsg = error.response.data.err
                    this.success = false
                }
            }
            
        },

        registerOn(){
            this.isLogin = !this.isLogin
            if(this.isLogin){
                this.buttTxt = 'LOGIN'
                this.message = "Don't have any account yet?"
                this.span = ' Sign Up'
            }
            else{
                this.buttTxt = 'REGISTER'
                this.message = 'Already have an account'
                this.span = ' Login'
            }
            
        }
    },
    mounted(){
        if(this.type == 'login') this.isLogin = false
        else this.isLogin = true
        console.log('type', this.type)
        this.registerOn()
    }
  }
  </script>
<style scoped>
.container{
    position: absolute;
    padding: 20px 10px;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20%;
    border-radius: 5px;
    margin: 0;
    box-shadow: 0px 0px 5px 2px rgba(0, 0, 0, 0.094);
}
.sub div{
    padding: 10px 20px;
}
.sub label{
    display: flex;
    margin-left: 5px;
    margin-bottom: 5px;
}
hr{
    background-color: #01702a;
    height: 1px;
    width: 70%;
    border: none;
}
</style>