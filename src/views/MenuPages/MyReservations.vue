<template>
    <ion-page>
        <!-- <ion-header>
            <ion-toolbar>
                <ion-back-button default-href="/main" slot="start"></ion-back-button>
                <ion-title>My Reservations</ion-title>
            </ion-toolbar>
        </ion-header> -->
        <ion-content class="ps-background-color">
            <section id="reservation">
                <div>
                    <ion-button class="ps-back-button ps-input-border-radius" router-link="/main" slot="start" mode="ios" color="warning">
                    <ion-icon slot="icon-only" :icon="chevronBackOutline" color="dark"></ion-icon>
                    </ion-button>
                </div>
                <div>
                    <ion-icon color="warning" :icon="list" slot="start"></ion-icon>
                    <ion-label color="light">My Reservations</ion-label>
                </div>
                <div>
                    <ion-card class="ps-background-color-secondary" v-for="reservation in reservations" :key="reservation.id">
                        <ion-card-content>
                            <ion-row>
                                        <p class="ps-title">Reservation Number:</p> <span class="ps-title-value"><strong>#{{reservation.id}}</strong></span>
                            </ion-row>
                            <ion-row>
                                <ion-col>
                                        <p class="ps-title">Department 
                                            Name: </p><span class="ps-title-value"><strong>{{reservation.parking_slot.department.abbreviation}}</strong></span>
                                </ion-col>
                                <ion-col>
                                        <p class="ps-title">Parking 
                                            Space Code: </p><span class="ps-title-value"><strong>{{reservation.parking_slot.parking_number}}</strong></span>
                                </ion-col>
                            </ion-row>
                            <ion-row>
                                <ion-col>
                                    <ion-button class="ps-input-border-radius"  @click="btnClick(reservation.id)" expand="block" size="large" color="warning">
                                        Scan Qr
                                    </ion-button>
                                </ion-col>
                                <ion-col>
                                    <ion-button class="ps-input-border-radius"  @click="removeReserveSlot(reservation.id)" expand="block" size="large" color="danger">
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
import {IonPage, /* IonHeader, IonToolbar, IonTitle, */IonIcon, IonLabel, IonCol,/*  IonButtons, */ IonContent, IonCard, /* IonCardHeader, */ IonCardContent, /* IonBackButton, */ /* IonCardTitle, */ /* IonCardSubtitle, */ IonButton, IonRow, /* IonGrid, */ modalController } from '@ionic/vue';
import QrScanner from '/src/components/QRScanner.vue';
import { qrCodeOutline, closeCircleOutline,chevronBackOutline, list} from 'ionicons/icons';

export default {
    setup() {
        
    },
    ionViewWillEnter () {
        this.getReservations();
        var today = new Date();
        var date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
        this.date = date;
        console.log(date);
    },
    mounted (){
        this.getReservations();
    },
     data: () => ({
        reservations: {},
        qrCodeOutline,
        date: null,
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
                console.log(this.reservations);
            })
        },
        btnClick(id){
            this.$axios.get('user/v1/reservation/show/' + id).then(({data}) => {
                this.selectedToScan = data;
                // console.log(this.selectedToScan);
                this.scanQrCode(this.selectedToScan);
            })
        },
        removeReserveSlot(id){
            this.$axios.delete('user/v1/reservation/delete/' + id).then(({data}) => {
                console.log(data);
                this.getReservations();
            })
        },
        async scanQrCode(datas) 
        {
            console.log(datas);
                const modal = await modalController
                    .create({
                    component: QrScanner,
                        componentProps: {
                            qr_value: datas,
                            // datetimepicker: DatePicker
                        },
                    })
                modal.onDidDismiss()
                .then(() => {
                    // this.test()
                    console.log('Gumawas sa qr page');
                });
                return modal.present();
        },
    },
}
</script>
<style>
#reservation>div:first-child {
    margin-top: 35px;
    margin-left: 15px;
}
#reservation>div:nth-child(2) {
    display: flex;
    align-items: center;
    gap: 20px;
    margin: 46px 0;
    padding: 0 30px;
}
#reservation>div:nth-child(2) ion-icon {
    font-size: 30px;
}
#reservation>div:nth-child(2) ion-label {
    font-size: 20px;
}

.ps-title {
    font-size: 14px !important;
    letter-spacing: 2px;
}

.ps-title-value {
    font-size: 18px;
    color: #fff;
    white-space: pre;
}
#reservation ion-row {
    align-items: center;
}

#reservation ion-row:first-child  {
    gap: 13px;
    margin-left: 5px;
}
#reservation .ps-input-border-radius {
    --border-radius: 30px !important;
    text-transform: capitalize;
    font-weight: 900;
}
</style>