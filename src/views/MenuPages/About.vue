<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-back-button default-href="/main" slot="start"></ion-back-button>
                <ion-title>About this App</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content>
            <ion-card>
                <ion-card-header>
                    <ion-card-title>
                        Meet The Team!
                    </ion-card-title>
                </ion-card-header>
                <ion-card-content>
                    <div>
                        <h5>Daniel B. Batican</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                        <p>- Back-end Developer</p>
                    </div>
                    <br/>
                    <div>
                        <h5>Angel Orbita</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                        <p>- Project Manager</p>
                    </div>
                    <br/>
                    <div>
                        <h5>Aira Asoncion</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                    </div>
                    <br/>
                    <div>
                        <h5>Erika Ibasco</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                    </div>
                    <br/>
                    <div>
                        <h5>Shonibie Jarito</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                    </div>
                    <br/>
                    <div>
                        <h5>Jan Kenneth Villa</h5>
                        <p>Bachelor of Science in Computer Engineering</p>
                        <p>-Mobile Dev</p>
                    </div>
                    <br/>
                </ion-card-content>
            </ion-card>
        </ion-content>
    </ion-page>
</template>

<script>
import {IonPage, IonHeader, IonToolbar, IonBackButton, IonTitle, IonContent, IonCard, IonCardHeader, IonCardTitle, IonCardContent  } from '@ionic/vue';

export default {
    setup() {
        
    },
      components: {
        IonPage, IonHeader, IonToolbar, IonBackButton, IonTitle, IonContent, IonCard, IonCardHeader, IonCardTitle, IonCardContent 
    },
}
</script>