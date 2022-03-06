<template>
    <ion-page>
    <!-- <ion-header>
        <ion-toolbar>
            <ion-button @click="closeModal" slot="start" fill="clear" color="dark">
                <ion-icon slot="icon-only" :icon="chevronBackOutline"></ion-icon>
            </ion-button>
        </ion-toolbar>
    </ion-header> -->
    <ion-content force-overscroll="false" mode="ios" class="ps-background-color">
        <section id="main-book-process">
            <div>
                <ion-button class="ps-back-button" @click="closeModal" slot="start" mode="ios">
                <ion-icon class="ps-back-button-icon" slot="icon-only" :icon="chevronBackOutline"></ion-icon>
            </ion-button>
            </div>
            <div style="height: 50vh; width: 80vw; margin: auto; margin-top: 120px">
                <qr-stream @decode="onDecode" class="mb">
                    <div style="color: yellow;" class="frame"></div>
                </qr-stream>
            </div>
        </section>
    </ion-content>
    </ion-page>
</template>
<script>
import { QrStream } from 'vue3-qr-reader';
import { Camera } from '@capacitor/camera';
import { chevronBackOutline} from 'ionicons/icons';

import {/* IonHeader, IonToolbar, */ IonPage, IonButton, IonIcon, IonContent, modalController,} from '@ionic/vue';

export default {
    setup() {
        
    },
    props: {
        qr_value: {
            required: true,
            type: Object,
        }
    },
    data: () => ({
        chevronBackOutline, 
    }),
    ionViewWillEnter () {
        console.log(this.qr_value)
        console.log(this.qr_value.parking_slot.qrCode_value)
    },
    mounted (){
        console.log(this.qr_value)
        console.log(this.qr_value.parking_slot.qrCode_value)

    },
    created() {
        Camera.requestPermissions();
    },
    components: {
        /* IonHeader, IonToolbar, */ IonPage, IonButton, IonIcon, IonContent, QrStream,
    },
    methods: {
        onDecode(data) {
            if(data == this.qr_value.parking_slot.qrCode_value){
                modalController.dismiss();
                Camera.dismiss();
                // console.log("Decoder = "+data);
                console.log("pareho");
                this.$axios.get('user/v1/reservation-done/'+ this.qr_value.id).then(({data}) => {
                        console.log(data);
                    // if(data.error){
                    //     this.openFailed();
                    //     // loading.dismiss();
                    // } else {
                    //     this.openSuccess();
                    //     // loading.dismiss();
                    // }
                });
                // get('reservation-done/{id}');
            } else {
                modalController.dismiss();
                Camera.dismiss();
                console.log('dire pareho')
            }
        },
        closeModal(){
            return modalController.dismiss();
        },
    }
}
</script>