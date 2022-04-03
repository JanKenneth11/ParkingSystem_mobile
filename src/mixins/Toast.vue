<script>
import {
  toastController
} from "@ionic/vue";
export default {
    data: () => ({

    }),
    methods: {
        async successNotify(message) {
            const toast = await toastController
                .create({
                    message: message,
                    duration: 1000,
                    color: "success",
                    position: "top",
                })
            return toast.present();
        },
        async errorNotify(message) { 
           const toast = await toastController
                .create({
                    message: message,
                    duration: 1000,
                    color: "danger",
                    position: "top",
                })

            return toast.present();
        }
    }
};
</script>