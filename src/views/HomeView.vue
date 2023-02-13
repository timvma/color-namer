<script setup>
import { ref , reactive, watch} from 'vue'
import colorDatabase from '../assets/color.json'

let color = ref("000000")
let colorName = ref("black");

let database = colorDatabase

watch(color, ( newValue, oldValue ) => {  
  findTheNearestColor(newValue)
})


let findTheNearestColor = (color)=> {

let r = parseInt(color.substring(0, 2), 16);
let g = parseInt(color.substring(2, 4), 16);
let b = parseInt(color.substring(4, 6), 16);

let closestColor = null;
let closestDistance = Infinity;

for (let i = 0; i < database.length; i++) {
  let dbColor = database[i].hex;

  // Convert the hex color to RGB
  let dbR = parseInt(dbColor.substring(0, 2), 16);
  let dbG = parseInt(dbColor.substring(2, 4), 16);
  let dbB = parseInt(dbColor.substring(4, 6), 16);

  // Calculate the Euclidean distance
  let distance = Math.sqrt(
    Math.pow(r - dbR, 2) +
    Math.pow(g - dbG, 2) +
    Math.pow(b - dbB, 2)
  );

  if (distance < closestDistance) {
    closestColor =  database[i].color;
    closestDistance = distance;    
  }
}
colorName.value = closestColor;
};


</script>

<template>
  <main>
    <div class="wrapper">
      <h1>#{{ color }}</h1>
        <input type="text" v-model="color" maxlength="6">
        <div :style="{ background: `#${color}` }" class="color__square"></div>
        <h2>{{ colorName }}</h2>
    </div>
  </main>
</template>


<style scoped >
.color__square {
  width:50px;
  height:50px;
}

</style>