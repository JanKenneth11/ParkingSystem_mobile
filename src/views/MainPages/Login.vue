<template>
    <ion-page>
        <ion-content class="ps-background-image" force-overscroll="false">
            <section id="main">
                <div class="app-login">
                    <div class="app-logo">
                        <ion-img src="/assets/img/Logo-big.png"></ion-img>
                    </div>
                    <div class="app-login-form">
                        <div>
                            <ion-label color="light">Email</ion-label>
                            <ion-item class="ps-input-border-radius" lines="none">
                                <ion-input v-model="input.email" type="email" placeholder="Enter Email" required></ion-input>
                                <!-- <ion-icon  :icon="personCircleOutline" slot="start"></ion-icon> -->
                            </ion-item>
                        </div>
                        <div>
                            <ion-label color="light">Password</ion-label>
                            <ion-item class="ps-input-border-radius" lines="none">
                                <!-- <ion-icon :icon="lockClosed" slot="start"></ion-icon> -->
                                <ion-input v-model="input.password" :type="passwordStatus ? 'text' : 'password'" placeholder="Enter Password" required></ion-input>
                                <ion-icon  :icon="passwordStatus ? eyeSharp : eyeOffSharp" slot="end" @click="passwordStatus = !passwordStatus"></ion-icon>
                            </ion-item>
                        </div>
                    </div>
                    <div>
                        <ion-button class="ps-btn" @click="mobileLogin" expand="block" size="large">Sign In</ion-button>
                    </div>
                </div>
            </section>
        </ion-content>
    </ion-page>
</template>
<script>
import { IonPage,IonContent, IonInput, IonLabel, IonItem, IonButton, IonIcon,IonImg } from '@ionic/vue';
import {personCircleOutline, lockClosed, eyeSharp, eyeOffSharp} from 'ionicons/icons';

export default {
    components: {
         IonPage,IonContent, IonInput, IonItem, IonButton, IonIcon, IonLabel,IonImg
    },
    data:() => ({
        personCircleOutline,
        lockClosed,
        eyeSharp, 
        eyeOffSharp,
        passwordStatus: false,
        static: {
            email: 'dan@gmail.com',
            password: '1234'
        },
        input: {
            email: 'dan@gmail.com',
            password: '1234'
        }
    }),
    methods: {
        mobileLogin(){
            this.$axios.post('user/login', this.input).then(({data}) => {
                localStorage.setItem("token", data.token);
                this.$router.push('/main');
            })
        },
    }
}
</script>
<style>
.app-logo {
    margin-top: 67px;
    margin-bottom: 80px;
}
.app-logo ion-img {
    width: fit-content;
    margin: 0 auto;
}
.app-login > div:last-child {
    margin-top: calc(100%/1.5);
    padding: 0 15px;
}
.app-login-form {
    padding: 0 15px;
    display: flex;
    flex-direction: column;
    gap: 22px;
}
.app-login-form div ion-label {
    font-size: 18px;
}
.app-login-form div ion-item {
    margin-top: 7px;
}
.ps-btn-footer-action {
    padding: 0 15px;
    margin-top: 25px;
    font-size: 18px;
    --border-radius: 30px
}
.app-login-form ion-item ion-input {
    font-size: 14px;
}
.app-login-form ion-icon {
    color: #25384A;
    font-size: 20px;
}
.ps-background-image{
    --background: url(/assets/img/login-background.png) 0 0/100% 100% no-repeat;
}
.ps-color {
    color: #25384A;
}
.ps-input-border-radius {
    border-radius: 30px;
}
</style>