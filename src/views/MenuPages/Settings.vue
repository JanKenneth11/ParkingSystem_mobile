<template>
    <ion-page @ionDidOpen="userInfo">
        <!-- <ion-header>
            <ion-toolbar>
                <ion-back-button default-href="/main" slot="start"></ion-back-button>
                <ion-title>Settings</ion-title>
                <ion-button @click="updateUserInfo" slot="end" fill="clear">Done</ion-button>
            </ion-toolbar>
        </ion-header> -->
        <ion-content class="ps-background-color">
            <section id="main-profile">
                <div>
                    <ion-button class="ps-back-button ps-input-border-radius" router-link="/main" slot="start" mode="ios" color="warning">
                        <ion-icon slot="icon-only" :icon="chevronBackOutline" color="dark"></ion-icon>
                    </ion-button>
                </div>
                <div>
                    <ion-icon color="warning" :icon="person" slot="start"></ion-icon>
                    <ion-label color="light">My Account</ion-label>
                </div>
                <div>
                    <ion-avatar class="ps-user-img" v-if="$store.getters.user.image == '' | $store.getters.user.image == null">
                        <ion-img src="/assets/img/no-image/png"></ion-img>
                    </ion-avatar>
                    <ion-avatar class="ps-user-img" v-else>
                        <ion-img :src="'http://18.223.235.134/storage/' + $store.getters.user.image"></ion-img>
                    </ion-avatar>
                    <div>
                        <ion-button color="warning">
                            Change Profile
                        </ion-button>
                        <ion-label class="ps-font-italic">
                            {{$store.getters.user.name}}
                        </ion-label>
                    </div>
                </div>
                <div>
                    <div>
                    <ion-label>Name:</ion-label>
                    <ion-item class="ps-background-color">
                        <ion-input color="light" @ionInput="updateUser.name = $event.target.value;" :value="$store.getters.user.name" ></ion-input>
                        <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img>
                    </ion-item>
                    </div>
                    <div>
                    <ion-label>Email:</ion-label>
                    <ion-item class="ps-background-color">
                        <ion-input color="light" @ionInput="updateUser.email = $event.target.value;" :value="$store.getters.user.email" ></ion-input>
                        <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img>
                    </ion-item>
                    </div>
                </div>
                <!-- <ion-card>
                    <ion-card-content>
                        <ion-item lines="none">
                            <ion-icon  :icon="person" slot="start"></ion-icon>
                            <ion-input @ionInput="updateUser.name = $event.target.value;" :value="$store.getters.user.name"></ion-input>
                        </ion-item>
                        <ion-item lines="none">
                            <ion-icon  :icon="mail" slot="start"></ion-icon>
                            <ion-input @ionInput="updateUser.email = $event.target.value;" :value="$store.getters.user.email"></ion-input>
                        </ion-item>
                    </ion-card-content>
                </ion-card> -->
            </section>
        </ion-content>
    </ion-page>
</template>
<script>
import {IonPage, /* IonHeader, IonBackButton, IonTitle, IonToolbar, */ IonContent, IonInput, IonItem, IonButton, IonIcon, /* IonCard, */ /* toastController, loadingController */ } from '@ionic/vue';
import {cardOutline, mail, lockClosed, transgender, calendar, person, chevronBackOutline} from 'ionicons/icons';

export default {
    setup() {
        
    },
    data:() => ({
        cardOutline, mail, lockClosed, transgender, calendar, person, chevronBackOutline,
        updateUser: {
            name: '',
            email: '',
        },
    }),
    ionViewWillEnter () {
      console.log(this.$store.getters.user);
    //   this.setCustomer()
    },
    mounted() {
        console.log(this.$store.getters.user);
        // this.setCustomer()
    },
    components: {
        IonPage, /* IonHeader, IonBackButton, IonTitle, IonToolbar, */ IonContent, IonInput, IonItem, IonButton, IonIcon, /* IonCard, */
    },
    watch: {
        updateUser : {
        handler(val) {
            console.log(val.name,"name")
            console.log(val.email,"email")
        },
        deep : true,
        }
    },
    methods: {
        userInfo(){
            let temp = JSON.parse(JSON.stringify(this.$store.getters.user))
            this.updateUser.name = temp.name 
            this.updateUser.email = temp.email   
        },
        updateUserInfo(){
            console.log(this.updateUser);
        },
    }
}
</script>
<style>
#main-profile>div:first-child {
    margin-top: 35px;
    margin-left: 15px;
}
.ps-user-img {
    height: 112px;
    width: 112px;
    border: 7px solid #F2C904;
}
#main-profile>div:nth-child(2) {
    display: flex;
    align-items: center;
    gap: 20px;
    margin: 46px 0;
    padding: 0 30px;
}
#main-profile>div:nth-child(2) ion-icon { 
    font-size: 30px;
}
#main-profile>div:nth-child(2) ion-label {
    font-size: 20px;
 }
#main-profile>div:nth-child(3) {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}
#main-profile>div:nth-child(3)>div {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 5px;
}
#main-profile>div:nth-child(3)>div ion-button {
    font-weight: 900;
}
#main-profile>div:last-child {
    padding: 0 15px;
    margin: 50px 0;
}
#main-profile>div:last-child div {
    display: flex;
    flex-direction: column;
    margin-bottom: 15px;
}
#main-profile>div:last-child div ion-label {
    color: #5D6974;
    font-size: 16px;
}
#main-profile ion-item {
    --border-color:#707070 ;
}
.ps-font-italic {
    color: #5D6974;
    font-size: 18px;
    font-style: italic;
}

</style>