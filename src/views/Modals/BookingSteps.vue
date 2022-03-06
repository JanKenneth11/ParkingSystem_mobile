<template>
    <ion-content mode="ios" class="ps-background-color">
        <section id="main-book-process">
            <div>
                <ion-button class="ps-back-button" @click="closeModal" slot="start" mode="ios">
                <ion-icon class="ps-back-button-icon" slot="icon-only" :icon="chevronBackOutline" ></ion-icon>
            </ion-button>
            </div>
            <div>
                <br/>
                <ion-row class="ion-text-uppercase">
                    <ion-col size="12" class="ps-col-grid">
                        <ion-card class="ps-background-color-secondary"  style="margin: 0;">
                            <ion-card-content class="ps-card-content" style="padding: 18px 15px;">
                                <span>Reserve 
                                    Number:</span>
                                <p class="ps-card-content"><strong style="color: #fff;">{{slotConfirmation.parking_number}}</strong></p>
                            </ion-card-content>
                        </ion-card>
                        <ion-card class="ps-background-color-secondary"  style="margin: 0;">
                            <ion-card-content class="ps-card-content" style="padding: 18px 15px;">
                                <span>Department 
                                    Name:</span>
                                <p class="ps-card-content"><strong style="color: #fff;">{{slotConfirmation.parking_number}}</strong></p>
                            </ion-card-content>
                        </ion-card>
                    </ion-col>
                    <ion-col size="12" class="ps-col-grid">
                        <ion-card class="ps-background-color-secondary"  style="margin: 0;">
                            <ion-card-content class="ps-card-content" style="padding: 18px 15px;">
                                <span>Parking 
                                    Space Code: </span>
                                <p class="ps-card-content"><strong style="color: #fff;">{{slotConfirmation.parking_number}}</strong></p>
                            </ion-card-content>
                        </ion-card>
                         <ion-card class="ps-background-color-secondary"  style="margin: 0;">
                            <ion-card-content class="ps-card-content" style="padding: 18px 15px;">
                                <span>Vehicle Type:</span>
                                <p class="ps-card-content"><strong style="color: #fff;">{{slotConfirmation.type}}</strong></p>
                            </ion-card-content>
                        </ion-card>
                    </ion-col>
                </ion-row>
            </div>
            <br/>
            <div class="ps-date-time">
                <ion-label>
                    <ion-icon :icon="calendarOutline"></ion-icon>
                    <span style="margin-left: 10px">Date / Time Picker</span>
                </ion-label>
                <ion-row>
                    <ion-col size="12" class="ps-date-picker">
                        <ion-card class="ps-card">
                            <ion-card-content style="height: 100%; position: relative;">
                                <div class="ps-date-display">
                                    <p>{{getMonthFromDate(getData.start_date)}}</p>
                                    <span>{{getDayFromDate(getData.start_date)}}</span>
                                </div>
                                <ion-datetime v-model="getData.start_date" display-format="MMMM DD" style="height: 100%;"></ion-datetime>
                            </ion-card-content>
                        </ion-card>
                        <ion-label class="ps-custom-label">To</ion-label>
                        <ion-card class="ps-card">
                            <ion-card-content style="height: 100%; position: relative;">
                                <div class="ps-date-display">
                                    <p>{{getMonthFromDate(getData.end_date)}}</p>
                                    <span>{{getDayFromDate(getData.end_date)}}</span>
                                </div>
                                <ion-datetime v-model="getData.end_date" display-format="MMMM DD" style="height: 100%;">
                                </ion-datetime>
                            </ion-card-content>
                        </ion-card>
                    </ion-col>
                    <ion-col size="12">
                    <br/>
                        <ion-slides mode="ios" pager="false" :options="slideOpts">
                            <ion-slide v-for="(hour) in hours" :key="hour.value">
                                <ion-button mode="ios" class="ps-slider-buttons ps-background-color-secondary" :class="hour.value == getData.start_time || hour.value == getData.end_time ? 'time-bg-active' : ''" @click="buttonStateGetHour(hour.value)" :disabled="checkTimes(hour.value)" >
                                    <p>
                                        <ion-label>{{hour.text}}</ion-label>
                                        <ion-label>{{hour.type}}</ion-label>
                                    </p>
                                </ion-button>
                            </ion-slide>
                        </ion-slides>
                    </ion-col>
                </ion-row>
            </div>
            <div class="place-btn">
                <ion-button class="ps-btn" @click="createReservation" :disabled="!getData.start_date + !getData.end_date + !getData.start_time + !getData.end_time" expand="block" size="large" color="warning">Reserve Space</ion-button>
            </div>
        </section>
    </ion-content>
