<template>
  <div class="container">
    <users-list></users-list>
  </div>
  <div class="container">
    <div class="block" :class="{animate: animatedBlock}"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <transition 
      :css="false" 
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="cancelEnter"
      @leave-cancelled="cancelLeave">
      <p v-if="paraIsVisible">This is only sometimes visible...</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
  <div class="container">
    <transition name="showButton" mode="out-in">
      <button @click="showButton" v-if="!isButtonVisible">Show</button>
      <button @click="hideButton" v-else>Hide</button>
    </transition>
  </div>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
import UsersList from './components/UsersList.vue'
export default {
  components: {
    UsersList
  },
  data() {
    return {
      animatedBlock: false,
      dialogIsVisible: false,
      paraIsVisible: false,
      isButtonVisible:false,
      enterInterval: null,
      leaveInterval: null
    };
  },
  methods: {
    cancelEnter() {
      clearInterval(this.enterInterval);
    },
    cancelLeave() {
      clearInterval(this.leaveInterval);
    },
    animateBlock() {
      this.animatedBlock = true;
    },
    showButton() {
      this.isButtonVisible = true;
    },
    hideButton() {
      this.isButtonVisible = false;
    },
    toggleParagraph() {
      this.paraIsVisible = !this.paraIsVisible;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    beforeEnter(el) {
      console.log('beforeEnter');
      el.style.opacity = 0;
    },
    enter(el,done) {
      let round = 0;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if(round > 100) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 20)
      console.log('enter');
    },
    afterEnter() {
      console.log('afterEnter')
    },
    beforeLeave() {
      console.log('beforeLeave')
    },
    leave(el,done) {
      let round = 100;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round--;
        if(round < 0) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20)
      console.log('leave')
    },
    afterLeave() {
      console.log('afterLeave')
    }
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s ease-out; */
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
.animate {
  /* transform: translateX(-150px); */
  animation: slide-fade 0.3s ease-out forwards;
}

.para-enter-from {
  /* opacity: 0;
  transform: translateY(-30px); */
}

.para-enter-active {
  /*animation: slide-scale 2s ease-out;*/
}

.para-enter-to {
  /* opacity: 1;
  transform: translateY(0); */
}

.para-leave-from {
  /* opacity: 1;
  transform: translateY(0); */
}

.para-leave-active {
  /* transition: all 0.3s ease-in; */
  /*animation: slide-scale 0.3s ease-out;*/
}

.para-leave-to {
  /* opacity: 0;
  transform: translateY(30px); */
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }

  70% {
    transform: translateX(-120px) scale(1.1);
  }

  100% {
    transform: translateX(-150px) scale(1);
  }
}

.showButton-enter-from,
.showButton-leave-to {
  opacity: 0;
}
.showButton-enter-active {
  transition: opacity 1s ease-out
}

.showButton-leave-active {
  transition: opacity 1s ease-in
}

.showButton-leave-from,
.showButton-enter-to {
  opacity: 1;
}
</style>