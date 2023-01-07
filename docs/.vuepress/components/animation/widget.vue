<template>
    <div class="widget" :style="{'height': height}">
        <div class="menu" :class="{'active': !!isActive}">
            <div class="toggle" @click="toggleMenu"><i class="iconfont icon-plus"></i></div>
            <li style="--i:0;">
                <a href="#"><i class="iconfont icon-home"></i></a>
            </li>
            <li style="--i:1;">
                <a href="#"><i class="iconfont icon-music"></i></a>
            </li>
            <li style="--i:2;">
                <a href="#"><i class="iconfont icon-biji"></i></a>
            </li>
            <li style="--i:3;">
                <a href="#"><i class="iconfont icon-a-youjianchakanyoujianfasongyoujianshouyoujian-01"></i></a>
            </li>
            <li style="--i:4;">
                <a href="#"><i class="iconfont icon-shenghuo"></i></a>
            </li>
            <li style="--i:5;">
                <a href="#"><i class="iconfont icon-toTop2"></i></a>
            </li>
        </div>
    </div>
</template>
<script>
// import Particles from '../../components/base/particles.vue'
export default {
    props: {
        show: {
            default: true,
            type: Boolean
        }
    },
    data() {
        return {
            isActive: false,
            height: 0,
        }
    },
    components: {
    },
    methods: {
        toggleMenu() {
            this.isActive = !this.isActive
        }
    },
    mounted() {
        this.height = window.innerHeight - 58 + 'px'
    }
}
</script>
<style lang="scss" scoped>
$--my-primary-color: #2CBFBE;
$--my-secondary-color: #2CBFBE66;
$--my-hover-color: #ff1252;

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.widget {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    width: 100%;
    background-image: linear-gradient(45deg #8460ed, #ff1252);
}

.menu {
    position: relative;
    width: 200px;
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
    // background: #0f0;
}

.menu li {
    position: absolute;
    left: 0;
    list-style: none;
    transform-origin: 100px;
    transition: 0.5s;
    transition-delay: calc(0.1s * var(--i));
    transform: rotate(0deg) translateX(80px);
}

.menu.active li {
    transform: rotate(calc(360deg / 6 * var(--i)));
}

.menu li a {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    background: $--my-secondary-color;
    border-radius: 50%;
    transform: rotate(calc(360deg / -6 * var(--i)));
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.15);
    color: #111;
    transition: 0.5s;
}

.menu li a:hover {
    color: $--my-hover-color;
}

.toggle {
    position: absolute;
    width: 60px;
    height: 60px;
    background: $--my-primary-color;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 99;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.15);
    font-size: 2em;
    transition: transform 1.25s;
}

.menu.active .toggle {
    transform: rotate(315deg);
}
</style>