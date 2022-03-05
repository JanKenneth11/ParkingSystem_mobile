<template>
    <ion-page>
        <ion-header collapse="condensed" class="ion-no-border" mode="ios">
            <ion-toolbar class="ps-dashboard-header ps-background-color">
                <!-- <ion-col slot="start" size="6"> -->
                    <ion-button class="ps-hover-remove" mode='ios' @click="openMenu" fill="clear" slot="start">
                        <ion-icon :icon="menuOutline" slot="icon-only" color="light"></ion-icon>
                    </ion-button>
                    <ion-title class="ps-background-image ion-text-center">
                        <ion-img alt="logo" src="/assets/img/Logo-big.png"></ion-img>
                        <!-- <img alt="logo" id="header_logo" height="40" src="/assets/img/Logo-big.png"> -->
                    </ion-title>
                    <ion-button class="ps-hover-remove" mode='ios' fill="clear" slot="end">
                    <!-- <ion-button class="ps-hover-remove" mode='ios' @click="openMenu" fill="clear" slot="end"> -->
                         <ion-badge class="ps-badge" color="danger">1</ion-badge>
                        <ion-icon :icon="notifications" slot="icon-only" color="light"></ion-icon>
                    </ion-button>
                <!-- </ion-col> -->
            </ion-toolbar>
        </ion-header>
         <ion-content force-overscroll="false" overflow-scroll="false" class="ps-background-color ion-padding">
             <section id="dashboard">
                <div class="ps-background-color">
                    <ion-row class="ion-align-items-center" style="gap:15px; padding: 0 10px;">
                        <ion-img src="/assets/img/filter.png" style="width: 14px; height: 15px;"></ion-img>
                        <p style="text-transform: uppercase;color: #fff;">Filters</p>
                    </ion-row>
                    <ion-row class="ion-justify-content-between">
                        <ion-col class="ps-filter" size="6">
                        <ion-select v-model="selectedDep" interface="popover" placeholder="By Department">
                            <ion-select-option v-for="department in departments" :key="department.id"  :value="department.id">{{department.abbreviation}}</ion-select-option>
                        </ion-select>
                        </ion-col>
                        <ion-col class="ps-filter" size="5">
                        <ion-select v-model="selectedType" interface="popover" placeholder="By Type" >
                            <ion-select-option v-for="type in types" :key="type"  :value="type">{{type}}</ion-select-option>
                        </ion-select>
                        </ion-col>
                    </ion-row>
                </div>
                <div class="ps-parking-slots ps-background-color-secondary">
                    <ion-grid class="ion-text-center ps-slots">
                        <ion-row class="ion-justify-content-between">
                            <ion-col size="5"  v-for="(slot,i) in parkingSpot" :key="slot.id">
                                <div class="avail" v-if="slot.status == 1">
                                    <ion-button mod="ios" class="loii-btn" expand="block" :color="slot.selected ? 'warning' : 'light'" @click="buttonState(i)" :fill="slot.selected ? 'solid' : 'clear'"><ion-label>{{slot.parking_number}}</ion-label></ion-button>
                                </div>
                                <div class="occu" v-else-if="slot.status == 2">
                                    <!-- <ion-button expand="block"><ion-label>{{slot.parking_number}}</ion-label></ion-button> -->
                                    <ion-img v-if="slot.type == 'Car'" src="/assets/img/car.png"></ion-img>
                                    <ion-img v-else-if="slot.type == 'Motor'" src="/assets/img/motorcycle.png"></ion-img>
                                </div>
                                <div class="resv" v-else-if="slot.status == 3">
                                    <ion-button expand="block" disabled color="medium"><ion-icon :icon="lockClosed" slot="icon-only"></ion-icon></ion-button>
                                </div>
                            </ion-col>
                        </ion-row>
                    </ion-grid>
                </div>
                <div>
                    <ion-button class="ps-btn-footer-action ps-input-border-radius" expand="block" size="large" color="warning" :disabled="!selectedSlot.id"  @click="proceed">Proceed</ion-button>
                </div>
             </section>
         </ion-content>
    </ion-page>
</template>
<script>
import {
    IonPage, IonHeader, IonToolbar, IonContent, IonGrid, IonRow, IonTitle, IonCol, IonImg, IonButton, IonBadge, IonIcon,  IonSelect, IonSelectOption, IonLabel, modalController ,menuController, alertController
    } from '@ionic/vue';
import {menuOutline, lockClosed, arrowUpOutline, carOutline, notifications,
} from 'ionicons/icons';
import ModalBookingSteps from '/src/views/Modals/BookingSteps.vue';
import ModalSuccess from '/src/views/Modals/BookSuccess.vue';
import ModalFailed from '/src/views/Modals/BookFailed.vue';
// import moment from 'moment'