</template>
<script>
import {
    modalController,
    loadingController,
    IonButton,
    IonIcon,
    IonContent,
    IonCard,
    IonCardContent,
    IonDatetime,
    IonLabel,
    IonCol,
    IonRow,
    IonSlide,
    IonSlides,
} from '@ionic/vue';
import { chevronBackOutline, calendarOutline } from 'ionicons/icons';
import ModalSuccess from '/src/views/Modals/BookSuccess.vue';
import ModalFailed from '/src/views/Modals/BookFailed.vue';
// import moment from 'moment';


export default {
    name: 'ModalBookingSteps',
    props: {
        slotConfirmation: {
            required: true,
            type: Object,
        }
    },
    components: {
        IonButton,
        IonIcon,
        IonContent,
        IonCard,
        IonCardContent,
        IonDatetime,
        IonLabel,
        IonCol,
        IonRow,
        IonSlide,
        IonSlides,
    },
    data: () => ({
        chevronBackOutline,
        calendarOutline,
        getData: {
            user_id: '',
            slot_id: '',
            start_date: '',
            end_date: '',
            start_time: '',
            end_time: '',
        },
        hours: [
            {
                text: '6:00',
                value: 6,
                type: 'am',
            },
            {
                text: '7:00',
                value: 7,
                type: 'am',
            },
            {
                text: '8:00',
                value: 8,
                type: 'am',
            },
            {
                text: '9:00',
                value: 9,
                type: 'am',
            },
            {
                text: '10:00',
                value: 10,
                type: 'am',
            },
            {
                text: '11:00',
                value: 11,
                type: 'am',
            },
            {
                text: '12:00',
                value: 12,
                type: 'pm',
            },
            {
                text: '1:00',
                value: 13,
                type: 'pm',
            },
            {
                text: '2:00',
                value: 14,
                type: 'pm',
            },
            {
                text: '3:00',
                value: 15,
                type: 'pm',
            },
            {
                text: '4:00',
                value: 16,
                type: 'pm',
            },
            {
                text: '5:00',
                value: 17,
                type: 'pm',
            },
            {
                text: '6:00',
                value: 18,
                type: 'pm',
            },
            {
                text: '7:00',
                value: 19,
                type: 'pm',
            },
            {
                text: '8:00',
                value: 20,
                type: 'pm',
            },
        ],
    }),
    setup() {
        // Optional parameters to pass to the swiper instance. See http://idangero.us/swiper/api/ for valid options.
        const slideOpts = {
            initialSlide: 0,
            slidesPerView: 4,
            speed: 400
            };
        return { slideOpts }
    },
    ionViewWillEnter () {
        // console.log(this.slotConfirmation)
        this.getData.slot_id = this.slotConfirmation.id;
        this.getData.user_id = this.$store.getters.user.id;

    },
    mounted (){
        // console.log(this.slotConfirmation);
        // console.log(this.slotConfirmation.parking_number);
        // console.log(this.$store.getters.user.id);
        this.getData.slot_id = this.slotConfirmation.id;
        this.getData.user_id = this.$store.getters.user.id;
        console.log(this.getData);

    },
    methods : {
        checkTimes(value){
            if(!this.getData.start_time || !this.getData.end_time){
                return false
            }
            else {
                return value > this.getData.start_time && value < this.getData.end_time
            }
        },
        closeModal(){
          return modalController.dismiss();
        },
        confirmSlot(){
            this.presentLoadingWithOptions();
        },
        async createReservation() {
            this.getData.start_date = new Date(this.getData.start_date).toLocaleDateString();
            this.getData.end_date = new Date(this.getData.end_date).toLocaleDateString();
            const loading = await loadingController
                .create({
                    spinner: 'circular',
                    showBackdrop: true,
                    translucent: true,
                });
            loading.present();
            this.$axios.post('user/v1/reservation/create', this.getData).then(({data}) => {
                 console.log(data.error);
                 console.log(this.getData);
                //  loading.dismiss();
                if(data.error){
                    this.openFailed();
                    loading.dismiss();
                } else {
                    this.openSuccess();
                    loading.dismiss();
                }
             });
            console.log(this.getData);
        },

        async openSuccess() 
        {
            const modal = await modalController
                .create({
                component: ModalSuccess,
                    componentProps: {
                        slotInfo: this.slotConfirmation,
                    },
                })
            var vm = this;
            modal.onDidDismiss()
            .then(() => {
                // this.test()
                vm.$router.push('/myreservations');
                modalController.dismiss();
                // console.log('gumawas ine');
            });
            return modal.present();
        },
        async openFailed() 
        {
            const modal = await modalController
                .create({
                component: ModalFailed,
                    componentProps: {
                        slotInfo: this.slotConfirmation,
                    },
                })
            modal.onDidDismiss()
            .then(() => {
                // this.test()
                modalController.dismiss();
                // console.log('gumawas ine');
            });
            return modal.present();
        },
        buttonStateGetHour(value){
            if(this.getData.start_time == ''){
                this.getData.start_time = value;
            } else if (value > this.getData.start_time) {
                this.getData.end_time = value;
            } else if (value < this.getData.start_time) {
                this.getData.start_time = value;
            } else {
                if(this.getData.start_time == value) {
                    this.getData.start_time = '';
                }
                if(this.getData.end_time == value) {
                    this.getData.end_time = '';
                }
            }
            console.log(this.getData);
        },
    },
}
</script>
<style >
.modal-header h2 {
    font-size: 28px;
    margin: 0;
    padding: 20px 0;
    font-weight: 900;
}
.modal-header ion-button{
    font-size: 16px;
    padding: 0;
}
.modal-header ion-button>button.button-native{
    padding: 0;

}
.ps-date-time>ion-label:first-child {
    margin: 10px 15px;
    display: flex !important;
    align-items: center;
}
.ps-date-time>ion-label:first-child ion-icon {
    color: #fec309;
    font-size: 24px;
}
.ps-date-time>ion-label:first-child span {
    color: #fff;
    text-transform: uppercase;
    letter-spacing: 2px;
}
.confirmation-datetime ion-icon {
    font-size: 25px;
}
.confirmation-datetime ion-icon {
    margin-left: 10px;
}
.ps-date-picker ion-card {
    margin:10px;
    width: 100%;
    height: 100%;
}
.ps-date-picker ion-card:last-child {
    margin-left: 5px;
    margin-right: 15px;
}
.ps-date-picker ion-card:first-child {
    margin-right: 5px;
    margin-left: 15px;
}
/* section#main-book-process>div:last-child{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 2%;
} */
.ps-col-grid {
    display: grid;
    grid-template-columns: auto auto;
    padding: 5px 15px;
    gap: 10px;
}
section#main-book-process>div:first-child {
    margin-top: 35px;
    margin-left: 15px;
}

