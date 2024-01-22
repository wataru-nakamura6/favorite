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

    for (var item of filteredList.value) {
        filteredValue.value += item.val
    }
})

// 画像アニメーション
// 何番目の画像クリックか？
let num = ref();
let isActive = (i) => {
    if (num.value === i) num.value = null;
    else num.value = i;
};

const filter = (e) => {
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
    for (var item of filteredList.value) {
        filteredValue.value += item.val
    }
}

</script>
<template>
    <header>
        <div class="search_wrap">
            <input type="text" @change="filter">
        </div>
        <button>
            <img src="/images/add.svg" alt="">
        </button>
    </header>
    <div class="timer_wrap">
        <h3>合計金額 : {{ filteredValue.toLocaleString() }}円</h3>
        <ul>
            <li @click="isActive(i)" v-for="(list, i) in filteredList">
                <div class="text_wrap">
                    <div class="category_wrap">
                        {{ list.category }}
                    </div>
                    <div class="flex">
                        <p>{{ list.type }}</p>
                        <p>{{ list.val.toLocaleString() }}円</p>
                    </div>
                </div>
                <div class="arrow_wrap" :class="{active: num == i}">
                    <div class="img_wrap">
                        <img :src="list.image" alt="">
                    </div>
                </div>
                <div class="remark_wrap">
                    <p>{{ list.sup }}</p>
                </div>
                <div class="icon_wrap">
                    <button>
                        <img src="/images/check.svg" alt="">
                    </button>
                    <button>
                        <img src="/images/pen.svg" alt="">
                    </button>
                </div>
            </li>
        </ul>
    </div>
</template>
<style lang="scss">
$x-margin: 20px;
$y-margin: 16px;
$ys-margin: 8px;
$header-h: 60px;
$header-bg: #fff;
$accent: #7bcccf;
$background: #f4fdff;
$box-shadow: #222226;
$color: #000;

body *:not(html):not(style):not(br):not(tr):not(code) {
    font-family: 'Zen Maru Gothic', serif;

    header {
        background: $accent;
        height: $header-h;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 $x-margin;
        border-radius: 8px;
        width: calc(100% - 24px);
        position: fixed;
        top: 8px;
        left: 12px;
        z-index: 100;

        button {
            background: none;
            box-shadow: none;
            border: none;
            width: 24px;
            height: 24px;
            padding: 0;
        }

        p {
            color: $color;
            margin: 0;
        }

        .search_wrap {
            input {
                font-size: 16px;
                color: $color;
                background: $background;
                border: none;
                box-shadow: none;
                border-radius: 4px;
                padding: 6px 12px;
            }
        }
    }

    p {
        margin-bottom: 0;
        font-size: 14px;
    }

    .timer_wrap {
        padding: $y-margin $x-margin $y-margin;
        color: $color;
        min-height: calc(100vh - #{$header-h});

        h3 {
            font-weight: normal;
            margin-bottom: 0;
            font-size: 20px;
            padding: 4px 0;
        }

        ul {
            list-style: none;
            padding: 0;
            margin: 0;

            li {
                border-bottom: 3px solid $accent;
                //padding-bottom: $y-margin;
                padding-top: $y-margin;
                position: relative;

                &:last-child {
                    border-bottom: none;
                    margin-bottom: 0;
                    padding-bottom: 0;
                }

                .arrow_wrap {
                    position: relative;

                    &::after {
                        content: '';
                        position: absolute;
                        top: 50%;
                        left: 100%;
                        transform: translate(-32px, -50%);
                        width: 0;
                        height: 0;
                        border-style: solid;
                        border-right: 12px solid transparent;
                        border-left: 12px solid transparent;
                        border-top: 20px solid #fff;
                        border-bottom: 0;
                        z-index: 2;
                        transition: all 0.6s;
                    }

                    &::before {
                        content: '';
                        position: absolute;
                        top: 50%;
                        left: 100%;
                        transform: translate(-100%, -50%);
                        width: 40px;
                        height: 40px;
                        z-index: 1;
                        background: $accent;
                        border-radius: 6px;
                        transition: all 0.6s;
                    }

                    .img_wrap {
                        max-height: 40px;
                        opacity: 0.6;
                        overflow: hidden;
                        transition: all 0.6s;
                        background: #000;
                        border-radius: 6px;

                        &:hover {
                            opacity: 1;
                        }

                        img {
                            height: 100%;
                            width: 100%;
                            object-fit: cover;
                        }
                    }

                    &.active {
                        .img_wrap {
                            max-height: 100vh;
                            opacity: 1;
                            transition: all 0.6s;
                        }

                        &::after {
                            top: 100%;
                            transform: translate(-32px, -32px);
                            border-right: 12px solid transparent;
                            border-left: 12px solid transparent;
                            border-bottom: 20px solid #fff;
                            border-top: 0;
                            transition: all 0.6s;
                        }

                        &::before {
                            top: 100%;
                            transform: translate(-100%, -100%);
                            transition: all 0.6s;
                        }
                    }
                }

                .text_wrap {
                    display: flex;
                    align-items: center;
                    margin-bottom: $y-margin;

                    .category_wrap {
                        font-family: 'Zen Maru Gothic', serif;
                        font-size: 20px;
                        background: #fff;
                        padding: 3px 0 4px;
                        width: 80px;
                        border-radius: 6px;
                        border: 3px solid $accent;
                        text-align: center;
                        font-weight: 500;
                    }

                    .flex {
                        display: flex;
                        align-items: flex-end;
                        justify-content: space-between;
                        flex: 1;
                        margin-left: 16px;

                        p {
                            font-size: 18px;
                            font-weight: 500;

                            &:last-child {
                                font-size: 14px;
                            }
                        }
                    }
                }

                .remark_wrap {
                    margin-top: $y-margin;

                    p {
                        font-size: 14px;
                    }
                }

                .icon_wrap {
                    text-align: right;

                    button {
                        background: none;
                        box-shadow: none;
                        border: none;
                        width: 24px;
                        height: 24px;
                        padding: 0;
                        margin-left: 4px;

                        img {
                            width: 16px;
                            margin: auto;
                            opacity: 0.3;
                        }
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
}

#app {
    background: $background;
    max-width: 450px;
    margin: auto;
    box-shadow: 0 0 8px $box-shadow;
    padding-top: 68px;
}
</style>
