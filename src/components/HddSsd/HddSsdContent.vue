!<template>
    <div style="overflow-y: auto; margin-bottom: 100px;">
        <HddSsdItem :prebuilt_item="prebuilt_item" v-for="prebuilt_item in prebuilt_items"/>
    </div>
</template>

<script>
import axios from 'axios';
import HddSsdItem from '@/components/HddSsd/HddSsdItem.vue';
    export default {
        components: {
            HddSsdItem
        },
        data() {
            return {
                prebuilt_items: []
            }
        },
        methods:{
            async fetchPrebuilt() {
                try {
                    await axios.get('https://nicepc-1-s2894705.deta.app/item/?type_id=10').then((res) => {
                    this.prebuilt_items = res.data
                    })
                } 
                catch (error) {
                    alert('Error while posting to API')
                }
            }
        },
        beforeMount() {
            this.fetchPrebuilt()
        }
}
</script>

<style>

</style>