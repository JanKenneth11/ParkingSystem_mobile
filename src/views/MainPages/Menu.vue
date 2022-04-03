<template>
    <ion-menu side="start" type="overlay" menu-id="app-menu" content-id="main-content">
        <ion-content class="menu-content ps-background-color" scroll-y="false">
            <ion-grid>
                <ion-col>
                    <div class="ion-text-center ps-user-profile">
                        <ion-avatar class="ps-user-avatar" >
                            <ion-img :src="'http://18.223.235.134/storage/' + $store.getters.user.image"></ion-img>
                        </ion-avatar>
                        <div>
                            <h4>{{$store.getters.user.name}}</h4>
                            <span>{{$store.getters.user.email}}</span>
                        </div>
                        
                    </div>  
                </ion-col>
                <ion-col>
                    <div id="menu-list" class="ps-background-color">
                        <ion-item lines="none" button @click="openSettings" class="ps-background-color">
                            <ion-icon class="ps-icon" :icon="person" slot="start" ></ion-icon>
                            <ion-label class="ps-label-bold">
                                My Account
                            </ion-label>
                        </ion-item>
                        <ion-item lines="none" button @click="openMyReservations" class="ps-background-color">
                            <ion-icon class="ps-icon" :icon="listOutline" slot="start" ></ion-icon>
                            <ion-label class="ps-label-bold">
                                My Reservations
                            </ion-label>
                        </ion-item>
                    </div>
                </ion-col>
                <!-- <ion-col>
                     <div class="btn-logout">
                        <ion-button class="ps-btn" size="large" expand="block" @click="logoutConfirmation">
                            <ion-icon :icon="logOutOutline" slot="start" ></ion-icon>
                            Log Out
                        </ion-button>
                    </div>
                </ion-col> -->
            </ion-grid>
        </ion-content>
        <ion-footer class="menu-footer ion-no-border" mode="ios">
        <ion-toolbar class="ps-background-color">
            <ion-button class="ps-btn" size="large" expand="block" @click="logoutConfirmation">
                <!-- <ion-icon :icon="logOutOutline" slot="start" ></ion-icon> -->
                Log Out
            </ion-button>
        </ion-toolbar>
        </ion-footer>
    </ion-menu>
</template>

<script>
import {
    IonMenu,
    IonContent,
    IonButton,
    IonImg,
    // IonList,
    IonItem,
    IonLabel,
    IonIcon,
    IonAvatar,
    menuController,
    alertController
} from "@ionic/vue";
import { closeOutline, logOutOutline, /* settingsOutline, */ bookmarksOutline, informationCircleOutline, helpCircleOutline, person, listOutline,} from 'ionicons/icons';

export default {
    setup() {
        
    },
    data: () => ({
        closeOutline,
        logOutOutline,
        /* settingsOutline, */
        bookmarksOutline,
        informationCircleOutline,
        helpCircleOutline,
        person, 
        listOutline,
        user: {},
    }),
    components: {
        IonMenu,
        IonContent,
        IonButton,
        IonImg,
        IonItem,
        IonLabel,
        // IonList,
        IonIcon,
        IonAvatar,

    },
     ionViewWillEnter () {
          console.log(this.$store.getters.user);
         //   this.setCustomer()
        // this.user = this.$store.getters.user;
        //  console.log(this.user);
    },
    mounted() {
        console.log(this.$store.getters.user);
        // this.setCustomer()
    },
    methods: {
        
        closeMenu(){
            menuController.close("app-menu");
            //   this.$router.push(url);
        },
        openSettings(){
            console.log('oks');
            menuController.close("app-menu");
            this.$router.push('/settings');
        },
        openAbout(){
            console.log('oks');
            menuController.close("app-menu");
            this.$router.push('/about');
        },
        openMyReservations(){
            console.log('oks');
            menuController.close("app-menu");
            this.$router.push('/myreservations');
        },
        async logoutConfirmation() {
            const alert = await alertController
                .create({
                // cssClass: 'my-custom-class',
                header: 'Logout',
                message: 'Are you sure you want to logout?',
                buttons: [
                    {
                        text: 'Cancel',
                        handler: (data) => {
                            console.log(data);
                        }
                    },
                    {
                        text: 'Logout',
                        handler: () => {
                            var vm = this
                            // console.log(data);
                            this.$axios.post('user/v1/logout').then(({data}) => {
                                if(data){
                                    // console.log(data);
                                    localStorage.removeItem("token") 
                                    vm.$router.push('/login')
                                    menuController.close("app-menu");
                                }
                            })

                        }
                    },
                ],
                });
            await alert.present();
        },
    }
}
</script>
<style>
    .user-profile {
        display: flex;
        align-items: center;
        padding: 100px 20px 30px;
        background: #222428;
        color: #F6FBFF;
    }
    .user-profile>div {
        margin-left: 10px;
        line-height: 0.7;
    }
    .user-profile>div h4{
        margin: 0;
    }
    .menu-content ion-list {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
    }
    .ps-user-profile {
        padding: 25px;
    }
    .ps-user-profile div h4 {
        color: #F6FBFF;
        margin: 20px 0 12px 0;
    }
    .ps-user-profile div span {
        color: #F6FBFF;
        padding: 10px 15px;
        background: #314355;
        border-radius: 8px;
    }
    .ps-user-avatar {
        height: 112px;
        width: 112px;
        margin: auto;
        border: 7px solid #F2C904;
    }
    .ps-label-bold {
        --color: #F6FBFF !important;
        font-weight: 900;
        font-size: 18px;
    }
    #menu-list {
        margin: 15px;
    }
    #menu-list ion-item {
        border-bottom: 1px solid #5D6974;
        padding: 10px 15px;
    }
    #menu-list ion-item:first-child {
        border-top: 1px solid #5D6974;
    }
    /* .btn-logout {
        margin: 0 15px;
        margin-top: 31vh;
        margin-top: calc(100%/0.91);
    }
    .btn-logout .ps-btn {
        --background: #D21F1F;
        --color: #F6FBFF;
    } */
    ion-footer.menu-footer {
        padding: 15px;
        background-color: #25384A;
        --ion-background-color: #25384a;
        /* --border: none !important; */
    }
    ion-footer ion-toolbar.ps-background-color .ps-btn {
        --background: #D21F1F !important;
        --background-activated: #D21F1F !important;
        --background-focused: #D21F1F !important;
        --background-hover: #D21F1F !important;
        --color: #F6FBFF !important;
        color: #F6FBFF !important;
        --color-activated: #F6FBFF !important;
        --color-focused: #F6FBFF !important;
        --color-hover: #F6FBFF !important;
    }
</style>