<script lang="ts" setup>
import {onMounted, ref, watch} from "vue";

const streamWrapper = ref<HTMLInputElement | null>(null);
const streamEntry = ref<string>('');


// Simulates the Stream
function createNewText(): void {
  streamEntry.value = "new text: " + new Date();
  setTimeout(() => {
    createNewText();
  }, 3000);
}

// Scroll to the End if user is not scrolling up
function scrollToEnd(): void {
  // its only possible with spa
  const positionNow = window.scrollY + window.innerHeight
  const positionEnd = window.document.body.scrollHeight - 18;
 // nextTick might be usefull
  if (streamWrapper.value && positionNow >= positionEnd) {
    const position = window.document.body.scrollHeight
    window.scrollTo(0, position);
  }
}

watch(streamEntry, (newValue, oldValue) => {
  if (newValue !== oldValue) {
    const child = document.createElement("p")
    child.textContent = streamEntry.value;
    streamWrapper.value?.appendChild(child)
    scrollToEnd();
  }
})

onMounted(() => {
  createNewText()
})
</script>

<template>
  <header>
    <h1>Exercise 1 – simulation streaming</h1>
  </header>
  <main>
    <div class="stream" ref="streamWrapper">
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

header {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-areas: "title title title title";
  gap: 1rem;
  margin-top: 4rem;
  padding: 1rem;
  @media (min-width: 425px) {
    grid-template-columns: repeat(8, 1fr);
    grid-template-areas: ". . title title title title . .";
  }
  @media (min-width: 768px) {
    grid-template-columns: repeat(12, 1fr);
    grid-template-areas: ". . . title title title title title title . . .";
  }
}

h1 {
  grid-area: title;
  font-size: 1.6rem;
  line-height: 2rem;
}

main {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-areas: "stream stream stream stream";
  gap: 1rem;
  margin-top: 2rem;
  padding: 1rem;
  @media (min-width: 425px) {
    grid-template-columns: repeat(8, 1fr);
    grid-template-areas: ". . stream stream stream stream . .";
  }
  @media (min-width: 768px) {
    grid-template-columns: repeat(12, 1fr);
    grid-template-areas: ". . . stream stream stream stream stream stream . . .";
  }
}
.stream {
  grid-area: stream;
}

</style>
