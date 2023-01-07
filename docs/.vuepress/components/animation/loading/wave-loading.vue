<template>
    <div class="container" :style="{'height': height}">
        <div class="item" style="--delay: -1.1s"></div>
        <div class="item" style="--delay: -1.0s"></div>
        <div class="item" style="--delay: -0.9s"></div>
        <div class="item" style="--delay: -0.8s"></div>
        <div class="item" style="--delay: -0.7s"></div>
    </div>
</template>

<script lang="js">
export default {
    name: 'WaveLoading',
    data() {
        return {
            height: 0
        }
    },
    mounted() {
        this.height = window.innerHeight - 58 + 'px'
    }
}
</script>

<style lang="scss" scoped>
.container {
    height: 100%;
    width: 100%;
    // background-color: ;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}

.item {
    height: 60px;
    width: 16px;
    margin-right: 5px;
    background-color: aquamarine;
    animation: wave 1.5s infinite;
    animation-delay: var(--delay);
}

@keyframes wave {

    0%,
    40%,
    100% {
        transform: scale(0.41);
    }

    20% {
        transform: scale(1);
    }
}
</style>