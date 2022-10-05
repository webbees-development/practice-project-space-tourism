<template>
    <div class="slide" v-show="current_index !== undefined">
        <div class="left">
            <div class="slider-menu">
                <ul>
                    <li v-for="(item, index) of json" :key="index">
                        <button 
                            :aria-current="index === current_index"
                            v-on:click="setCurrentIndex(index)"
                            v-html="index + 1"
                        />
                    </li>
                </ul>
            </div>
            <div class="content" :key="current_index">
                <h3>The Terminology...</h3>
                <h1>{{ json[current_index].name }}</h1>
                <p>{{ json[current_index].description }}</p>
            </div>
        </div>
        <div class="image" :key="current_index">
            <img class="desktop" :src="json[current_index].images.portrait">
            <img class="mobile" :src="json[current_index].images.landscape">
        </div>
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
            }
        }
    }
</script>

<style lang="scss" scoped>
    @import "../styles/global.scss";

    .slide {
        @include flex;
        gap: 5rem;
        justify-content: space-between;

        margin-block-start: 2rem;

        @media (max-width: 60rem) {
            flex-direction: column;

            text-align: center;

            margin-block-start: 4rem;

            gap: 3.5rem;
        }
    }

    .left {
        @include flex;
        gap: 5rem;

        @media (max-width: 60rem) {
            flex-direction: column;
            gap: 3rem;

            order: 2;            
        }
    }

    .content,
    .slider-menu {
        margin-block-start: 7rem;

        @media (max-width: 60rem) {
            margin-block-start: 0;
        }
    }

    h1 {
        @include heading-3;
        
        margin-block-start: 1rem;
        margin-block-end: 1.5rem;

        @include fade-in-animation(1s);

        @media (max-width: 60rem) {
            font-size: 40px;
        }
    }

    h3 {
        @include nav-text;
        @include color-primary;

        @include fade-in-animation(1s);

        @media (max-width: 60rem) {
            font-size: 16px;
        }
    }

    p {
        @include text;

        max-width: 45ch;

        @include fade-in-animation(1s);

        @media (max-width: 60rem) {
            margin-inline: auto;

            max-width: 50ch;

            font-size: 16px;
        }
    }

    .image {
        @include fade-in-animation(1s);

        @media (max-width: 60rem) {
            order: 1;

            width: 100%;
            aspect-ratio: 2.4;
        }

        img {
            max-width: 30vw;

            @media (max-width: 60rem) {
                position: absolute;
                left: 0.1rem;

                max-width: 100vw;
                width: 100vw;
            }

            @media (max-width: 35rem) {
                left: 0.1rem;
            }
        }
    }

    .slider-menu {
        @include fade-in-animation(1s);

        ul {
            @include flex;

            flex-direction: column;
            gap: 2rem;

            list-style: none;

            @media (max-width: 60rem) {
                flex-direction: row;
                justify-content: center;
            }
        }

        button {
            @include heading-5;
            padding: 0;

            width: 5rem;
            aspect-ratio: 1;
            border-radius: calc(5rem / 2);

            border: solid 1px rgba($light-color, 0.5);
            background: none;
            color: $light-color;

            cursor: pointer;

            transition: all 150ms ease-out;

            /* center number */
            padding-left: 3px;
            padding-bottom: 5px;
        }

        button[aria-current="true"] {
            background-color: $light-color;
            color: $dark-color;
        }

        button:hover,
        button:focus-visible {
            border-width: 2px;
            border-color: $light-color;
        }

        button:focus-visible {
            outline: none;
        }
    }

    /* images */
    .desktop {
        display: none;

        @media (min-width: 60rem) {
            display: block;
        }
    }

    /* mobile = mobile + tablet */
    .mobile {
        display: none;

        @media (max-width: 60rem) {
            display: block;
        }
    }
</style>