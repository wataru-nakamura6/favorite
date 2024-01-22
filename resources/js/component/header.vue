<script setup>
import {ref} from "vue";

</script>
<template>
    <header>
        <p>ほしい物リスト</p>
        <div class="serch_wrap">
            <input type="text">
        </div>
    </header>
</template>
<style lang="scss">
header{
    background: rgba(0, 0, 0, 0.3);
    height: 60px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;

    p{
        color: #fff;
        margin: 0;
    }

    .serch_wrap{

        input{
            font-size: 16px;
            color: #fff;
            background: rgba(255, 255, 255, 0.2);
            border: none;
            box-shadow: none;
            border-radius: 4px;
            padding: 6px 12px;
        }
    }
}
</style>
