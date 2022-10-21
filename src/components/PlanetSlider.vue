<template>
    <div class="slide" v-show="current_planet !== undefined">
        <div class="image" :key="current_planet">
            <img :src="current_planet.images.webp">
        </div>
        <div class="content">
            <div class="slider-menu">
                <ul>
                    <li v-for="(name, index) of planet_names" :key="index">
                        <button 
                            :aria-current="name === current_planet_name"
                            v-on:click="setCurrentPlanetName(name)"
                            v-html="name"
                        />
                    </li>
                </ul>
            </div>
            <!-- Start of Slide -->
            <div class="wrapper" :key="current_planet">
                <h1>{{ current_planet.name }}</h1>
                <p>{{ current_planet.description }}</p>
                <hr class="divider">
                <div class="stats">
                    <div>
                        <h3>Avg. distance</h3>
                        <p>{{ current_planet.distance }}</p>
                    </div>
                    <div>
                        <h3>Est. travel time</h3>
                        <p>{{ current_planet.travel }}</p>
                    </div>
                </div>
            </div>
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
        computed: {
            planet_names() {
                if (this.json) {
                    const planets = this.json.map(planet => planet.name);
                    return planets;
                } else {
                    return undefined;
                }
            },
            current_planet() {
                if (this.current_planet_name) {
                    return this.json.find(({ name }) => name === this.current_planet_name);
                } else {
                    return undefined;
                }
            }
        },
        data() {
            return {
                current_planet_name: undefined,
            }
        },
        created() {
            if (this.planet_names && this.current_planet_name === undefined) {
                this.current_planet_name = this.planet_names[0];
            }
        },
        methods: {
            setCurrentPlanetName(name) {
                this.current_planet_name = name;
            }
        }
    }
</script>

<style lang="scss" scoped>
	@import "../styles/global.scss";

	.slide {
		@include flex;

		margin-block-start: 8rem;

		justify-content: center;
		gap: clamp(2rem, 5vw, 12rem);

        .image {
            @include fade-in-animation(1s);

            text-align: right;

            img {
                max-width: 30vw;
            }

            @media (max-width: 60rem) {
                text-align: center;
            }
        }

        .wrapper {
            @include fade-in-animation(1s);
        }

        @media (max-width: 60rem) {
            display: block;

            margin-block-start: 3rem;
        }
	}

	h1 {
		@include heading-2;

		margin-block-end: 1.5rem;

        @media (max-width: 35rem) {
            margin-block-end: 0.5rem;
            font-size: 56px;
        }
	}

	.content {
        @include fade-in-animation(1s);

		position: relative;

		margin-block-start: 3rem;

        @media (max-width: 60rem) {
            text-align: center;
        }

        @media (max-width: 35rem) {
            margin-block-start: 1.5rem;
        }
	}

	.content > .wrapper > p {
		@include text;

		max-width: 45ch;

        @media (max-width: 60rem) {
            margin-inline: auto;
        }

        @media (max-width: 35rem) {
			font-size: min(18px, 4vw);
		}
	}

	.divider {
		opacity: 0.3;

		margin-block-start: 2rem;
		margin-block-end: 1.5rem;

        @media (min-width: 35rem) and (max-width: 60rem) {
            margin-inline: 2rem;
        }

        @media (max-width: 35rem) {
            margin-block-end: 2rem;
        }
	}

	.stats {
		@include flex;

		position: relative;

		div {
			width: 50%;
		}

        @media (max-width: 60rem) {
            margin-inline: 2rem;

            justify-content: space-around;
        }

        @media (max-width: 35rem) {
            display: block;

            div {
                width: auto;
            }

            div + div {
                margin-top: 2rem;
            }
        }
	}

	h3 {
		@include sub-heading-2;

		margin-block-end: 0.8rem;
	}

	.stats p {
		@include sub-heading-1;
	}

    /* slider menu */
    .slider-menu {
		position: absolute;
		top: -5rem;
		left: 0;

		ul {
			@include flex;

			list-style: none;

            @media (max-width: 60rem) {
                justify-content: center;
            }
		}

		button {
			@include nav-text;

			color: $primary-color;
			cursor: pointer;

			border: none;
			background: none;
            padding: 0;

            position: relative;
		}

        @media (max-width: 60rem) {
            position: relative;
            top: 0;

            margin-bottom: 2em;
        }
	}

    /* Decoration */
    button::before {
        content: "";

        position: absolute;
        width: 100%;
        
        border-bottom: solid 3px $light-color;
        padding-bottom: 2em;
        opacity: 0;
        transition: opacity 150ms ease-out;
    }

    button:hover::before,
    button:focus-visible::before {
        opacity: 0.6;
    }

    button:focus-visible {
        outline: none;
    }

    button[aria-current="true"]::before {
        opacity: 1;
    }
</style>