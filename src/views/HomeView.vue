<script setup>
import { ref ,toRefs, reactive, watch} from 'vue'
import colorDatabase from '../assets/color.json'


const state = reactive({
  color: "000000",
  colorName:"black"
})

let color = ref("000000")
let colorName = ref("black");

let database = colorDatabase

console.log("DATABSE", colorDatabase.length)

function findRepetitions(arr) {
  const seen = new Set();
  const repetitions = [];

  for (const obj of arr) {
    const objString = JSON.stringify(obj);

    if (seen.has(objString)) {
      repetitions.push(obj);
    } else {
      seen.add(objString);
    }
  }

  return repetitions;
}


const repeatedObjects = findRepetitions(colorDatabase);

console.log("REPEATED",repeatedObjects.length);  

const unique = ref()


unique.value = colorDatabase.reduce((acc, obj) => {
  const existingObj = acc.find(item => item.color === obj.color);
  if (existingObj) {
    if (!Array.isArray(existingObj.hex)) {
      existingObj.hex = [existingObj.hex];
    }
    if (!existingObj.hex.includes(obj.hex)) {
      existingObj.hex.push(obj.hex);
    }
  } else {
    acc.push({ color: obj.color, hex: obj.hex });
  }
  return acc;
}, []);

console.log("UNIQUE",unique.value.length);


var array3 = colorDatabase.filter(function(obj) { return repeatedObjects.indexOf(obj) == -1; });
console.log(array3)


watch(color, ( newValue, oldValue ) => {  
  if(newValue.length == 6)
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
      <div class="content__container">

        <div class="color__visual">
          <div :style="{ background: `#${color}` }" class="color__square"></div>
          <h1>#{{ color }}</h1>
        </div>
 

      <div class="color-input__container">
        <input type="text" v-model="color" maxlength="6" class="color_input">
      </div>


        <h2>{{ colorName }}</h2>

          
      </div>
    </div>
  </main>
</template>


<style scoped >
.color__square {
  width:50px;
  height:50px;
}

.color_input {
  padding: 1rem 1rem 1rem 2rem;
  font-size: 1rem;
  border-radius: 5px;
  border: none;
 
}
.color-input__container {
  position: relative;
}
.color-input__container::after {
  content: "#";
  position: absolute;
  top: 1rem;
  left: 1rem;
  color: #999;
  font-size: 1rem;
}

.wrapper {
  display: grid;
  width: 100%;
  height: 100%;
  place-items: center;
  min-height: 100vh;
}
.color__visual {
  display: flex;
  gap: 1rem;
  align-items: center;
}
</style>