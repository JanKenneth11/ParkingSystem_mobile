<template>
    <ion-page @ionDidOpen="userInfo">
        <ion-content class="ps-background-color">
            <section id="main-profile">
                <div>
                    <ion-button class="ps-back-button ps-input-border-radius" @click="backToMain()" slot="start" mode="ios">
                        <ion-icon class="ps-back-button-icon" slot="icon-only" :icon="chevronBackOutline"></ion-icon>
                    </ion-button>
                </div>
                <div>
                    <ion-icon class="ps-icon" :icon="person" slot="start"></ion-icon>
                    <ion-label>My Account</ion-label>
                </div>
                <div>
                    <!-- <ion-avatar class="ps-user-img" v-if="$store.getters.user.image == '' | $store.getters.user.image == null">
                        <ion-img src="/assets/img/no-image/png"></ion-img>
                    </ion-avatar>
                    <ion-avatar class="ps-user-img" v-else>
                        <ion-img :src="'http://18.223.235.134/storage/' + $store.getters.user.image"></ion-img>
                    </ion-avatar> -->
                    <ion-avatar class="ps-user-img">
                        <ion-img :src="isNewImage ? user.image : 'http://18.223.235.134/storage/' +  user.image"></ion-img>
                        <!-- <ion-img :src="user.image ? user.image : ($store.getters.user.image ? $store.getters.user.image :'/assets/img/person-icon.png')"></ion-img> -->
                    </ion-avatar>
                    <div>
                        <ion-button class="ps-btn" @click="uploadOrTakePhoto">
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
                        <ion-input @ionBlur="updateUserInfo" @ionInput="user.name = $event.target.value;" :value="$store.getters.user.name" ></ion-input>
                        <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img>
                    </ion-item>
                    </div>
                    <div>
                    <ion-label>Email:</ion-label>
                    <ion-item class="ps-background-color">
                        <ion-input @ionBlur="updateUserInfo" @ionInput="user.email = $event.target.value;" :value="$store.getters.user.email" ></ion-input>
                        <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img>
                    </ion-item>
                    </div>
                    <div>
                    <ion-label>Type:</ion-label>
                    <ion-item class="ps-background-color">
                        <ion-input @ionBlur="updateUserInfo" @ionInput="user.email = $event.target.value;" :value="$store.getters.user.type" disabled ></ion-input>
                        <!-- <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img> -->
                    </ion-item>
                    </div>
                    <div>
                    <ion-label>User ID(RFID):</ion-label>
                    <ion-item class="ps-background-color">
                        <ion-input @ionBlur="updateUserInfo" @ionInput="user.email = $event.target.value;" :value="$store.getters.user.rfid_number" disabled ></ion-input>
                        <!-- <ion-img src="/assets/img/edit.png" slot="end" color="warning"></ion-img> -->
                    </ion-item>
                    </div>
                </div>
            </section>
        </ion-content>
    </ion-page>
</template>
<script>
import {IonPage, /* IonHeader, IonBackButton, IonTitle, IonToolbar, */ IonContent, IonInput, IonItem, IonButton, IonIcon, IonLabel, IonAvatar, IonImg /* IonCard, */ /* toastController, loadingController */ } from '@ionic/vue';
import {cardOutline, mail, lockClosed, transgender, calendar, person, chevronBackOutline} from 'ionicons/icons';
import { Camera, CameraResultType,CameraSource} from '@capacitor/camera';

export default {
    setup() {
        
    },
    data:() => ({
        cardOutline, mail, lockClosed, transgender, calendar, person, chevronBackOutline,
        // updateUser: {},
        user: {
            id: null,
            name: '',
            type: '',
            email: '',
            rfid_number: '',
            image: null,
        },
        isNewImage: false,
    }),
    ionViewWillEnter () {
        this.userInfo();
      console.log(JSON.parse(JSON.stringify(this.$store.getters.user)));
    },
    mounted() {
        this.userInfo();
        console.log(JSON.parse(JSON.stringify(this.$store.getters.user)));
    },
    components: {
        IonPage, /* IonHeader, IonBackButton, IonTitle, IonToolbar, */ IonContent, IonInput, IonItem, IonButton, IonIcon, IonLabel, IonAvatar, IonImg /* IonCard, */
    },
    watch: {
        user : {
        handler(val) {
            console.log(val)
        },
        deep : true,
        }
    },
    methods: {
        userInfo(){
            this.user = JSON.parse(JSON.stringify(this.$store.getters.user))
            // this.user.id = JSON.parse(JSON.stringify(this.$store.getters.user.id))
            // this.user.name = JSON.parse(JSON.stringify(this.$store.getters.user.name))
            // this.user.type = JSON.parse(JSON.stringify(this.$store.getters.user.type))
            // this.user.email = JSON.parse(JSON.stringify(this.$store.getters.user.email))
            // this.user.rfid_number = JSON.parse(JSON.stringify(this.$store.getters.user.rfid_number))
            // this.user.image = JSON.parse(JSON.stringify(this.$store.getters.user.image))
            console.log(this.user)
            // console.log(this.user)
        },
        async uploadOrTakePhoto() {
            const image = await Camera.getPhoto({
                quality: 90,
                allowEditing: true,
                source: CameraSource.Camera,
                resultType: CameraResultType.Base64,
            });

            this.user.image ="data:image/jpeg;base64," + image.base64String;
            this.isNewImage = true;
            // console.log()
            this.updateUserInfo();
        },
        updateUserInfo(){
            console.log(this.user);
            this.$axios.post('user/v1/user/update/'+this.user.id,this.user).then(({data}) => {
                if(!data.error){
                    this.successNotify('Update Successful');
                } else {
                    this.errorNotify('Error on updating. Try again.');
                }
            })
        },
        backToMain() {
            this.$router.push('/main');
            this.isNewImage = false;
        }
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
    color: #F6FBFF;
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
    letter-spacing: 2.5px;
    font-size: 14px;
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
    --color: #F6FBFF;
    --border-color:#707070 ;
}
.ps-font-italic {
    color: #5D6974;
    font-size: 18px;
    font-style: italic;
}

</style>