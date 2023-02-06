<template>
  <div class="wrapper">
  <div class="slide-card">
    <div class="slide-card-header">
      <h1>Post an unflitered version to work towards your reward!</h1>
    </div>
    <div class="slide-card-img">

    </div>

    <div class="container">
      <!-- <div class="card">
      <div>
        <h2>{{ countdown }} days until <span class="highlight">Reward day!</span></h2>
      </div>
      <div>
        <p>
          Your chosen reward this month...
          <span class="reward">20% off at Pizza Express</span>
        </p>
      </div>
      <div v-if="threshhold" class="slide-card-buttons">
          <button class="btn btn-positive">See Tips on Posting</button>
          <button class="btn btn-positive">Learn more about the affects</button>
      </div>
    </div> -->

    <!--<div class="progress-card card">
       <div class="progress-card-information">
         <div class="progress-card-description">
          <div v-html="progressDescription"></div>
        </div>
        <CircleProgress 
          :percent="selfieStats.percentage" 
          :viewport="true" 
          :show-percent="true"
          border-width="8"
          border-bg-width="5"
          :fill-color="fillColor"
          size="118"
          class="selfie-progress"
        />
      </div>
      <div v-if="threshhold" class="slide-card-buttons">
          <button class="btn btn-positive">See Tips on Posting</button>
          <button class="btn btn-positive">Learn more about the affects</button>
      </div> 
    </div>-->
    <!-- <div class="card">
       <div id="app">
        <CalendarMonth/>
      </div>
      <div class="insights">
        <h4>Insights</h4>
        <p>It looks like your down on your unfiltered selfie ratio this month. Has anything different happened this month?</p>
        <p>You also seem to be posting filtered selfies on the <strong>weekend</strong>. What might be different then to the rest of the week? </p>
    </div>

    </div> -->
      <div class="slide-card-description">
        <h2>20% off at Pizza Express</h2>
        <p>You're on target with <span class="percentage">{{ selfieStats.percentage }}%</span> unflitered selfies!<br/> Keep it up and claim your reward in <span>4</span> days</p>
      </div>

      <div class="slide-card-progress-cards">
        <ProgressBar 
          sequence-num="progress-1" 
          title="Current average" 
          :threshold-met="selfieStats.thresholdMet"
          :width="selfieStats.percentage"
        />
        <ProgressBar 
          sequence-num="progress-2"
          title="Average after posting this selfie filtered" 
          :threshold-met="selfieStats.thresholdMetPostFiltered"
          :width="selfieStats.postFilteredPercentage"
        />
        <ProgressBar 
          sequence-num="progress-3"
          title="Average after post this selfie unfiltered" 
          :threshold-met="selfieStats.thresholdMetPostUnFiltered"
          :width="selfieStats.postUnFilteredPercentage"
        />
        <div class="threshold-line" :style="`left:${threshhold}%;`"></div>
      </div>

      <div class="slide-card-CTA">
        <button class="btn btn-negative">Post</button>
        <button class="btn btn-positive">Post Unfiltered</button>
      </div>
    </div>

  </div>
  </div>
</template>

<script lang="ts" setup>

import { ref, computed } from 'vue';
import CalendarMonth from "./components/CalendarMonth";
import ProgressBar from "./components/ProgressBar";
import "vue3-circle-progress/dist/circle-progress.css";
import CircleProgress from "vue3-circle-progress";


const props = defineProps({
  threshhold: {
    default: 76,
    required: true,
    type: Number
  },
  selfies: {
    default: {
      filtered: 4,
      unfiltered: 14
    },
    required: true,
    type: Object
  }
});

useHead ({
  bodyAttrs: {
    style: 'background: black; margin:0;'
  },
});

