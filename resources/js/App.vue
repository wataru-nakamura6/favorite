<script setup>
import {ref, onMounted} from "vue";
import Header from "./component/header.vue"
import moment from 'moment'

const lists = [
    {
        'type': 'TV',
        'sup': 'ヨドバシカメラ',
        'val': 115000,
        'category': '家電',
        'image': '/images/1000002142.jpg',
    },
    {
        'type': 'オーブンレンジ',
        'sup': 'ヨドバシカメラ',
        'val': 58080,
        'category': '家電',
        'image': '/images/1000002144.jpg',
    },
    {
        'type': 'オーブンレンジ',
        'sup': 'ヨドバシカメラ',
        'val': 63120,
        'category': '家電',
        'image': '/images/1000002145.jpg',
    },
    {
        'type': '冷蔵庫',
        'sup': 'ヨドバシカメラ',
        'val': 179080,
        'category': '家電',
        'image': '/images/1000002147.jpg',
    },
    {
        'type': '洗濯機',
        'sup': 'ヨドバシカメラ',
        'val': 89610,
        'category': '家電',
        'image': '/images/1000002148.jpg',
    },
    {
        'type': '電動自転車',
        'sup': 'ヨドバシカメラ',
        'val': 100800,
        'category': '自転車',
        'image': '/images/1000002150.jpg',
    },
]

const filteredList = ref([]);
const filteredValue = ref(0);

onMounted(() => {
    filteredList.value = lists

    for (var item of filteredList.value){
        filteredValue.value += item.val
    }
})

// 画像アニメーション
// 何番目の画像クリックか？
let num = ref();
let isActive = (i) => {
    if(num.value === i) num.value = null;
    else num.value = i;
};

const filter = (e) =>
{
    // 全体検索
    filteredList.value = lists.filter(item => {
        // オブジェクトの全ての値に対して検索
        return Object.values(item).some(value => {
            if (typeof value === 'string') {
                return value.includes(e.target.value);
            }
            return false;
        });
    });

    // 合計金額
    filteredValue.value = 0
    for (var item of filteredList.value){
        filteredValue.value += item.val
    }
}

</script>
<template>
    <header>
        <p>ほしい物リスト</p>
        <div class="search_wrap">
            <input type="text" @change="filter">
        </div>
    </header>
    <div class="timer_wrap">
        <h3>合計金額 : {{ filteredValue.toLocaleString() }}円</h3>
        <ul>
            <li @click="isActive(i)" v-for="(list, i) in filteredList">
                <div class="text_wrap">
                    <p>{{ list.type }}</p>
                    <p>{{ list.val.toLocaleString() }}円</p>
                </div>
                <div class="img_wrap" :class="{active: num == i}">
                    <img :src="list.image" alt="">
                </div>
                <div class="remark_wrap">
                    <p>店舗 : {{ list.sup }}</p>
                    <p>カテゴリー : {{ list.category }}</p>
                </div>
            </li>
        </ul>
    </div>
</template>
<style lang="scss">
$x-margin: 20px;
$y-margin: 20px;
$header-h: 60px;
$background: #2e323b;
$box-shadow: #222226;

header{
    background: rgba(0, 0, 0, 0.3);
    height: $header-h;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 $x-margin;

    p{
        color: #fff;
        margin: 0;
    }

    .search_wrap{
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
body{
    background: $background;
}
#app{
    background: $background;
    max-width: 450px;
    margin: auto;
    box-shadow: 0 0 8px $box-shadow;
}

p {
    margin-bottom: 0;
    font-size: 14px;
}
.timer_wrap{
    padding: $y-margin $x-margin $y-margin;
    color: #fff;
    min-height: calc(100vh - #{$header-h});

    h3{
        font-weight: normal;
    }

    ul{
        list-style: none;
        padding: 0;
        margin: 0;

        li{
            border-bottom: 1px solid #fff;
            padding-bottom: $y-margin;
            margin-bottom: $y-margin;

            &:last-child{
                border-bottom: none;
                margin-bottom: 0;
                padding-bottom: 0;
            }

            .img_wrap{
                max-height: 40px;
                opacity: 0.3;
                overflow: hidden;
                transition: all 0.6s;
                position: relative;

                &.active{
                    max-height: 100vh;
                    opacity: 1;
                    transition: all 0.6s;
                }

                &:hover{
                    opacity: 1;
                }

                img{
                    height: 100%;
                    width: 100%;
                    object-fit: cover;
                }
            }
            .text_wrap{
                display: flex;
                align-items: center;
                justify-content: space-between;
                margin-bottom: 12px;
            }

            .remark_wrap {
                margin-top: 12px;

                p {
                    font-size: 12px;
                }
            }
        }
    }
}
h1 {
    font-weight: bold;

    span {
        font-weight: normal;
        font-size: 20px;
    }
}
</style>
