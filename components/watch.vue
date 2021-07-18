<template>
    <div class="container">
        <div id="time">
            {{time.hours}}<div class="timeBlink">:</div>{{time.minutes}}
            </div>
        <div>{{Object.keys(dataTo).length?dataTo.name:'UTC'}} Time </div>
    </div>

</template>

<script>
export default {
    data(){
        return {
            now: new Date(),
                }
    },
    props:{
        dataTo: Object,
    },
    computed:{
        time(){
            const lt = this.now.getTime();
            const lo = this.now.getTimezoneOffset() * 60 * 1000;
            const localtime = lt+lo+Number(this.dataTo.timezone)*1000;
            const localtimeC = new Date(localtime);
            const hours = localtimeC.getHours();
            const minutes = localtimeC.getMinutes();
            return {hours, minutes}
        }

    },
    methods:{
        setDate(){
            this.now = new Date()
        }

    },
    mounted(){
        this.setDate()
        setInterval(() => this.setDate(), 1000)
    }

}
</script>

<style scoped>
#time{
    display: flex;
    justify-content: center;
    font-size: 50px
}
.timeBlink{
    animation: 1s blink infinite
}
@keyframes blink {
    50% {color: transparent;}
}
</style>