const selfieStats = computed(() => {
  const totalSelies = props.selfies.filtered + props.selfies.unfiltered;
  const percentage = Math.floor((100/totalSelies) * props.selfies.unfiltered)
  const thresholdMet = percentage >= props.threshhold;
  const postFilteredPercentage = Math.floor((100/(totalSelies + 1)) * props.selfies.unfiltered);
  const thresholdMetPostFiltered = postFilteredPercentage >= props.threshhold;
  const postUnFilteredPercentage = Math.floor((100/(totalSelies + 1)) * (props.selfies.unfiltered + 1));
  const thresholdMetPostUnFiltered = postUnFilteredPercentage >= props.threshhold;
  return {
    percentage: percentage,
    totalSelies: totalSelies,
    thresholdMet: thresholdMet,
    postFilteredPercentage: postFilteredPercentage,
    thresholdMetPostFiltered: thresholdMetPostFiltered,
    postUnFilteredPercentage: postUnFilteredPercentage,
    thresholdMetPostUnFiltered: thresholdMetPostUnFiltered
  } 
});

const fillColor = computed(() => {
  const totalSelies = props.selfies.filtered + props.selfies.unfiltered;
  const percentage = Math.floor((100/totalSelies) * props.selfies.unfiltered)
  const thresholdMet = percentage >= props.threshhold;
  return thresholdMet ? '#FE0B67' : '#830071';
});

const progressDescription = computed(() => {
  const totalSelies = props.selfies.filtered + props.selfies.unfiltered;
  const percentage = Math.floor((100/totalSelies) * props.selfies.unfiltered)
  const thresholdMet = percentage >= props.threshhold;
  if(thresholdMet) {
    return (
      `<h4>Looking Good</h4>
       <p>You're beating your monthly target of over <strong>${ props.threshhold }%</strong> unfiltered selfies!</p>
      `
    )
  } else {
    return (
      `<h4>Need a bit of help?</h4>
       <p>You're below your monthly target of <strong>${ props.threshhold }%</strong> unfiltered selfies. See your monthly breakdown for insights on your posting behaviour.</p>
      `
    )
  }
});

const countdown = computed (() => {
  const today = new Date();
  const lastDayOfMonth = new Date(today.getFullYear(), today.getMonth()+1, 0);
  const Difference_In_Time = lastDayOfMonth.getTime() - today.getTime();
  return  Math.ceil(Difference_In_Time / (1000 * 3600 * 24));
});


</script>

<style lang="scss">

.wrapper {
  margin: 0 auto;
  max-width: 390px;
  position: relative;
  width: 100%;
  height: 100vh;

}
.slide-card {
  height: calc(100vh - 7.5rem);
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  background: #fff;
  border-radius: 0.625rem 0.625rem 0 0;
  display: flex;
  flex-direction: column;
  text-align: center;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #444;
  line-height: 1.5;
  font-size: 14px;

  h1, h2 {
    font-size: 20px;
  }

  .percentage {
    font-size: 20px;
    font-weight: bold;
    color: #FE0B67;
  }
}

.slide-card-progress-cards {
  position: relative;
  display: flex;
  flex-direction: column;
  margin-top: 30px;
  .threshold-line {
    position: absolute;
    top: 0;
    bottom: 0;
    z-index: -1;
    width: 1px;
    border-left: 1px dashed #999;

    &:before{
      content: url('assets/crown.svg');
      position: absolute;
      display: block;
      width: 30px;
      height: 15px;
      top: -36px;
      left: -15px;
      opacity: 0;
      animation-name: lower;
      animation-duration: 0.75s;
      animation-delay: 0.5s;
      animation-fill-mode: forwards;
      animation-timing-function: ease-out;
    }
  }
}

@keyframes lower {
  100% { opacity: 1; top: -16px; }
}

@keyframes rise {
  100% { opacity: 1; bottom: -35px; }
}

.container {
  padding: 0 20px;
}

.slide-card-progress-card {
  margin-bottom: 30px;
  h3 {
    font-size: 12px;
    text-align: left;
    font-weight: normal;
  }
  &:first-of-type h3{
    margin-top: 0;
  }
}
// .progress-bar {
//   height: 5px;
//   background: #ccc;
//   border-radius: 2.5px;
//   overflow: visible;
//   position: relative;

