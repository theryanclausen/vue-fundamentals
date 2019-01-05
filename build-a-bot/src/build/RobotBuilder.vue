<template>
  <div class="content">
    <button @click="addToCart()" class="add-to-cart">Add to Cart</button>
    <div class="top-row">
      <div :class="[saleBorderStyle, 'top', 'part']">
        <div class="robot-name">
          {{selectedRobot.head.title}}
          <span v-if="selectedRobot.head.onSale" class="sale">Sale!</span>
        </div>
        <img :src="selectedRobot.head.src" title="head">
        <button @click="selectPrevHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobot.leftArm.src" title="left arm">
        <button @click="selectPrevLeftArm()" class="prev-selector">&#9650;</button>
        <button @click="selectNextLeftArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobot.torso.src" title="left arm">
        <button @click="selectPrevTorso()" class="prev-selector">&#9668;</button>
        <button @click="selectNextTorso()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobot.rightArm.src" title="left arm">
        <button @click="selectPrevRightArm()" class="prev-selector">&#9650;</button>
        <button @click="selectNextRightArm()" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobot.base.src" title="left arm">
        <button @click="selectPrevBase()" class="prev-selector">&#9668;</button>
        <button @click="selectNextBase()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
          <tr>
            <th>Robot</th>
            <th class="cost">Cost</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(robot,i) in cart" :key="i">
            <td>{{robot.head.title}}</td>
            <td class="cost">{{robot.cost}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import availableParts from '../data/parts';

const getPrevValidIndex = (index, length) => {
  const prevIndex = index - 1;
  return prevIndex < 0 ? length - 1 : prevIndex;
};
const getNextValidIndex = (index, length) => {
  const NextIndex = index + 1;
  return NextIndex < length ? NextIndex : 0;
};
export default {
  name: 'RobotBuilder',
  data() {
    return {
      availableParts,
      cart: [],
      selectedHeadIndex: 0,
      selectedLeftArm: 0,
      selectedRightArm: 0,
      selectedBase: 0,
      selectedTorso: 0,
    };
  },
  computed: {
    saleBorderStyle() {
      return this.selectedRobot.head.onSale ? 'sale-border' : '';
    },
    selectedRobot() {
      return {
        head: availableParts.heads[this.selectedHeadIndex],
        leftArm: availableParts.arms[this.selectedLeftArm],
        rightArm: availableParts.arms[this.selectedRightArm],
        torso: availableParts.torsos[this.selectedTorso],
        base: availableParts.bases[this.selectedBase],
      };
    },
  },
  methods: {
    addToCart() {
      const robot = this.selectedRobot;
      const cost = robot.head.cost
        + robot.leftArm.cost
        + robot.rightArm.cost
        + robot.torso.cost
        + robot.base.cost;
      this.cart.push({ ...robot, cost });
    },
    selectNextHead() {
      this.selectedHeadIndex = getNextValidIndex(
        this.selectedHeadIndex,
        availableParts.heads.length,
      );
    },
    selectPrevHead() {
      this.selectedHeadIndex = getPrevValidIndex(
        this.selectedHeadIndex,
        availableParts.heads.length,
      );
    },
    selectNextLeftArm() {
      this.selectedLeftArm = getNextValidIndex(
        this.selectedLeftArm,
        availableParts.arms.length,
      );
    },
    selectPrevLeftArm() {
      this.selectedLeftArm = getPrevValidIndex(
        this.selectedLeftArm,
        availableParts.arms.length,
      );
    },
    selectNextRightArm() {
      this.selectedRightArm = getNextValidIndex(
        this.selectedRightArm,
        availableParts.arms.length,
      );
    },
    selectPrevRightArm() {
      this.selectedRightArm = getPrevValidIndex(
        this.selectedRightArm,
        availableParts.arms.length,
      );
    },
    selectNextTorso() {
      this.selectedTorso = getNextValidIndex(
        this.selectedTorso,
        availableParts.torsos.length,
      );
    },
    selectPrevTorso() {
      this.selectedTorso = getPrevValidIndex(
        this.selectedTorso,
        availableParts.torsos.length,
      );
    },
    selectNextBase() {
      this.selectedBase = getNextValidIndex(
        this.selectedBase,
        availableParts.bases.length,
      );
    },
    selectPrevBase() {
      this.selectedBase = getPrevValidIndex(
        this.selectedBase,
        availableParts.bases.length,
      );
    },
  },
};
</script>
<style lang="scss" scoped>


.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
  img {
  width: 165px;
}
}

.top-row {
  display: flex;
  justify-content: space-around;
}
.middle-row {
  display: flex;
  justify-content: center;
}
.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}
.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}
.sale {
  color: red;
}
.content {
  position: relative;
}
.add-to-cart {
  position: absolute;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}
td,
th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}
.cost {
  text-align: right;
}
.sale-border {
  border: 3px solid red;
}
</style>
