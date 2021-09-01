<template>
    <div class="main-wrapper">
        <keep-alive>
            <router-view></router-view>
        </keep-alive>
    </div>
</template>

<script>

    export default {
    name: 'v-main-wrapper',
     props: {},
     computed:{},
     data(){
        return{
            title: 'Main Wrapper'
        }
     },
     methods:{},
     watch:{},

 }
</script>

    <style>
    .main-wrapper {
        display: flex;
        flex-direction: column;
        max-width: 900px;
        margin: 0 auto;

    }
    </style>