//   &:before {
//     content: '0';
//     bottom: -22px;
//     left: 0;
//     font-size: 12px;
//     font-weight: bold;
//     display: block;
//     position: absolute;
//   }

//   &:after {
//     content: '100';
//     bottom: -22px;
//     left: calc(100% - 20px);
//     font-size: 12px;
//     font-weight: bold;
//     display: block;
//     position: absolute;
//   }
// }
// .progress {
//   height: 100%;
//   width: 0;
//   border-radius: 4px;
//   background: #830071;
//   animation-name: grow;
//   animation-duration: 1s;
//   animation-fill-mode: forwards;
//   position: relative;
//   overflow: visible;
//   &:after {
//     content: ' ' attr(data-count)  '%';
//     position: absolute;
//     color: #fff;
//     font-size: 11px;
//     line-height: 35px;
//     text-align: center;
//     right: 100%;
//     opacity: 0;
//     bottom: -55px;
//     left: 100%;
//     transform: translateX(-50%);
//     width: 35px;
//     height: 35px;
//     border-radius: 50%;
//     background: #830071;
//     animation-name: rise;
//     animation-duration: 1s;
//     animation-fill-mode: forwards;
//     animation-timing-function: ease-out;
//   }
// }
// .progress-1 .progress {  
//   animation-delay: .5s; 
//   &:after {
//     display: none;
//   }
// }
// .progress-2 .progress {  animation-delay: 1.5s; }
// .progress-2 .progress:after {  animation-delay: 2s; }
// .progress-3 .progress {  animation-delay: 2.5s; }
// .progress-3 .progress:after {  animation-delay: 3s; }

// @keyframes grow {
//   0%   { width: 0%; }
//   90%  { width: calc(100% + 20px); }
//   100% { width: 100%; }
// }
 
// .progress-container {
//   height: 8px;
//   left: 0;
//   top: calc(50% - 4px);
//   position: absolute;
// }

// .threshold-met {
//   .progress, .progress:after {
//     background: #FE0B67;
//   }
// }

.slide-card-CTA {
  margin-top: 30px;
  display: flex;
  flex-direction: row;
  gap: 10px;
}

.slide-card-buttons {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}


.btn {
  color: #fff;
  font-weight: bold;
  font-size: 15px;
  line-height: 45px;
  border-radius: 5px;
  border:0;
  flex: 1;

  &-negative {
    background: #830071;
  }

  &-positive {
    background: #FE0B67;
  }
}

.current-counter {
    font-size: 26px;
    font-weight: bold;
    transform: translate(40px, 40px);

    &:after {
      content: '%';
    }
  }

  .card {
    margin: 10px 0px;
    padding: 20px 30px;
    border: 1px solid #ededed;
    background: #fff;
    box-shadow: 0px 1px 12px rgba(0,0,0,0.15);
    border-radius: 10px;
    display: flex;
    flex-direction: column;
  }

  .progress-card-information {
    display: flex;
    flex-direction: row;
    text-align: left;
    gap: 10px;
    align-items: flex-start;
    .selfie-progress {
      flex: 0 0 118px;
    }
    .progress-card-description > div {
      h4 {
        font-size: 20px;
        margin:0;
      }
    }
  }
  .insights {
    border-radius: 10px;
    padding: 15px;
    background-color: #FECF00;
    margin: 10px -15px -5px;
    h4 {
        font-size: 20px;
        margin:0;
        display: flex;
        align-items: flex-end;
        justify-content: center;
        transform: translate(-10px, -4px);
        gap: 5px;
        &:before {
          content:  url('assets/insight.svg');
          height: 40px;
          display: inline-block;
          width: 40px;
          transform: translateY(-2px);
        }
        
      }
      strong {
        color: #830071;
      }
  }
  .highlight {
    color: #FE0B67;
  }
  .reward {
    display: block;
    font-size: 20px;
    font-weight: bold;
  }

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

li {
  padding: 0;
  margin: 0;
  list-style: none;
}

}
</style>
