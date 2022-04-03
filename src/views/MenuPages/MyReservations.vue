<template>
    <ion-page>
        <ion-content class="ps-background-color">
            <section id="reservation">
                <div>
                    <div>
                        <ion-button class="ps-back-button" router-link="/main" slot="start" mode="ios">
                        <ion-icon class="ps-back-button-icon" slot="icon-only" :icon="chevronBackOutline"></ion-icon>
                        </ion-button>
                    </div>
                    <div>
                        <ion-icon :icon="list" slot="start"></ion-icon>
                        <ion-label>My Reservations</ion-label>
                    </div>
                </div>
                <div  style="margin: 225px 15px 0;">
                    <ion-card class="ps-background-color-secondary" v-for="reservation in reservations" :key="reservation.id">
                        <ion-card-content>
                            <ion-row>
                                <p class="ps-title">Reservation Number:</p> <span class="ps-title-value"><strong>#{{reservation.id}}</strong></span>
                            </ion-row>
                            <ion-row>
                                <ion-col>
                                    <p class="ps-title">Department Name: </p><span class="ps-title-value"><strong>{{reservation.parking_slot.department.abbreviation}}</strong></span>
                                </ion-col>
                                <ion-col>
                                    <p class="ps-title">Parking Space Code: </p><span class="ps-title-value"><strong>{{reservation.parking_slot.parking_number}}</strong></span>
                                </ion-col>
                            </ion-row>
                            <ion-row>
                                <ion-col>
                                    <ion-button class="ps-btn"  @click="btnClick(reservation.id)" expand="block" :disabled="!checkIfToday(reservation.date)">
                                        Scan QR
                                    </ion-button>
                                </ion-col>
                                <ion-col>
                                    <ion-button class="ps-btn"  @click="removeReserveSlot(reservation.id)" expand="block">
                                        Cancel
                                    </ion-button>
                                </ion-col>
                            </ion-row>
                        </ion-card-content>
                    </ion-card>
                </div>
            </section>
        </ion-content>
    </ion-page>
</template>
<script>
import {IonPage, useIonRouter, useBackButton, IonIcon, IonLabel, IonCol,/*  IonButtons, */ IonContent, IonCard, /* IonCardHeader, */ IonCardContent, /* IonBackButton, */ /* IonCardTitle, */ /* IonCardSubtitle, */ IonButton, IonRow, /* IonGrid, */ /* modalController */ } from '@ionic/vue';
// import QrScanner from '/src/components/QRScanner.vue';
import { qrCodeOutline, closeCircleOutline,chevronBackOutline, list} from 'ionicons/icons';
import { Camera } from '@capacitor/camera';
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';
// import moment from 'moment';


export default {
    setup() {
        const ionRouter = useIonRouter();
        useBackButton(0, () => {
            console.log(ionRouter);
            // if (!ionRouter.canGoBack()) {
            //     if(window.confirm('are you sure wanna exit app.')){
            //     console.log('true');
            //     App.exitApp();
            //     } else {
            //     console.log('false');
            //     }
            // }
        });
    },
    ionViewWillEnter () {
        this.getReservations();
        var today = new Date();
        var date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
        this.dateToday = date;
        console.log(this.getReservations());
        // this.checkDateValidation();
        Camera.requestPermissions();
        console.log(Camera.checkPermissions()['camera']);
    },
    mounted (){
        this.getReservations();
    },
     data: () => ({
        reservations: [],
        qrCodeOutline,
        dateToday: null,
        closeCircleOutline,
        chevronBackOutline,
        list,
        selectedToScan: [],
     }),
    components: {
        IonPage, /* IonHeader, IonToolbar, IonTitle, */IonIcon, IonLabel, IonCol,/*  IonButtons, */ IonContent, IonCard, /* IonCardHeader, */ IonCardContent, /* IonBackButton, */ /* IonCardTitle, */ /* IonCardSubtitle, */ IonButton, IonRow, /* IonGrid, */
    },
    methods: {
        getReservations(){
            this.$axios.get('user/v1/user/show/' + this.$store.getters.user.id).then(({data}) => {
                console.log(data);
                this.reservations = data.reservations
                // console.log(this.reservations);
            })
        },
        btnClick(id){
            this.$axios.get('user/v1/reservation/show/' + id).then(({data}) => {
                this.selectedToScan = data;
                // console.log(this.selectedToScan);
                this.startScan(this.selectedToScan);
            })
        },
        removeReserveSlot(id){
            this.$axios.delete('user/v1/reservation/delete/' + id).then(({data}) => {
                console.log(data);
                this.getReservations();
            })
        },
        async startScan(data) {
            BarcodeScanner.hideBackground(); // make background of WebView transparent
            document.body.classList.add("qrscanner"); // add the qrscanner class to body
            const result = await BarcodeScanner.startScan(); // startscan
            document.body.classList.remove("qrscanner"); // remove the qrscanner from the body
         
            if(result.hasContent){
                console.log(result.hasContent);
                console.log(result);
                console.log(data);
                if(data.parking_slot.qrCode_value == result.content){
                    if(data.parking_slot.status == '3'){
                        this.successNotify('Scanned Successfully!');
                        this.stopScan();
                        this.$axios.get('user/v1/reservation-done/'+ data.id).then(({data}) => {
                            console.log(data);
                            this.$router.push('/main');
                        });
                    } else {
                        this.stopScan();
                        this.errorNotify('Parking slot is already been scanned');
                    }
                } else {
                    this.stopScan();
                    this.errorNotify('Qr scanned is invalid. Try again.');
                }
            }
        },
        stopScan() {
            BarcodeScanner.showBackground();
            BarcodeScanner.stopScan();
        },
    },
}
</script>
<style>
#reservation>div:first-child {
        /* border: 1px solid white; */
    position: fixed;
    width: -webkit-fill-available;
    top: 0;
    z-index: 9;
    background: #25384A;
}
#reservation div>div:first-child {
    margin-top: 35px;
    margin-left: 15px;
}
#reservation div>div:nth-child(2) {
    display: flex;
    align-items: center;
    gap: 20px;
    margin: 46px 0;
    padding: 0 30px;
}
#reservation div>div:nth-child(2) ion-icon {
    font-size: 30px;
    color: #F2C904;
}
#reservation div>div:nth-child(2) ion-label {
    font-size: 20px;
    color: #F6FBFF;
}

.ps-title {
    font-size: 11px !important;
    letter-spacing: 2.5px;
    text-transform: uppercase;
}

.ps-title-value {
    font-size: 18px;
    color: #F6FBFF;
    white-space: pre;
}
#reservation ion-row {
    align-items: center;
}
#reservation ion-row:nth-child(2) {
    margin-top: 5px;
    margin-bottom: 15px;
}
#reservation ion-row:nth-child(2) ion-col {
    display: flex;
    align-items: center;
}
#reservation ion-row:nth-child(2) ion-col .ps-title {
    width: 65%;

}
#reservation ion-row:first-child  {
    gap: 13px;
    margin-left: 5px;
}
#reservation ion-row:last-child ion-col:last-child .ps-btn {
    --background: #D21F1F;
    --color: #F6FBFF;
}
#reservation .ps-btn {
    height: 45px;
}

#reservation .ps-background-color-secondary{
    margin: 30px 0;
    border-radius: 10px;
}
</style>