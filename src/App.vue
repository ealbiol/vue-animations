<template>
  <div class="container">
    <!--Dynamic Class :class. When animatedBlock is true the animate class will be added. -->
    <div class="block" :class="{ animate: animatedBlock }"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <!--<transition> Component: Vue Built-in Component to control appearance of the appear and removal of an animation.-->
    <transition
      name="para"
      mode="out-in"
      @before-enter="beforeEnter"
      @before-leave="beforeLeave"
      @enter="enter"
      @after-enter="afterEnter"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paraIsVisible">This is sometimes visible....</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
  <div class="container">
    <!--//Exception in which <transtion></transtion> accepts more than one child. It happens because in reality only one is added to the DOM because of v-if.-->
    <transition name="fade-button">
      <button @click="showUsers" v-if="!usersAreVisible">Show Users</button>
      <button @click="hideUsers" v-else-if="usersAreVisible">Hide Users</button>
    </transition>
    <p v-if="usersAreVisible">Users...</p>
  </div>
  <!--Sending via props dialogIsVisible to BaseModal.vue-->
  <base-modal @close="hideDialog" :dialogIsVisible="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>

export default {
  data() {
    return {
      animatedBlock: false,
      dialogIsVisible: false,
      paraIsVisible: false,
      usersAreVisible: false,
      enterInterval: null,
      leaveInterval: null,
    };
  },
  methods: {
    beforeEnter(el) {
      console.log('beforeEnter');
      console.log(el);
      //Setting the style of the element from which beforeEnter is in. In this case a <p></p>
      el.style.opacity = 0;
    },
    beforeLeave(el) {
      console.log('beforeLeave');
      console.log(el);
      el.style.opacity = 1;
    },
    enter(el, done) {
      console.log('enter');
      let round = 1;
      //JS function that allows us to execute code every few miliseconds. The second argument is the amount of miliseconds we want to wait. The first argument is a function that is executed every 20 miliseconds in this case.
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.enterInterval);
          //done argument is the second argument of enter to let know that we finished with the animation.
          done();
        }
      }, 20);
    },
    afterEnter(el) {
      console.log('afterEnter');
      console.log(el);
    },
    leave(el, done) {
      console.log('leave');
      console.log(el);
      let round = 1;
      //JS function that allows us to execute code every few miliseconds. The second argument is the amount of miliseconds we want to wait. The first argument is a function that is executed every 20 miliseconds in this case.
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.leaveInterval);
          //done argument is the second argument of enter to let know that we finished with the animation.
          done();
        }
      }, 20);
    },
    afterLeave(el) {
      console.log('afterLeave');
      console.log(el);
    },
    enterCancelled() {
      clearInterval(this.enterInterval)
    },
    leaveCancelled() {
      clearInterval(this.leaveInterval)

    },
    showUsers() {
      this.usersAreVisible = true;
    },
    hideUsers() {
      this.usersAreVisible = false;
    },
    toggleParagraph() {
      this.paraIsVisible = !this.paraIsVisible;
    },
    animateBlock() {
      this.animatedBlock = true;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
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
  /*transition: transform 0.3s ease-out;*/
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
  /*transform: translateX(-150px);*/
  animation: slide-scale 0.3s ease-out forwards;
}
/*Keyframes: the name is up to you. Here you can defind in detail how the animation is going to be.*/
@keyframes slide-scale {
  /*At 0% of animation completion, so at the beggining. We want the following:*/
  0% {
    transform: translateX(0) scale(1);
  }
  /*After 70% of the animation:*/
  70% {
    transform: translateX(-120px) scale(1.1);
  }
  /*At 100%, so at the end of the animation:*/
  100% {
    transform: translateX(-150px) scale(1);
  }
}

/*Default vue <transform></transform> component classes: */
/*These CSS classes will be added at different times when the element is first added to the DOM. In this example when the <p> appears for the first time.</p>*/
/*CLASSES FOR ENTRYING AN ANIMATION*/

.fade-button-enter-from,
.fade-button-leave-to {
  opacity: 0;
}
.fade-button-enter-active {
  transition: opacity 0.3s ease-out;
}

.fade-button-leave-active {
  transition: opacity 0.3s ease-in;
}
.fade-button-enter-to,
.fade-button-leave-from {
  opacity: 1;
}
</style>
