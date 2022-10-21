<template>
    <div class="slide" v-show="current_index !== undefined">
        <div class="content" :key="current_index">
            <h3>{{ json[current_index].role }}</h3>
            <h1>{{ json[current_index].name }}</h1>
            <p>{{ json[current_index].bio }}</p>
        </div>
        <div class="slider-menu">
            <ul>
                <li v-for="(item, index) of json" :key="index">
                    <button 
                        :aria-current="index === current_index"
                        v-on:click="setCurrentIndex(index)"
                    />
                </li>
            </ul>
        </div>
        <div class="image" :key="current_index">
            <img :src="json[current_index].images.webp">
        </div>

        <Teleport to="head" v-if="json">
            <link v-for="(element, index) in json" :key="index" :href="element.images.webp" rel="preload" as="image">
        </Teleport>
    </div>
</template>

<script>
    export default {
        props: {
            json: {
                type: Object,
                required: true,
            }
        },
        data() {
            return {
                current_index: undefined,
            }
        },
        created() {
            if (this.json && this.current_index === undefined) {
                this.current_index = 0;
            }
        },
        methods: {
            setCurrentIndex(index) {
                this.current_index = index;
            },
        }
    }
</script>

<style lang="scss" scoped>
    @import "../styles/global.scss";

    .slide {
        @include flex;

        position: relative;

        @media (max-width: 60rem) {
            display: block;

            text-align: center;
        }

        @media (max-width: 35rem) {
            display: flex;
            flex-direction: column;
            order: 3;
        }
    }

    .content {
        @include fade-in-animation(1s);

        margin-top: 10rem;
        
        width: 50%;

        @media (max-width: 60rem) {
            width: auto;

            margin-top: 4rem;
        }

        @media (max-width: 35rem) {
            order: 3;

            margin-top: 0;
        }
    }

    .image {
        @include flex;

        @include fade-in-animation(1s);

        align-items: flex-end;

        height: calc(100vh - 14rem);
        width: 50%;

        position: relative;

        img {
            max-height: calc(100vh - 14rem);
            max-width: 100%;

            @media (max-width: 60rem) {
                margin-inline: auto;
                height: auto;
            }

            @media (max-width: 35rem) {
                height: 35vh;
            }
        }

        @media (max-width: 60rem) {
            width: auto;
            height: auto;
        }

        @media (max-width: 35rem) {
            order: 1;
            border-bottom: solid 1px rgba($light-color, 0.2);

            margin-block-start: 2rem;
        }
    }

    h1 {
        @include heading-3;

        margin-block-start: 1.2rem;
        margin-block-end: 2rem;

        @media (max-width: 60rem) {
            font-size: 40px;

            margin-block-start: 0.75rem;
            margin-block-end: 1.5rem;
        }
    }

    h3 {
        @include heading-4;

        opacity: 0.6;

        @media (max-width: 60rem) {
            font-size: 24px;
        }
    }

    p {
        @include text;

        max-width: 45ch;

        @media (max-width: 60rem) {
            margin-inline: auto;

            font-size: 16px;

            max-width: 50ch;
        }
    }

    .slider-menu {
        position: absolute;
        bottom: 6rem;

        @media (max-width: 60rem) {
            position: relative;
            bottom: 0;

            margin-block-start: 3rem;
            margin-block-end: 2.5rem;
        }

        @media (max-width: 35rem) {
            order: 2;

            margin-block-start: 2rem;
            margin-block-end: 2rem;
        }

        ul {
            @include flex;

            list-style: none;

            @media (max-width: 60rem) {
                justify-content: center;
            }
        }

        button {
            --circle-width: 15px;
            width: var(--circle-width);
            aspect-ratio: 1;
            background-color: $light-color;
            border-radius: calc(var(--circle-width) / 2);
            border: none;
            opacity: 0.4;

            cursor: pointer;

            transition: opacity 150ms ease-out;

            @media (max-width: 60rem) {
                --circle-width: 10px;
            }
        }

        button[aria-current="true"] {
            opacity: 1;
        }

        button:hover,
        button:focus-visible {
            opacity: 0.7;
        }

        button:focus-visible {
            outline: none;
        }
    }
</style>