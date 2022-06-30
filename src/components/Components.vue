<template>
    <div>
        <form @submit.prevent="submit">
            <input type="file" @change="previewFiles" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet">
            <button type="submit">
                Submit
            </button>
        </form>
        <div v-if="ibnr" style="margin-top:10">
            IBNR: {{ibnr}}
        </div>
    </div>
</template>
<script>
import axios from "axios"
export default {
    name: "PostComponent",
    data: () => ({
        formData: null,
        ibnr: null
    }),
    methods:{
        previewFiles(event){
            this.formData = event.target.files;
        },

        async submit(){
            const data = new FormData();
            data.append('myfile', this.formData);
            await axios.post("http://127.0.0.1:5000/", this.formData)
            .then(res => {
                this.ibnr = res.data.IBNR
            })
        }
    }
}
</script>