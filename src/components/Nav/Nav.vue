<template>
    <div class="nav">
      <router-link to="/goods" class="nav-item">
        點餐
        <i class="line"></i>
      </router-link>
      <router-link to="/ratings" class="nav-item">
        評價({{commentNum}})
        <i class="line"></i>
      </router-link>
      <router-link to="/seller" class="nav-item">
        商家
        <i class="line"></i>
      </router-link>
    </div>
</template>

<script>
export default {
    props: {
        commentNum: {
            type: Number,
            default:0 
        }
    }
}
</script>

<style scoped>
    .nav {
        display: flex;
        width: 100%;
        height: 40px;
        line-height: 40px;
        border-bottom: 1px solid #e4e4e4;
    }

    .nav .nav-item {
         flex: 1;
         text-align: center;
         font-size: 13px;
         text-decoration: none;
         color: #666666;
         position: relative;
    }

    .nav .active {
        color: #ffbb22;
    } 

    .nav .active .line {
        width: 63px;
        height: 2px;
        display: inline-block;
        background-color: #ffbb22;
        position: absolute;
        left: 50%;
        bottom: 0;
        margin-left: -32px;
    }
</style>