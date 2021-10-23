<template>
  <div>
    <div class="alert-box">
      <b-alert
        :show="dismissCountDown"
        dismissible
        variant="success"
        @dismissed="dismissCountDown = 0"
        @dismiss-count-down="countDownChanged"
      >
        Your product has been added to your cart...
      </b-alert>
    </div>
    <div id="card-container">
      <div class="left-side">
        <div class="image-box">
          <img :src="require(`@/assets/img/${info.image}`)" alt="Image" />
        </div>

        <div class="color-rate">
          <div class="color">Color: <span :class="info.color"></span></div>
          <div class="rating">
            <span style="margin-right: 10px">Rating:</span>
            <span
              class="fa fa-star"
              :class="info.color + 'text'"
              v-for="(item, index) in info.rating"
              :key="'full-' + index"
            >
            </span>
            <span
              class="far fa-star"
              v-for="(item, index) in 5 - info.rating"
              :key="'empty-' + index"
              :class="info.color + 'text'"
            >
            </span>
          </div>
        </div>

        <div class="sizes">
          <span>Size:</span>

          <div class="size-box" v-for="(item, index) in info.size" :key="index">
            <span
              :class="[info.color, { focus: selectedSizeBox == index }]"
              @click="selectSize(index, item)"
              >{{ item }}</span
            >
          </div>
        </div>
      </div>

      <div class="right-side">
        <div class="product-info">
          <span class="product-title">{{ info.title }}</span>
          <p class="product-description">{{ info.description }}</p>
        </div>
        <div class="select">
          <span class="price" :class="info.color">
            {{ result }}
            $
          </span>
          <select :class="info.color" v-model="quantity">
            <option :value="item" v-for="item in 10" :key="item">
              {{ item }}
            </option>
          </select>
        </div>

        <div class="addCart">
          <span class="addCartButton">
            <button
              :class="info.color"
              @click="sendToCart(info, selectingSize,firstCost,quantity), showAlert()"
            >
              Add to Cart
            </button>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    info: Object,
  },
  data() {
    return {
      quantity: 1,
      dismissSecs: 3,
      dismissCountDown: 0,
      selectedSizeBox: 0,
      selectingSize: this.info.size[0],

    };
  },
  computed:{
    result(){
      let result = Math.floor(this.selectingSize * 5.5)  * this.quantity
      return result
    },
    firstCost(){
      let cost = Math.floor(this.selectingSize * 5.5)
      return cost
    }
  },
  methods: {
    sendToCart(info,selectingSize,firstCost,quantity) {
      this.$emit("sendInfo",[info,selectingSize,firstCost,quantity]);
    },
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    showAlert() {
      this.dismissCountDown = this.dismissSecs;
    },
    selectSize(index, item) {
      this.selectedSizeBox = index;
      this.selectingSize = item;
    },
  },
};
</script>

<style scoped>
.alert-box {
  position: absolute;
  top: 100px;
  left: 150px;
  width: 1600px;
  z-index: 1;
}
#card-container {
  width: 790px;
  height: auto;
  border: 1px solid rgb(205, 204, 210);
  border-radius: 15px;
  margin-bottom: 1rem;
  padding: 2rem;
  background-color: white;
  display: flex;
  justify-content: space-between;
  box-shadow: 0 5px 10px 3px rgba(0, 0, 0, 0.2);
}
.left-side {
  width: 400px;
  height: calc(450px - 2rem);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.image-box {
  width: 100%;
  height: auto;
}
.image-box img {
  max-width: 100%;
  max-height: 100%;
  transition: 0.3s ease;
  transform: scale(1) rotate(0);
  z-index: 0;
}
.image-box img:hover {
  transform: scale(1.1) rotate(-3deg);
}
.color-rate {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 400px;
}
.rating {
  display: flex;
  align-items: center;
}
.fa-star {
  font-size: 20px;
}
.checked {
  color: orange;
}
.color {
  width: auto;
  height: 45px;
  display: flex;
  align-items: center;
}
.color span {
  display: flex;
  width: 25px;
  height: 25px;
  border-radius: 0.5rem;
  margin-left: 10px;
}
.pink {
  background-color: rgb(211, 174, 172);
  transition: 0.2s ease;
}
.pinktext {
  color: rgb(211, 174, 172);
}
.pink:hover {
  background-color: rgb(209, 156, 153);
}
.blue {
  background-color: rgb(57, 155, 247);
  transition: 0.2s ease;
}
.bluetext {
  color: rgb(57, 155, 247);
}
.blue:hover {
  background-color: rgb(26, 126, 219);
}
.dark {
  background-color: #303438;
  transition: 0.2s ease;
}
.dark:hover {
  background-color: #5b6269;
}
.darktext {
  color: #303438;
}
.white-blue {
  background: rgb(205, 204, 210);
}
.white-blue:hover {
  background: rgb(171, 170, 179);
}
.white-bluetext {
  color: rgb(205, 204, 210);
}
.sizes {
  width: 100%;
  display: flex;
  height: 55px;
  align-items: center;
}
.size-box {
  display: flex;
  justify-content: flex-end;
  width: 100%;
  position: relative;
}

.sizes span {
  margin-right: 10px;
}
.size-box span {
  width: 40px;
  height: 40px;
  padding: 0.5rem;
  border-radius: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: white;
  font-weight: 600;
  font-size: 18px;
}

.focus {
  border: 3px solid red;
}
.right-side {
  display: flex;
  width: 275px;
  flex-direction: column;
  justify-content: space-between;
}
.product-title {
  font-size: 1.5rem;
  font-weight: bold;
  width: 275px;
  height: 100px;
  font-family: "Helvetica", sans-serif;
  display: flex;

  align-items: center;
}
.product-description {
  color: #7f7f7f;
}
.select {
  margin-top: 20px;
  display: flex;
  width: 275px;
  height: 40px;
  justify-content: space-between;
  position: relative;
}
.select select {
  appearance: none;
  outline: 0;
  border: 0;
  box-shadow: none;
  color: white;
  padding-left: 1.5rem;
  cursor: pointer;
  width: 160px;
  font-size: 20px;
  border-top-right-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
}
.select::after {
  content: "+";
  position: absolute;
  padding: 1rem;
  right: -5px;
  top: -20px;
  font-size: 30px;
  transition: 0.25s all ease;
  pointer-events: none;
  color: white;
}
.price {
  width: 110px;
  height: 40px;
  display: flex;
  justify-content: center;
  color: white;
  align-items: center;
  font-size: 20px;
  border-top-left-radius: 0.5rem;
  border-bottom-left-radius: 0.5rem;
}
.addCart {
  display: flex;
  width: 100%;
  height: 55px;
  align-items: center;
  justify-content: space-between;
}
.addCartButton {
  width: 140px;
  height: 40px;
}
.addCartButton button {
  width: 275px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  outline: none;
  border: none;
  color: white;
  font-size: 16px;
  font-weight: 600;
  border-radius: 0.5rem;
}
</style>