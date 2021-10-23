<template>
  <div id="app">
    <div class="header">
      <div class="logo">
        <a href="#">Vue Shoe</a>
      </div>
      <div class="cart">
        <i class="fas fa-shopping-cart"> </i>
        <span class="count">{{ productBasket.length }}</span>
        <button class="dropdown-button" @click="showDropdown()">
          <i class="fas fa-caret-down" id="dropdown-button"></i>
        </button>

        <div class="dropdown" id="dropdown">
          <div class="dropdownCont" v-if="productBasket.length > 0">
            <div
              class="dropdownItem"
              v-for="(item, index) in productBasket"
              :key="index"
            >
              <div class="image-box">
                <img
                  :src="require(`@/assets/img/${item[0].image}`)"
                  :alt="item[0].title"
                  class="mini-image"
                />
              </div>
              <div class="property">
                <span>{{ item[0].shortName }}({{ item[1] }}) </span>
              </div>
              <div class="options">
                <span>{{ item[2] * item.count}}$</span>
                <i
                  class="fas fa-minus-square"
                  @click="showCart({ value: item, type: '-' })"
                ></i>
                <span>{{ item.count }}</span>
                <i
                  class="fas fa-plus-square"
                  @click="showCart({ value: item, type: '+' })"
                ></i>
                <i
                  class="far fa-trash-alt"
                  @click="showCart({ value: item, type: 'del' })"
                ></i>
              </div>
            </div>
            <div class="dropdownItem"><span>Total Product: <b>{{totalQuantity}}</b></span> <span>Total Price: <b>{{totalPrice}} $</b></span></div>
          </div>
          <div class="dropdownCont" v-else>Your cart is empty</div>
        </div>
      </div>
    </div>

    <div class="cards">
      <ShoeCard
        v-for="(item, index) in shoesList"
        :key="index"
        :info="item"
        @sendInfo="showCart({ value: $event, type: 'add' })"
      />
    </div>
  </div>
</template>

<script>
import ShoeCard from "./components/ShoeCard.vue";

