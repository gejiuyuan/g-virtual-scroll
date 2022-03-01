<script setup lang="ts">
import { computed, onMounted, reactive, ref, shallowReactive } from 'vue'
import faker from '@faker-js/faker'

const city = faker.address.city();
const animal = faker.animal.type();

console.log(city, '\n', animal);


interface ListProps {
  itemHeight?: number | string;
  data?: any[];
}

const props = withDefaults(defineProps<ListProps>(), {
  data: () => Array.from({length: 1e4}).map((v, i) => i+1),
  itemHeight: 80,
})

function getItemHeight() {
  return typeof props.itemHeight === 'number' ? props.itemHeight : parseFloat(props.itemHeight);
}

const totoalHeight = getItemHeight() * props.data.length;

const variableData = shallowReactive({
  viewportHeight: 0,
  offset: 0,
  startIndex: 0,
  endIndex: 0,
})
const containerRef = ref<HTMLElement>();

const visibleData = computed(() => {
  return props.data.slice(variableData.startIndex, variableData.endIndex);
})

const visibleCount = computed(() => {
  return Math.ceil(variableData.viewportHeight / getItemHeight())
})

onMounted(() => {
  variableData.viewportHeight = containerRef.value!.offsetHeight;
})

const showingStyle =computed(() => {
  return {
    transform: `translate3d(0,${variableData.offset}px,0)`
  };
}) 

function containerScroll(ev: UIEvent) {
  const { scrollTop } = containerRef.value!;
  variableData.offset = scrollTop;
  console.log(scrollTop);
  
  variableData.startIndex = ~~(scrollTop / getItemHeight());
  variableData.endIndex = variableData.startIndex + visibleCount.value;
}

</script>

<template>
  <section id="container" ref="containerRef" @scroll="containerScroll">
    <div class="temp-wrap" :style="{ height: totoalHeight + 'px' }"></div>
    <div class="showing" :style="showingStyle">
      <div
        class="item"
        v-for="item of visibleData"
        :key="Math.random()"
        :style="{ height: itemHeight + 'px' }"
      >
        {{item}}
      </div>
    </div>
  </section>
</template>

<style scoped>
#container {
  position: relative;
  height: 650px;
  background-color: antiquewhite;
  overflow: overlay;
}
.showing {
  position: absolute;
  top: 0%;
  left: 0;
}
</style>
