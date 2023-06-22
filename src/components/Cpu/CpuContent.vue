!<template>
    <div style="overflow-y: auto; margin-bottom: 100px;">
        <cpu-item :cpu_item="cpu_item" v-for="cpu_item in cpu_items"/>
    </div>
</template>

<script>
import axios from 'axios';
import CpuItem from '@/components/Cpu/CpuItem.vue';
    export default {
        components: {
            CpuItem
        },
        data() {
            return {
                cpu_items: []
            }
        },
        methods:{
            async fetchCpu() {
                try {
                    await axios.get('https://nicepc-1-s2894705.deta.app/item/?type_id=1').then((res) => {
                    this.cpu_items = res.data
                    })
                } 
                catch (error) {
                    alert('Error while posting to API')
                }
            }
        },
        beforeMount() {
            this.fetchCpu()
        }
}
</script>

<style>

</style>