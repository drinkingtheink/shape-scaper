<template>
    <div class="shaper" v-if="circles.length && triangles.length">
        <transition-group name="fade" >
            <div v-for="triangle, index in triangles" :key="`triangle-${index}-wrapper`" class="triangle-wrapper" :style="getTriStyles()" :class="{ 'drift': Math.random() < 0.5 }">
                <Triangle class='actual-shape' :fill="getTriStyles.fill" :key="`triangle-${index}`" />
            </div>
        </transition-group>

        <transition-group name="fade" >
            <div v-for="circle, index in circles" :key="`circle-${index}-wrapper`"  class="circle-wrapper drif" :style="getCircleStyles()" :class="{ 'drift2': Math.random() < 0.5 }">
                <Circle class='actual-shape' :fill="getCircleStyles.fill" :key="`circle-${index}`" />
            </div>
        </transition-group>
            
        <transition-group name="fade">
            <div v-for="rect, index in rects" :key="`rect-${index}-wrapper`"  class="rect-wrapper drif" :style="getRectStyles()" :class="{ 'drift': Math.random() < 0.5 }" />
        </transition-group>

        <button class="redraw-shapes" @click="shapeUp()">Redraw</button>
        <section class="timing-bar-wrapper">
            <div class="timing-bar"></div>
        </section>
    </div>
</template>

<script>
import Circle from './shapes/Circle.vue';
import Triangle from './shapes/Triangle.vue';

const colorSet = ['#54478c', '#2c699a', '#0db39e', '#83e377', '#f29e4c'];

const aniLength = 10000;

export default {
    name: 'Shaper',
    data() {
        return {
            circles: [],
            triangles: [],
            rects: [],
            circlesCount: 0,
            trianglesCount: 0,
            rectsCount: 0,
            createShapes: false,
            interval: null,
        }
    },
    components: {
        Circle,
        Triangle,
    },
    mounted() {
        this.shapeUp();

        this.interval = setInterval(() => {
            this.shapeUp();
        }, aniLength);
    },
    watch: {
        createShapes() {
            if (this.createShapes === true) {
                this.shapeUp();

                this.createShapes = false;
            }
        }
    },
    methods: {
        getColorClass() {
            const colors = ['white', 'PaleGreen', colorSet[0], colorSet[2], colorSet[1]];

            return colors[Math.floor(Math.random()*colors.length)];
        },
        getDriftClass() {
            const drifts = ['drift1', 'drift2', 'drift3'];

            return drifts[Math.floor(Math.random()*drifts.length)];
        },
        getStyles() {
            return {
                'width': `${this.generateRandom(100, 300)}px`,
                'fill': `${this.getColorClass}`
            }
        },
        generateRandom(min = 0, max = 100) {
            // find diff
            let difference = max - min;

            // generate random number 
            let rand = Math.random();

            // multiply with difference 
            rand = Math.floor( rand * difference);

            // add with min value 
            rand = rand + min;

            return rand;
        },
        getCircleStyles() {
            return {
                'width': `${this.generateRandom(60, 200)}px`,
                'fill': `${this.getColorClass()}`,
                'left': `${this.generateRandom(8, 80)}%`,
                'bottom': `${this.generateRandom(30, 92)}%`,
                'opacity': `0.${this.generateRandom(5, 9)}`,
                'animationDelay': `0.${this.generateRandom(0, 9)}s`,
                'filter': `blur(${this.generateRandom(0, 12)}px`,
            }
        },
        getTriStyles() {
            return {
                'width': `${this.generateRandom(10, 220)}px`,
                'fill': `${this.getColorClass()}`,
                'left': `${this.generateRandom(8, 80)}%`,
                'bottom': `${this.generateRandom(40, 110)}%`,
                'opacity': `0.${this.generateRandom(4, 10)}`,
                'animationDelay': `0.${this.generateRandom(0, 9)}s`,
                'filter': `blur(${this.generateRandom(0, 12)}px`,
            }
        },
        getRectStyles() {
            return {
                'height': `${this.generateRandom(100, 220)}px`,
                'width': `${this.generateRandom(6, 10)}px`,
                'backgroundColor': `${this.getColorClass()}`,
                'left': `${this.generateRandom(8, 80)}%`,
                'bottom': `${this.generateRandom(10, 80)}%`,
                'animationDelay': `0.${this.generateRandom(0, 9)}s`,
                'transform': `rotate(${this.generateRandom(0, 20)}deg)`,
                'filter': `blur(${this.generateRandom(4, 12)}px`,
            }
        },
        shapeUp() {
            this.circles = [];
            this.circlesCount = 0;
            this.triangles = [];
            this.trianglesCount = 0;
            this.rects = [];
            this.rectsCount = 0;

            this.circlesCount = this.generateRandom(5, 8);

            for (let i = 0; i < this.circlesCount; i++) {
                this.circles.push(`circle-${i}`);
            }

            this.trianglesCount = this.generateRandom(4, 10);

            for (let i = 0; i < this.trianglesCount; i++) {
                this.triangles.push(`triangle-${i}`);
            }

            this.rectsCount = this.generateRandom(0, 3);

            for (let i = 0; i < this.rectsCount; i++) {
                this.rects.push(`rect-${i}`);
            }
        }
    },
}
</script>

<style lang="scss">
@keyframes spread {
    from {
        width: 0;
    }
    to {
        width: 80%;
    }
}

@keyframes spin {
  from {
    transform:rotate(0deg);
  }
  to {
    transform:rotate(360deg);
  }

  99% {
      filter: blur(20px);
  }
}

@keyframes drift {
  from {
    transform: translateX(100px);
    transform: translateY(10px);
  }

  to {
    transform: translateX(-100px);
    transform: translateY(30px);
  }

  99% {
      filter: blur(20px);
  }
}

@keyframes appear {
    from {
        transform: translateY(20px);
        opacity: 0;
    }

    to {
        transform: translateY(0);
        opacity: 1;
    }
}

.shaper {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 100%;
    z-index: 0;

    * {
        position: absolute;
    }

    button {
        position: absolute;
        right: 4rem;
        font-size: 70%;
    }

    .timing-bar-wrapper {
        width: 60px;
        padding: 4px;
        background-color: black;
        position: absolute;
        right: 4rem;
        top: 2rem;
        border-radius: 10px;

        .timing-bar {
            background-color: lightsalmon;
            height: 1px;
            border-radius: 10px;
            margin-top: -1px;
            animation: spread 10s linear 0.4s infinite forwards;
            transition: all 0.2s;
        }
    }
}

.circle, .triangle, rect {
    position: absolute;
}

.drift {
    animation-name: spin;
    animation-duration: 30s;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
    animation-direction: alternate;
}

.actual-shape {
    animation: appear;
    animation-duration: 1s;
    animation-iteration-count: initial;
    animation-timing-function: reverse;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>