export default {
  name: "App",
  components: {
    ShoeCard,
  },
  data() {
    return {
      shoesList: [
        {
          id: 1,
          title: "Nike Epic React Flyknit Pearl Pink",
          image: "pink.jpg",
          description:
            "Nike React Infinity Run Flyknit 2 ile koşmayı asla bırakma.Flyknit ve Flywire teknolojisini birleştiren yenilenmiş üst kısım, ihtiyacın olan noktalarda destek ve nefes alabilirlik sunar.",
          rating: 3,
          size: [35, 36, 37, 38, 39, 40, 41, 42],
          color: "pink",
          shortName: "Nike Pearl Pink",
        },
        {
          id: 2,
          title: "Nike Epic React Flyknit Ocean Blue",
          image: "blue.jpg",
          description:
            "Nike React Infinity Run Flyknit 2 ile koşmayı asla bırakma.Flyknit ve Flywire teknolojisini birleştiren yenilenmiş üst kısım, ihtiyacın olan noktalarda destek ve nefes alabilirlik sunar.",
          rating: 4,
          size: [35, 36, 37, 38, 39, 40, 41, 42],
          color: "blue",
          shortName: "Nike Ocean Blue",
        },
        {
          id: 3,
          title: "Nike Epic React Flyknit Night Dark",
          image: "black.jpg",
          description:
            "Nike React Infinity Run Flyknit 2 ile koşmayı asla bırakma.Flyknit ve Flywire teknolojisini birleştiren yenilenmiş üst kısım, ihtiyacın olan noktalarda destek ve nefes alabilirlik sunar.",
          rating: 5,
          size: [38, 39, 40, 41, 42, 43, 44, 45],
          color: "dark",
          shortName: "Nike Night Dark",
        },
        {
          id: 4,
          title: "Nike Epic React Flyknit Smoke Blue",
          image: "white-blue.jpg",
          description:
            "Nike React Infinity Run Flyknit 2 ile koşmayı asla bırakma.Flyknit ve Flywire teknolojisini birleştiren yenilenmiş üst kısım, ihtiyacın olan noktalarda destek ve nefes alabilirlik sunar.",
          rating: 4,
          size: [35, 36, 37, 38, 39, 40, 41, 42],
          color: "white-blue",
          shortName: "Nike White Blue",
        },
      ],
      productBasket: [],
      quantity: 1,
    };
  },
  computed:{
    totalQuantity(){
        let total = 0;
        for (let index = 0; index<this.productBasket.length; index++){
          total += this.productBasket[index].count
        }
      return total
    },
    totalPrice(){
      let price = 0;
      for(let index = 0; index< this.productBasket.length; index++){
        price += this.productBasket[index][2] * this.productBasket[index].count
      }
      return price
    }
  },
 
  methods: {
    showDropdown() {
      let dropdown = document.getElementById("dropdown");
      let icon = document.getElementById("dropdown-button");
      dropdown.classList.toggle("flex");
      icon.classList.toggle("fa-times");
    },
    showCart(val) {
      let index = this.productBasket.findIndex(
        (item) =>
          item[0].color === val.value[0].color && item[1] === val.value[1]
      );
      switch (val.type) {
        case "add":
          if (index >= 0) {
            this.productBasket[index].count += val.value[3];
          } else {
            this.productBasket.unshift({...val.value,count:val.value[3]});
          }
          
          break;
        case '+':
          this.productBasket[index].count++
          break;
        case '-':
          if(this.productBasket[index].count >1){
            this.productBasket[index].count--
          }else{
            this.productBasket.splice(index,1)
          }
          break;
        case 'del':
          this.productBasket.splice(index,1)
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.header {
  height: 80px;
  width: 100%;
  background-color: rgb(205, 204, 210);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10rem;
  box-shadow: 0 1px 15px #303438;
}
.logo a {
  text-decoration: none;
  color: #303438;
  font-size: 2rem;
}
.cart {
  width: 60px;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 99999;
}
.fas {
  font-size: 1.2rem;
  color: #303438;
}
.count {
  position: absolute;
  top: -9px;
  right: 25px;
  width: 15px;
  height: 15px;
  color: white;
  background-color: red;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
  border-radius: 5px;
}
.dropdown-button {
  border: none;
  outline: none;
  background-color: transparent;
}
.dropdown {
  position: absolute;
  width: 380px;
  height: auto;
  top: 40px;
  left: -310px;
  background-color: rgb(228, 227, 233);
  border-radius: 0.5rem;
  border: 1px solid #8f9192;
  padding: 0.5rem;
  display: none;
}
.dropdownCont {
  width: 100%;
  height: auto;
  border-radius: 0.5rem;
  display: flex;
  align-items: center;
  flex-direction: column;
  z-index: 99999;
  position: relative;
}
.dropdownItem {
  width: 100%;
  height: 50px;
  border: 1px solid #cbcdce;
  background-color: white;
  border-radius: 0.5rem;
  display: flex;
  padding: 0 0.4rem;
  margin-bottom: 0.2rem;
  align-items: center;
  justify-content: space-between;
  z-index: 9999;
}
.image-box {
  width: 60px;
  height: 40px;
}
.mini-image {
  max-height: 100%;
  max-width: 100%;
}
.property {
  display: flex;

  align-items: center;
  height: 40px;
}
.property span {
  font-size: 0.9rem;
  font-weight: 700;
  margin-right: 15px;
}
.flex {
  display: flex;
}
.options {
  width: 120px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.options span {
  font-size: 0.9rem;
  font-weight: 700;
}
.options i {
  cursor: pointer;
}
.fa-minus-square {
  color: red;
}
.fa-plus-square {
  color: green;
}
.cards {
  width: 1600px;
  padding: 6rem 0;
  height: auto;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
</style>
