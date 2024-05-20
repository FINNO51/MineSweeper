<script setup>
import Score from "./Score.vue";
import { ref, reactive } from "vue";

defineProps({
  msg: {
    type: String,
    required: true,
  },
});

const counter = ref(10);
const marked = ref(0);
const intro = ref(true);
const columns = ref(10);
const rows = ref(10);
const grid = ref([[]]);
const victory = ref(0);

function startGame() {
  rows.value = columns.value;
  let localgrid = new Array(rows.value);

  for (let i = 0; i < rows.value; i++) {
    localgrid[i] = new Array(columns.value);
    for (let j = 0; j < columns.value; j++)
      localgrid[i][j] = {
        number: 0,
        hidden: true,
        marked: false,
        x: i,
        y: j,
      };
  }

  grid.value = localgrid;

  for (let i = 0; i < counter.value; i++) {
    let x = Math.floor(Math.random() * rows.value);
    let y = Math.floor(Math.random() * columns.value);
    while (grid.value[x][y].number === -1) {
      x = Math.floor(Math.random() * rows.value);
      y = Math.floor(Math.random() * columns.value);
    }
    grid.value[x][y].number = -1;
    if (x != 0) {
      if (grid.value[x - 1][y].number != -1) grid.value[x - 1][y].number++;
      if (y != 0)
        if (grid.value[x - 1][y - 1].number != -1)
          grid.value[x - 1][y - 1].number++;
      if (y != columns.value - 1)
        if (grid.value[x - 1][y + 1].number != -1)
          grid.value[x - 1][y + 1].number++;
    }
    if (x != rows.value - 1) {
      if (grid.value[x + 1][y].number != -1) grid.value[x + 1][y].number++;
      if (y != 0)
        if (grid.value[x + 1][y - 1].number != -1)
          grid.value[x + 1][y - 1].number++;
      if (y != columns.value - 1)
        if (grid.value[x + 1][y + 1].number != -1)
          grid.value[x + 1][y + 1].number++;
    }
    if (y != 0)
      if (grid.value[x][y - 1].number != -1) grid.value[x][y - 1].number++;
    if (y != columns.value - 1)
      if (grid.value[x][y + 1].number != -1) grid.value[x][y + 1].number++;
  }

  intro.value = !intro.value;
}

function victoryCheck() {
  if (counter.value == marked.value) {
    let total = 0;
    for (let i = 0; i < rows.value; i++)
      for (let j = 0; j < columns.value; j++)
        if (grid.value[i][j].marked && grid.value[i][j].number == -1) total++;

    if (total == counter.value) victory.value = 1;
  }
}

function leftClick(item) {
  if (victory.value == 0) {
    item.hidden = false;
    let redo = rows.value;
    if (columns.value > rows.value) redo = columns.value;
    for (let i = 0; i < redo; i++) {
      for (let x = 0; x < rows.value; x++)
        for (let y = 0; y < columns.value; y++) {
          if (x != 0) {
            if (
              grid.value[x - 1][y].number == 0 &&
              grid.value[x - 1][y].hidden == false
            ) {
              grid.value[x][y].hidden = false;
              if (grid.value[x][y].marked == true) {
                marked.value--;
                grid.value[x][y].marked = false;
              }
            }
            if (y != 0)
              if (
                grid.value[x - 1][y - 1].number == 0 &&
                grid.value[x - 1][y - 1].hidden == false
              ) {
                grid.value[x][y].hidden = false;
                if (grid.value[x][y].marked == true) {
                  marked.value--;
                  grid.value[x][y].marked = false;
                }
              }
            if (y != columns.value - 1)
              if (
                grid.value[x - 1][y + 1].number == 0 &&
                grid.value[x - 1][y + 1].hidden == false
              ) {
                grid.value[x][y].hidden = false;
                if (grid.value[x][y].marked == true) {
                  marked.value--;
                  grid.value[x][y].marked = false;
                }
              }
          }
          if (x != rows.value - 1) {
            if (
              grid.value[x + 1][y].number == 0 &&
              grid.value[x + 1][y].hidden == false
            ) {
              grid.value[x][y].hidden = false;
              if (grid.value[x][y].marked == true) {
                marked.value--;
                grid.value[x][y].marked = false;
              }
            }
            if (y != 0)
              if (
                grid.value[x + 1][y - 1].number == 0 &&
                grid.value[x + 1][y - 1].hidden == false
              ) {
                grid.value[x][y].hidden = false;
                if (grid.value[x][y].marked == true) {
                  marked.value--;
                  grid.value[x][y].marked = false;
                }
              }
            if (y != columns.value - 1)
              if (
                grid.value[x + 1][y + 1].number == 0 &&
                grid.value[x + 1][y + 1].hidden == false
              ) {
                grid.value[x][y].hidden = false;
                if (grid.value[x][y].marked == true) {
                  marked.value--;
                  grid.value[x][y].marked = false;
                }
              }
          }
          if (y != 0)
            if (
              grid.value[x][y - 1].number == 0 &&
              grid.value[x][y - 1].hidden == false
            ) {
              grid.value[x][y].hidden = false;
              if (grid.value[x][y].marked == true) {
                marked.value--;
                grid.value[x][y].marked = false;
              }
            }
          if (y != columns.value - 1)
            if (
              grid.value[x][y + 1].number == 0 &&
              grid.value[x][y + 1].hidden == false
            ) {
              grid.value[x][y].hidden = false;
              if (grid.value[x][y].marked == true) {
                marked.value--;
                grid.value[x][y].marked = false;
              }
            }
        }
    }

    if (item.number == -1) {
      grid.value[item.x][item.y].number = -2;
      victory.value = -1;
      for (let i = 0; i < rows.value; i++)
        for (let j = 0; j < columns.value; j++)
          if (grid.value[i][j].number == -1) grid.value[i][j].hidden = false;
    } else victoryCheck();
  }
}

