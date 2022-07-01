<template>
    <div>
        <form @submit.prevent="submit">
            <input type="file" @change="previewFiles" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet">
            <button type="submit">
                Calculate
            </button>
        </form>
        <div v-if="ibnr" style="margin-top:10">
            IBNR: {{parseFloat(ibnr).toFixed(2)}}
        </div>
        <div v-if="lossTriangle" style="margin-top:10">
            <h3>Loss Triangles:</h3>
            <div>
                <table>
                    <tr>
                        <th>Date</th>
                        <th v-for="x in Object.keys(dateTriangle).length" v-bind:key="x">{{x}}</th>
                    </tr>
                    <tr v-for="(item, index) in dateTriangle" v-bind:key="index">
                        <th>{{item}}</th>
                        <td v-for="x in lossTriangle" v-bind:key="x[item]">
                            {{parseFloat(x[item]).toFixed(2)}}
                        </td>
                    </tr>
                </table>    
            </div>
        </div>
        <div v-if="ratio" style="margin-top:10">
            <h3>Link Ratios:</h3>
            <div>
                <table>
                    <tr>
                        <th>Date</th>
                        <th v-for="x in Object.keys(periodRatio).length" v-bind:key="x">{{x}}-{{x+1}}</th>
                    </tr>
                    <tr v-for="(item, index) in periodRatio" v-bind:key="index">
                        <th>{{item}}</th>
                        <td v-for="x in ratio" v-bind:key="x[item]">
                            {{parseFloat(x[item]).toFixed(2)}}
                        </td>
                    </tr>
                </table>    
            </div>
        </div>
    </div>
</template>
<script>
import axios from "axios"
export default {
    name: "PostComponent",
    data: () => ({
        formData: null,
        ibnr: null,
        lossTriangle: null,
        dateTriangle: null,
        periodRatio: null,
        ratio: null

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
                this.ibnr = res.data.ibnr
                this.lossTriangle = res.data.lossTriangle
                this.dateTriangle = Object.keys(this.lossTriangle[Object.keys(this.lossTriangle)[0]])
                this.ratio = res.data.ratio
                this.periodRatio = Object.keys(this.ratio[Object.keys(this.ratio)[0]])
            })
        }
    }
}
</script>