.ps-date-picker {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    height: 175px;
}
.ps-custom-label {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 9;
    background: #fec309;
    border: 10px solid var(--ion-color-primary);
    border-radius: 75px;
    padding: 10px;
    margin: 0;
    text-transform: uppercase;
    font-weight: 900;
}
.swiper-pagination {
    display: none;
}
.ps-slider-buttons{
    width: 90%;
    height: 90px;
}
.ps-slider-buttons p {
    display: flex;
    flex-direction: column;
}
.ps-slider-buttons ion-label {
    margin: 3px 0;
}
.ps-slider-buttons ion-label:first-child {
    letter-spacing: 3px;
    font-weight: 900;
}
.datetime-text {
    opacity: 0 !important;
}

ion-datetime {
    color: transparent !important;
}
.ps-date-display {
    position: absolute;
    text-align: center;
    left: 50%;
    /* z-index: 9; */
    transform: translate(-50%, 0);
}
.ps-date-display p:first-child {
    font-size: 23px;
    letter-spacing: 1.7px;
    margin: 20px 0 -15px;
}
.ps-date-display p, .ps-date-display span {
    color: #25384A;
    font-size: 50px;
    font-weight: bold;
    text-transform: uppercase;
}
.time-bg-active {
    --background: var(--ion-color-warning) !important;
    color: var(--ion-color-dark) !important;
}
.place-btn {
    margin-top: calc(100%/30);
    padding: 0 15px;
}
.ps-card-content {
    display: flex;
    flex-direction: column;
    gap: 10px;
}
.ps-card-content p,
.ps-card-content span {
    letter-spacing: 4.5px;
}
.ps-card-content span {
    font-size: 12px;
}
ion-card:last-child .ps-card-content span {
    width: min-content;
}
.ps-card-content p {
    color: var(--ion-color-secondary);
}
</style>