function rightClick(item) {
  if (victory.value == 0) {
    if (item.marked == false) {
      item.marked = true;
      marked.value++;
    } else {
      item.marked = false;
      marked.value--;
    }

    victoryCheck();
  }
}
</script>

<template>
  <div class="preload">
    <img src="@/assets/x.svg" style="width: 100%; height: 100%" /><img
      src="@/assets/bomb.svg"
      style="width: 100%; height: 100%"
    /><img src="@/assets/explosion.svg" style="width: 100%; height: 100%" />
  </div>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <Transition>
      <div v-show="intro" class="intro">
        <div class="introtext">Mine total:</div>
        <div class="container">
          <span class="next" v-if="counter < 15" v-on:click="counter++"></span>
          <span class="prev" v-if="counter > 5" v-on:click="counter--"></span>
          <div id="box">
            <span>{{ counter }}</span>
          </div>
        </div>
        <div class="introtext">Cells per side:</div>
        <div class="container">
          <span class="next" v-if="columns < 15" v-on:click="columns++"></span>
          <span class="prev" v-if="columns > 5" v-on:click="columns--"></span>
          <div id="box">
            <span>{{ columns }}</span>
          </div>
        </div>
        <div class="submit" @click="startGame()">Start</div>
      </div>
    </Transition>
    <Transition>
      <div v-show="!intro" class="hud">
        <div class="grid">
          <div class="column" v-for="line in grid">
            <div v-for="item in line">
              <div
                class="cell"
                v-if="item.marked && item.hidden"
                :style="{ 'background-color': 'dimgray' }"
                @contextmenu.prevent="rightClick(item)"
              >
                <img src="@/assets/x.svg" style="width: 100%; height: 100%" />
              </div>
              <div
                class="cell"
                v-else-if="item.hidden"
                :style="{ 'background-color': 'dimgray' }"
                v-on:click="leftClick(item)"
                @contextmenu.prevent="rightClick(item)"
              ></div>
              <div
                class="cell"
                v-else-if="item.number == -2"
                :style="{ 'background-color': 'magenta' }"
              >
                <img
                  src="@/assets/explosion.svg"
                  style="width: 100%; height: 100%"
                />
              </div>
              <div
                class="cell"
                v-else-if="item.number == -1"
                :style="{ 'background-color': 'magenta' }"
              >
                <img
                  src="@/assets/bomb.svg"
                  style="width: 100%; height: 100%"
                />
              </div>
              <div
                class="cell"
                v-else-if="item.number == 0"
                :style="{ 'background-color': 'white' }"
              ></div>
              <div
                class="cell"
                v-else-if="item.number == 1"
                :style="{ 'background-color': 'blue' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 2"
                :style="{ 'background-color': 'green' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 3"
                :style="{ 'background-color': 'red' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 4"
                :style="{ 'background-color': 'darkblue' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 5"
                :style="{ 'background-color': 'darkred' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 6"
                :style="{ 'background-color': 'darkcyan' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 7"
                :style="{ 'background-color': 'black' }"
              >
                {{ item.number }}
              </div>
              <div
                class="cell"
                v-else-if="item.number == 8"
                :style="{ 'background-color': 'grey' }"
              >
                {{ item.number }}
              </div>
            </div>
          </div>
        </div>

        <Score :victory="victory" :counter="counter" :marked="marked"></Score>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 5vh;
  position: absolute;
  top: 5%;
}

