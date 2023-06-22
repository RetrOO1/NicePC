<template>
    <my-header/>
    <div class="page">
        {{ this.info }}
    </div>
    <div>
        <!-- write here -->
    </div>
</template>

<script>
import axios from 'axios'
import MyHeader from '../MyHeader.vue'
export default {
    components: {
        MyHeader
    },
    data() {
        return {
            path: 'https://nicepc-1-s2894705.deta.app/',
            info: []
        }  
    },
    methods: {  
        async fetchThisCpuInfo() {
            await axios.get(`https://nicepc-1-s2894705.deta.app/item/one?item_id=${this.$route.params.id}`)
            .then((res) => {
                if (res.status === 200) {
                    this.info = res.data
                }
            })
        }
    },
    beforeMount() {
        this.fetchThisCpuInfo()
    }
}
</script>

<style>
.page {
    margin-top: 100px;
    font-size: 20px;
}
</style>