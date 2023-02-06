<template>
    <div class="slide-card-progress-card">
        <h3>{{ props.title }}</h3>
        <div class="progress-bar">
            <div 
                class="progress-container"
                :class="processClasses"
                :style="`width:${ props.width }%;`"
            >
                <div 
                    class="progress"
                    data-count="40"
                ></div>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
    import { computed } from 'vue';

    const props = defineProps({
        title: {
            default: "",
            required: true,
            type: String
        },
        sequenceNum: {
            required: true,
            default: "progress-1",
            type: String
        },
        thresholdMet: {
            required: true,
            default: false,
            type: Boolean
        },
        width: {
            default: 0,
            required: true,
            type: Number
        }
    });

    const processClasses = computed(() => {
        return `${ props.thresholdMet ? 'threshold-met' : ''} ${ props.sequenceNum }`;
    });

</script>
<style scoped lang="scss">

.progress-bar {
  height: 5px;
  background: #ccc;
  border-radius: 2.5px;
  overflow: visible;
  position: relative;

  &:before {
    content: '0';
    bottom: -22px;
    left: 0;
    font-size: 12px;
    font-weight: bold;
    display: block;
    position: absolute;
  }

  &:after {
    content: '100';
    bottom: -22px;
    left: calc(100% - 20px);
    font-size: 12px;
    font-weight: bold;
    display: block;
    position: absolute;
  }
}
.progress {
  height: 100%;
  width: 0;
  border-radius: 4px;
  background: #830071;
  animation-name: grow;
  animation-duration: 1s;
  animation-fill-mode: forwards;
  position: relative;
  overflow: visible;
  &:after {
    content: ' ' attr(data-count)  '%';
    position: absolute;
    color: #fff;
    font-size: 11px;
    line-height: 35px;
    text-align: center;
    right: 100%;
    opacity: 0;
    bottom: -55px;
    left: 100%;
    transform: translateX(-50%);
    width: 35px;
    height: 35px;
    border-radius: 50%;
    background: #830071;
    animation-name: rise;
    animation-duration: 1s;
    animation-fill-mode: forwards;
    animation-timing-function: ease-out;
  }
}
.progress-1 .progress {  
  animation-delay: .5s; 
  &:after {
    display: none;
  }
}
.progress-2 .progress { 
    animation-delay: 1.5s; 
    &:after {
        animation-delay: 2s;
    }
}
.progress-3 .progress { 
    animation-delay: 2.5s; 
    &:after {  
        animation-delay: 3s; 
    } 
}

@keyframes grow {
  0%   { width: 0%; }
  90%  { width: calc(100% + 20px); }
  100% { width: 100%; }
}
 
.progress-container {
  height: 8px;
  left: 0;
  top: calc(50% - 4px);
  position: absolute;
}

.threshold-met {
  .progress, .progress:after {
    background: #FE0B67;
  }
}
</style>