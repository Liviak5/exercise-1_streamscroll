<script lang="ts" setup>
import {onMounted, ref, watch} from "vue";

const streamWrapper = ref<HTMLDivElement | null>(null);
const streamArray = ref<string[]>([]);
const streamEntry = ref<string>('');
const threshold = ref<number>(18);


// Simulates the Stream
function createNewText(): void {
  streamArray.value.push("new text: " + new Date());
  setTimeout(() => {
    createNewText();
  }, 3000);
}

// Scroll to the End if user is not scrolling up
function scrollToEnd(): void {
  // its only possible with spa
  const positionNow = window.scrollY + window.innerHeight
  const positionEnd = window.document.body.scrollHeight - threshold.value;
 // nextTick might be usefull
  if (streamWrapper.value && positionNow >= positionEnd) {
    const position = window.document.body.scrollHeight
    window.scrollTo(0, position);
  }
}

watch(streamEntry, (newValue, oldValue) => {
  if (newValue !== oldValue) {
    scrollToEnd();
  }
})

onMounted(() => {
  createNewText()
})
</script>

<template>
  <header class="wrapper">
    <h1>Exercise 1 – simulation streaming</h1>
  </header>
  <main class="wrapper">
    <div class="stream" ref="streamWrapper">
      <p v-for="(entry, index) in streamArray" :key="index">{{entry}}</p>
    </div>
  </main>
</template>

<style lang="scss">
@mixin venderprefix($property, $value) {
  -webkit-#{$property}: $value;
  -moz-#{$property}: $value;
  -ms-#{$property}: $value;
  -o-#{$property}: $value;
  $property: $value;
}
html {
  @include venderprefix(box-sizing, border-box);
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  font-weight: 300;
  font-size: 18px;
  line-height: 25px;
}

*,
*:before,
*:after {
  @include venderprefix(box-sizing, inherit);
  margin: 0;
  padding: 0;
}

.wrapper{
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  padding: 1rem;
  @media (min-width: 425px) {
    grid-template-columns: repeat(8, 1fr);
  }
  @media (min-width: 768px) {
    grid-template-columns: repeat(12, 1fr);
  }
}
header {
  grid-template-areas: "title title title title";
  margin-top: 4rem;
  padding: 1rem;
  @media (min-width: 425px) {
    grid-template-areas: ". . title title title title . .";
  }
  @media (min-width: 768px) {
    grid-template-areas: ". . . title title title title title title . . .";
  }
}
main {
  grid-template-areas: "stream stream stream stream";
  margin-top: 2rem;
  @media (min-width: 425px) {
    grid-template-areas: "stream stream stream stream . . . .";
  }
  @media (min-width: 768px) {
    grid-template-areas: ". . . . stream stream stream stream stream stream . .";
  }
}

h1 {
  grid-area: title;
  font-size: 1.6rem;
  line-height: 2rem;
}


.stream {
  grid-area: stream;
}

</style>
