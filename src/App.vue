<template>
  <div class="container">
    <!--Dynamic Class :class. When animatedBlock is true the animate class will be added. -->
    <div class="block" :class="{ animate: animatedBlock }"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <div class="container">
    <!--<transition> Component: Vue Built-in Component to control appearance of the appear and removal of an animation.-->
    <transition name="para">
      <p v-if="paraIsVisible">This is sometimes visible....</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
  <base-modal @close="hideDialog" v-if="dialogIsVisible">
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
    };
  },
  methods: {
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
.para-enter-from {
  /*The starting state*/
  /*opacity: 0;
  transform: translateY(-30px);*/
}
.para-enter-active {
  /*The place where you for example add the transition to tell vue to watch for all CSS properties that might be animated. In this case opacity and transform.*/
  /*transition: all 0.3s ease-out;*/
  animation: slide-scale 0.3s ease-out;
}
.para-enter-to {
  /*The final state*/
  /*opacity: 1;
  transform: translateY(0);*/
}

/*CLASSES FOR LEAVING AN ANIMATION*/
.para-leave-from{
  /*opacity: 1;
  transform: translateY(0);*/
}
.para-leave-active{
  /*transition: all 0.3s ease-in;*/
  animation: slide-scale 0.3 ease-out
}
.para-levate-to{
  /*opacity: 0;
  transform: translateY(30px);*/
}

</style>
