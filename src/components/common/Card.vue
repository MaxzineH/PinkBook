<script setup>
import { ref, defineProps, onMounted, getCurrentInstance } from 'vue'

const props = defineProps(['cardInfo'])
const emits = defineEmits(['addImageCount'])
function imgLoad(item) {
    emits('addImageCount')
}
</script>

<template>
    <div class="card-content">
        <!-- <slot></slot> -->
        <img class="card-source" :src="props.cardInfo.source" @load="imgLoad(props.cardInfo)" />
        <div class="card-title">{{ props.cardInfo.title }}</div>
        <div class="card-bottom">
            <div class="card-b-left">
                <img class="card-head" :src="props.cardInfo.head" />
                <div class="card-name">{{ props.cardInfo.name }}</div>
            </div>
            <div class="card-b-right">
                <i class="iconfont icon-aixin1 icon-card-small"></i>
                <p class="card-like">{{ props.cardInfo.like }}</p>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.card-content {
    position: absolute;
    top: 0;
    left: 0;
    width: 50%;
    transition: 0.3s ease;
    background-color: rgba(254, 254, 254);
    border-radius: 8px;
    box-shadow: 0px 2px 5px rgba(180, 180, 180, 0.1);

    .card-source {
        width: 100%;
        height: auto;
    }

    .card-title {
        width: calc(100% - 1rem);
        font-size: 0.9rem;
        color: rgba(51, 51, 51);
        padding: 0 0.5rem;
        line-height: 1.3rem;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        overflow: hidden
    }

    .card-bottom {
        padding: 0.8rem 0.5rem;
        @include flexlr;

        .card-b-left {
            @include flexl;

            .card-head {
                width: 1.2rem;
                height: 1.2rem;
                border-radius: 50%;
            }

            .card-name {
                color: rgba(153, 153, 153);
                font-size: 0.5rem;
                margin: 0 0 0 7px;
            }
        }

        .card-b-right {
            @include flexl;

            .icon-card-small {
                font-size: 0.5rem;
                color: rgba(153, 153, 153);
            }

            .card-like {
                margin: 0 0 0 7px;
                color: rgba(153, 153, 153);
                font-size: 0.5rem;
            }
        }
    }

}
</style>
