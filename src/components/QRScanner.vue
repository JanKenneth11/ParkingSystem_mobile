<template>
    <ion-page>
    <ion-content force-overscroll="false" class="ps-background-color">
        <section id="main-book-process">
            <div>
                <ion-button class="ps-back-button" @click="closeModal" slot="start" >
                <ion-icon class="ps-back-button-icon" slot="icon-only" :icon="chevronBackOutline"></ion-icon>
            </ion-button>
            </div>
            <p>{{error}}</p>
            <div style="height: 50vh; width: 80vw; margin: auto; margin-top: 120px">
                <!-- <barcode-scanner @show="startScan()" class="mb">
                    <div style="color: yellow;" class="frame"></div>
                </barcode-scanner> -->
            </div>
        </section>
    </ion-content>
    </ion-page>
</template>
<script>
// import { QrcodeStream} from 'vue-qrcode-reader';
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';
import { Camera } from '@capacitor/camera';
import { chevronBackOutline} from 'ionicons/icons';

import {IonPage, IonButton, IonIcon, IonContent, modalController,} from '@ionic/vue';

export default {
    setup() {
        
    },
    components: {
        IonPage, IonButton, IonIcon, IonContent,/*  QrcodeStream */
    },
    props: {
        qr_value: {
            required: true,
            type: Object,
        }
    },
    data: () => ({
        chevronBackOutline, 
        error: '',
    }), 
    mounted (){
        console.log(this.qr_value)
        console.log(this.qr_value.parking_slot.qrCode_value)
        Camera.requestPermissions();
        this.startScan();
    },
    created() {
        Camera.requestPermissions();
    },
    methods: {
        onDecode(data) {
            console.log(data,'scanned_qr');
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
        async onInit (promise) {
            try {
                await promise
            } catch (error) {
                  console.log(error)
                if (error.name === 'NotAllowedError') {
                this.error = "ERROR: you need to grant camera access permission"
                } else if (error.name === 'NotFoundError') {
                this.error = "ERROR: no camera on this device"
                } else if (error.name === 'NotSupportedError') {
                this.error = "ERROR: secure context required (HTTPS, localhost)"
                } else if (error.name === 'NotReadableError') {
                this.error = "ERROR: is the camera already in use?"
                } else if (error.name === 'OverconstrainedError') {
                this.error = "ERROR: installed cameras are not suitable"
                } else if (error.name === 'StreamApiNotSupportedError') {
                this.error = "ERROR: Stream API is not supported in this browser"
                } else if (error.name === 'InsecureContextError') {
                this.error = 'ERROR: Camera access is only permitted in secure context. Use HTTPS or localhost rather than HTTP.';
                } else {
                this.error = `ERROR: Camera error (${error.name})`;
                }
            }
        },
        closeModal(){
            return modalController.dismiss();
        },
        async startScan() {
              BarcodeScanner.hideBackground(); // make background of WebView transparent
            document.body.classList.add("qrscanner"); // add the qrscanner class to body
            const result = await BarcodeScanner.startScan(); // startscan
            document.body.classList.remove("qrscanner"); // remove the qrscanner from the body
         
            if(result.hasContent){
                console.log(result.hasContent);
            }
        }
    }
}
</script>

<style scoped>
.frame {
  border-style: solid;
  border-width: 2px;
  border-color: red;
  height: 200px;
  width: 200px;
  position: absolute;
  top: 0px;
  bottom: 0px;
  right: 0px;
  left: 0px;
  margin: auto;
}
</style>