export default {
    setup() {
        
    },
    data: () => ({
        menuOutline,
        notifications,
        lockClosed,
        arrowUpOutline,
        carOutline,
        selectedDep: null,
        selectedType: null,
        selectedSlot: {},
        types: ['All','Car','Motor'],
        parkingSpot: [],
        departments: [],
        getData: {
            user_id: '',
            slot_id: '',
            start_date: '',
            end_date: '',
            start_time: 6,
            end_time: 20,
        },
        date: Date.now(),
    }),
    ionViewWillEnter () {
        this.init();
        this.getData.user_id = this.$store.getters.user.id;
    },
    mounted (){
        this.init();
        this.getData.user_id = this.$store.getters.user.id;
    },
    
    components: {
        IonPage, IonHeader, IonToolbar, IonContent, IonGrid, IonRow, IonTitle, IonCol, IonImg, IonButton, IonBadge, IonIcon,  IonSelect, IonSelectOption, IonLabel, 
    },
    watch: {
        selectedDep: {
            handler(data){
                this.getParkingSlotsByDepartment(data)
            }
        },
        selectedType: {
            handler(data){
                this.segmentChanged(data);
            }
        },
    },
    methods: {
        init() {
            this.selectedType = 'All';
            this.selectedSlot = {};
            this.$axios.get('user/v1/department/all').then(({data}) => {
                if(data){
                    this.departments = data
                    this.selectedDep = data[0].id
                    this.types
                }
                this.getParkingSlotsByDepartment(this.selectedDep)
            });
        },
        async openSuccess() 
        {
            const modal = await modalController
                .create({
                component: ModalSuccess,
                    componentProps: {
                        slotInfo: this.selectedSlot,
                    },
                })
            var vm = this;
            modal.onDidDismiss()
            .then(() => {
                vm.$router.push('/myreservations');
                modalController.dismiss();
            });
            return modal.present();
        },
        async openFailed() 
        {
            const modal = await modalController
                .create({
                component: ModalFailed,
                    componentProps: {
                        slotInfo: this.selectedSlot,
                    },
                })
            modal.onDidDismiss()
            .then(() => {
                modalController.dismiss();
            });
            return modal.present();
        },
        segmentChanged(v_type){
            this.getParkingSlotsByDepartment(this.selectedDep,v_type)
        },
        async getParkingSlotsByDepartment(id,v_type = "All"){
            var vm = this;
            this.$axios.get('user/v1/department/show/'+id).then(({data}) => {
                vm.parkingSpot = v_type == "All" ? data.parking_slots : data.parking_slots.filter(parking_slot => parking_slot.type == v_type)

            });
        },
        proceed() {
            if(Object.keys(this.selectedSlot).length === 0) {
                this.warningAlert();
            } else {
                this.typeAlert();
            }
        },
        async bookASpot() 
        {
                const modal = await modalController
                    .create({
                    component: ModalBookingSteps,
                        componentProps: {
                            slotConfirmation: this.selectedSlot,
                        },
                    })
                modal.onDidDismiss()
                .then(() => {
                    this.init();
                });
                return modal.present();
        },
        async warningAlert() {
            const alert = await alertController
                .create({
                header: 'Warning!',
                subHeader: 'No Parking Spot selected',
                message: 'You didn\'t select any parking spot, please select a parking spot.',
                buttons: ['OK'],
                });
            await alert.present();

            const { role } = await alert.onDidDismiss();
            console.log('onDidDismiss resolved with role', role);
        },
        async typeAlert() {
            var vm = this;
            const alert = await alertController
                .create({
                    header: 'Select a type',
                    subHeader: '',
                    message: '',
                    inputs: [
                        {
                            type: 'radio',
                            value: '1',
                            label: 'Book a Spot'
                        },
                        {
                            type: 'radio',
                            value: '2',
                            label: 'For Today'
                        }
                    ],
                    buttons: [
                        {
                            text: 'OK',
                            handler: (data) => {
                                if(data == 1){
                                    this.bookASpot();
                                } 
                                if(data == 2) {
                                    vm.getData.start_date = new Date(vm.date).toLocaleDateString();
                                    vm.getData.end_date = new Date(vm.date).toLocaleDateString();
                                    vm.getData.slot_id = vm.selectedSlot.id;

                                    this.$axios.post('user/v1/reservation/create', vm.getData).then(({data}) => {
                                        if(data.error){
                                            this.openFailed();
                                        } else {
                                            this.openSuccess();
                                        }
                                    });
                                }
                            }
                        }
                    ],
                });
            return alert.present();

        },
        buttonState(i)
        {
            this.parkingSpot.forEach(element => {
                element.selected = false;
            });            
            if (this.parkingSpot[i].id == this.selectedSlot.id) {
                this.parkingSpot[i].selected == false;
                this.selectedSlot = {};
            } else {
                this.selectedSlot = this.parkingSpot[i];
                this.parkingSpot[i].selected = true;
            }
        },
        openMenu()
        {
            menuController.open('app-menu');
        },
    },
}
</script>
<style>
/* new css */
.ps-badge {
    position: absolute;
    top: 8px;
    left: -6px;
    width: auto;
    height: auto;
    font-size: 8px;
    border-radius: 50px;
}
.ps-background-color{
    --background: #25384A;
}
.ps-background-color-secondary {
    --background: #314355;
}
.ps-background-color>ion-button:last-child ion-icon {
    font-size: 24px;
}
ion-title.ps-background-image {
    --background: url(/assets/img/Logo-small.png) 0 0/100% 100% no-repeat;
}
.ps-dashboard-header ion-img {
    height: 40px;
}
.ps-parking-slots {
    width: 100%;
    height: 450px;
    background: #314355;
    padding: 30px;
    margin: 30px auto auto auto;
    overflow: scroll;
}
.ps-filter {
    background: #fff;
    border-radius: 30px;
}
.ion-no-border {
    border: 0 !important;
}
section#dashboard>div:last-child{
    /* position: absolute;
    left: 0;
    right: 0;
    bottom: 10%; */
    margin-top: 30px;
}
ion-select::part(icon) {
  color: #000;
  opacity: 1;
}

.ps-slots ion-col:nth-child(odd) {
    border: 1px dashed #fff;
    border-right: none;
}
.ps-slots ion-col:nth-child(even) {
    border: 1px dashed #fff;
    border-left: none;
}
section#dashboard .ps-btn-footer-action {
    padding: 0;
}
</style>