.column {
  position: relative;
}

.cell {
  position: relative;
  border: 0.2vh solid var(--color-border);
  height: calc(60vh / v-bind(columns));
  width: calc(60vh / v-bind(rows));
  font-size: calc(50vh / v-bind(rows));
  font-weight: 700;
  text-align: center;
  line-height: calc(50vh / v-bind(rows));
  color: var(--vt-c-text-dark-2);
}

.intro {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 20px;
}

.introtext {
  position: relative;
  font-size: 1.2rem;
  top: 40%;
}

.container {
  position: relative;
  display: block;
  width: 80px;
  height: 50px;
  border-radius: 40px;
  border: 2px solid var(--color-border);
  transition: 0.5s;
  top: 45%;
}

.container:hover {
  width: 120px;
  border: 2px solid var(--color-border-hover);
}

.container .next {
  position: absolute;
  top: 50%;
  right: 30px;
  display: block;
  width: 12px;
  height: 12px;
  border-top: 2px solid var(--color-border-hover);
  border-left: 2px solid var(--color-border-hover);
  z-index: 1;
  transform: translateY(-50%) rotate(135deg);
  cursor: pointer;
  opacity: 0;
  transition: 0.5s;
}

.container:hover .next {
  opacity: 1;
  right: 20px;
}

.container .prev {
  position: absolute;
  top: 50%;
  left: 30px;
  display: block;
  width: 12px;
  height: 12px;
  border-top: 2px solid var(--color-border-hover);
  border-left: 2px solid var(--color-border-hover);
  z-index: 1;
  transform: translateY(-50%) rotate(315deg);
  cursor: pointer;
  opacity: 0;
  transition: 0.5s;
}

#box span {
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  text-align: center;
  line-height: 46px;
  color: hsla(160, 100%, 37%, 1);
  font-size: 24px;
  font-weight: 700;
  user-select: none;
}

.container:hover .prev {
  opacity: 1;
  left: 20px;
}

.submit {
  position: relative;
  width: 110px;
  height: 50px;
  border-radius: 40px;
  border: 2px solid var(--color-border);
  transition: 0.5s;
  display: block;
  text-align: center;
  line-height: 46px;
  color: hsla(160, 100%, 37%, 1);
  font-size: 24px;
  font-weight: 700;
  user-select: none;
  cursor: pointer;
  top: 52%;
}

.submit:hover {
  background-color: hsla(160, 100%, 37%, 1);
  color: #fff;
}

.grid {
  position: relative;
  height: 60vh;
  width: 60vh;
  top: 20%;
  display: grid;
  grid-template-columns: repeat(v-bind(rows), 1fr);
  grid-template-rows: repeat(v-bind(columns), 1fr);
  user-select: none;
}

.hud {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
  width: 80vw;
  align-items: center;
}

.greetings {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  height: 100%;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: center;
  }
}

@media screen and (orientation: portrait) {
  .grid {
    position: relative;
    width: 70vw;
    height: 70vw;
    padding-top: 10vh;
    display: grid;
    grid-template-columns: repeat(v-bind(columns), 1fr);
    grid-template-rows: repeat(v-bind(rows), 1fr);
    user-select: none;
  }
  .cell {
    position: relative;
    border: 0.2vw solid var(--color-border);
    height: calc(70vw / v-bind(rows));
    width: calc(70vw / v-bind(columns));
    font-size: calc(55vw / v-bind(rows));
    font-weight: 700;
    text-align: center;
    align-items: center;
    line-height: calc(55vw / v-bind(rows));
    color: var(--vt-c-text-dark-2);
  }
  .hud {
    display: flex;
    flex-direction: column-reverse;
    justify-content: space-evenly;
    width: 80vw;
    align-items: center;
  }
}
</style>
