<script setup lang="ts">
import ChristmasPresent from './components/ChristmasPresent.vue'
import ChristmasTree from './components/ChristmasTree.vue'
import { ref } from 'vue'

const presents = ref(['small-red-gift', 'blue-gift', 'tall-red-gift'])
const underTheTree = ref([])

const startDrag = (evt, index) => {
  evt.dataTransfer.dropEffect = 'move'
  evt.dataTransfer.effectAllowed = 'move'
  evt.dataTransfer.setData('presentIndex', index)
}

const onDrop = evt => {
  const presentIndex = evt.dataTransfer.getData('presentIndex')
  const presentToMove = presents.value.splice(presentIndex, 1)
  underTheTree.value.push(...presentToMove)
}
</script>

<template>
  <div class="flex flex-col items-center mt-10 min-h-screen w-full">
    <h1 class="text-xl font-bold">Drag the presents under the tree!</h1>
    <!-- TODO: make ChristmasTree our drop zone! -->
    <ChristmasTree @drop="onDrop($event)" @dragenter.prevent @dragover.prevent :presents="underTheTree" class="mt-16" />
    <div class="pt-10 mt-10 bg-gray-100 w-full justify-center flex-1">
      <div class="flex items-end justify-center" v-auto-animate>
        <!-- TODO: make each present draggable -->
        <ChristmasPresent
          draggable="true"
          @dragstart="startDrag($event, index)"
          v-for="(p, index) in presents"
          :key="p"
          :name="p"
        />
      </div>
    </div>
  </div>
</template>
