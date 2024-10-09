<script>
export default{
  data(){
    return {
      desserts: [
        {id: 1, name: "Waffle", description: "Waffle with berries",  price: 6.50, quantity: 0, image: '../assets/images/image-waffle-desktop.jpg', mobileImage: '/assets/images/image-waffle-mobile.jpg', showQuantity: false },
        {id: 2, name: "Crème Brûlée", description: "Vanilla Bean Crème Brûlée", price: 7.00, quantity: 0,  image: '../assets/images/image-creme-brulee-desktop.jpg', mobileImage: '/assets/images/image-creme-brulee-mobile.jpg', showQuantity: false },
        {id: 3, name: "Macaron", description: "Macaron Mix of Five", price: 8.00, quantity: 0, image: '../assets/images/image-macaron-desktop.jpg', mobileImage: '/assets/images/image-macaron-mobile.jpg', showQuantity: false },
        {id: 4, name: "Tiramisu", description: "Classic Tiramisu", price: 5.50, quantity: 0, image: '../assets/images/image-tiramisu-desktop.jpg', mobileImage: '/assets/images/image-tiramisu-mobile.jpg', showQuantity: false },
        {id: 5, name: "Baklava", description: "Pistachio Baklava", price: 4.00, quantity: 0, image: '/assets/images/image-baklava-desktop.jpg', mobileImage: '/assets/images/image-baklava-mobile.jpg', showQuantity: false },
        {id: 6, name: "Pie", description: "Lemon Meringue Pie", price: 5.00, quantity: 0, image: '/assets/images/image-meringue-desktop.jpg', mobileImage: '/assets/images/image-meringue-mobile.jpg', showQuantity: false },
        {id: 7, name: "Cake", description: "Red Velvet Cake", price: 4.50, quantity: 0, image: '/assets/images/image-cake-desktop.jpg', mobileImage: '/assets/images/image-cake-mobile.jpg', showQuantity: false },
        {id: 8, name: "Brownie", description: "Salted Caramel Brownie", price: 4.50, quantity: 0, image: '/assets/images/image-brownie-desktop.jpg', mobileImage: '/assets/images/image-brownie-mobile.jpg', showQuantity: false },
        {id: 9, name: "Panna Cotta", description: "Vanilla Panna Cotta", price: 6.50, quantity: 0, image: '/assets/images/image-panna-cotta-desktop.jpg', mobileImage: '/assets/images/image-panna-cotta-mobile.jpg', showQuantity: false },
      ],
      showConfirmed: false,
      isMobile: window.innerWidth < 769,
    }
  },
  computed:{
    totalQuantity(){
      return this.desserts.reduce((sum, dessert) => sum + dessert.quantity, 0);
    },
    addedDesserts(){
      return this.desserts.filter(dessert => dessert.quantity > 0);
    },
    sumPrice(){
      return this.addedDesserts.reduce((sum, dessert) => sum + (dessert.price * dessert.quantity), 0);
    },
  },
  mounted() {
    window.addEventListener('resize', this.handleResize);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    toggleQuantity(index) {
      this.desserts[index].showQuantity = !this.desserts[index].showQuantity;
      if (this.desserts[index].showQuantity){
        this.desserts[index].quantity = 1;
      }
      
    },
    incrementQuantity(index){
      this.desserts[index].quantity += 1;
    },
    decrementQuantity(index){
      if(this.desserts[index].quantity > 1){
        this.desserts[index].quantity -= 1;
      }
      else{
        this.desserts[index].showQuantity = false;
        this.desserts[index].quantity = 0;
      }
    },
    removeItem(index){
      const dessert = this.addedDesserts[index];
      const fullDessertIndex = this.desserts.findIndex(d => d.id === dessert.id);
      if (fullDessertIndex !== -1) {
        this.desserts[fullDessertIndex].quantity = 0; 
        this.desserts[fullDessertIndex].showQuantity = false; 
      }
    },
    confirmOrder(){
      this.showConfirmed = true;
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    },

    startNewOrder(){
      this.desserts.forEach((dessert) =>{
        dessert.quantity = 0;
        dessert.showQuantity = false;
      })
      this.showConfirmed = false;
    },
    handleResize() {
      this.isMobile = window.innerWidth < 769;
    }

  }



}
</script>

<template>
  <div class="container">
    <div v-if="showConfirmed" class="overlay"></div>
    <div class="desserts">
        <h1 class="h1">Desserts</h1>
        <div class="slike">
          <div class="card" v-for="(dessert, index) in desserts" :key="dessert.id">
            <div class="img-and-button">
                <img :src="isMobile ? dessert.mobileImage : dessert.image" alt="Waffle with berries" :class="{'orange-border' : dessert.quantity > 0}">
              <button class="add" v-if="!dessert.showQuantity" @click="toggleQuantity(index)"><img src="../assets/images/icon-add-to-cart.svg" alt="add to cart icon" id="add">Add to Cart</button>
              <div class="twobuttons" v-if="dessert.showQuantity">
                <button class="plusiminus" @click="decrementQuantity(index)"><img src="../assets/images/icon-decrement-quantity.svg" alt="slika minusa" class="decrement"></button>
                <p class="obojaj">{{ dessert.quantity }}</p>
                <button class="plusiminus" @click="incrementQuantity(index)"><img src="../assets/images/icon-increment-quantity.svg" alt="slika plusa" class="increment"></button>
              </div>
            </div>
            <p class="light">{{ dessert.name }}</p>
            <p class="bold"> {{ dessert.description }}</p>
            <p class="bold orange">${{ dessert.price.toFixed(2) }}</p>
          </div>
        </div>
    </div>
    <div class="emptyCart" v-if="totalQuantity === 0">
      <h2 class="bold orange">Your Cart ({{ totalQuantity }})</h2>
      <div id="cart-content">
        <div id="empty">
          <img src="../assets/images/illustration-empty-cart.svg" alt="empty cart">
        </div>
        <p>Your added items will appear here</p>
      </div>
    </div>  
    <div class="inCart" v-if="totalQuantity > 0">
      <h2 class="bold orange">Your Cart ({{ totalQuantity }})</h2>
      <div class="cart-items" >
        <div  class="cart-item" v-for="(dessert, index) in addedDesserts" :key="dessert.id" >
          <p class="bold">{{ dessert.description }}</p>
          <div class="x-icon">
            <img src="../assets/images/icon-remove-item.svg" alt="x" class="iksic" @click="removeItem(index)">
          </div>
          <div>
            <p class="orange bold">
              {{ dessert.quantity }}x&nbsp;&nbsp;&nbsp; 
              <span class="light">@ ${{ dessert.price.toFixed(2) }}</span>&nbsp;&nbsp; 
              <span class="dark bold">${{ (dessert.price * dessert.quantity).toFixed(2) }}</span>
            </p>
          </div>
          <hr class="hr">
        </div>
        <div class="total">
          <p>Order Total</p>
          <h2 class="bold">${{ sumPrice.toFixed(2) }}</h2>
        </div>
        <div class="carbon">
          <img src="../assets/images/icon-carbon-neutral.svg" alt="carbon neutral tree" class="tree">
          <p>&nbsp;&nbsp;This is a <span class="bold">carbon-neutral </span>delivery</p>
        </div>
        <div class="confirm">
          <button class="confirmButton" @click="confirmOrder">Confirm Order</button>
        </div>
        
      </div>
    </div>
    <div class="orderConfirmed" v-if="showConfirmed">
      <div class="confirmedTick">
        <img src="../assets/images/icon-order-confirmed.svg" alt="confirmed tick">
      </div>
      <h1 class="bold">Order Confirmed</h1>
      <p>We hope you enjoy your food!</p>
      <div class="confirmedItems">
        <div class="cart-item" v-for="(dessert, index) in addedDesserts" :key="dessert.id">
          <div class="confirmedItem">
            <div class="smallImage">
            <img :src="dessert.image" alt="Small dessert image">
            </div>
            <div class="confirmedItemColumn">
              <p class="bold">{{ dessert.description }}</p>
              <div class="x-icon negativeTop">
                <p class="bold">${{ (dessert.price * dessert.quantity).toFixed(2) }}</p>
              </div>
              <div class="negativeTop">
                <p> 
                {{ dessert.quantity }}x&nbsp;&nbsp;&nbsp;
                @&nbsp;${{ dessert.price }}
                </p>
              </div>
            </div>
          </div>
          <hr class="hr2">
        </div>
        <div class="total moveDown">
          <p>Order Total</p>
          <h2 class="bold">${{ sumPrice.toFixed(2) }}</h2>
        </div>
      </div>
      <button class="startNew" @click="startNewOrder" >Start New Order</button>
    </div>
    
  </div>
</template>

<style>
  body{
    background-color: hsl(20, 50%, 98%);
    display: block;
    font-family: "Red Hat Text", sans-serif;
  }
  #app{
    display: flex;
    margin: 0%;
    max-width: 100vw;
    padding: 7rem;
    color: black;
    justify-content: center;
  }
  .h1{
    color: hsl(14, 65%, 9%);
    font-weight: bold;
    margin-bottom: 30px;
  }
  .hr{
    margin-top: 15px;
    border: solid 1px hsl(13, 31%, 94%); 
  }
  .hr2{
    margin-top: 10px;
    border: solid 1px hsl(0, 26%, 91%);
  }
  .moveDown{
    margin-top: 10px;
  }
  .img-and-button{
    position: relative;
    min-width: 230px;
    height: 230px;
    margin-right: 30px;
    margin-bottom: 30px;
    display: flex;
    flex-direction: column;
  }
  picture{
    display: block;
    width: 100%;
    height: auto;
  }
  img{
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 10px;
    box-sizing: border-box;
  }
  .emptyCart{
    padding: 24px;
    background-color: white;
    min-width: 370px;
    max-width: 370px;
    height: 300px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
  }
  .inCart{
    padding: 24px;
    background-color: white;
    min-width: 370px;
    max-width: 370px;
    height: auto;
    max-height: fit-content;
    border-radius: 10px;
    display: flex;
    flex-direction: column;

  }
  .orderConfirmed{
    padding: 24px;
    background-color: white;
    min-width: 450px;
    max-width: 450px;
    height: auto;
    max-height: 100vh;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 101;
    overflow: auto;
    
  }
  .confirmedItems{
    background-color: hsl(13, 31%, 94%);
    height: auto;
    max-height: fit-content;
    margin-top: 20px;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    padding: 20px 20px 10px 20px;
  }
  .container{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .desserts{
    display: flex;
    width: 820px;
    max-width: 820px;
    flex-wrap: wrap;
    flex-direction: row;
  }
  .slike{
    display: flex;
    flex-wrap: wrap;
  }
  .bold{
    font-weight: bold;
  }
  .orange{
    color: hsl(14, 86%, 42%);
  }
  .light{
    color: hsl(7, 20%, 60%);
  }
  .dark{
    color: hsl(12, 20%, 44%);
  }
  #empty{
    width: 40%;
    margin-bottom: 20px;
    margin-top: 30px;
  }
  #cart-content{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .add{
    position: absolute;
    bottom: -20px;
    left: 50%;
    transform: translateX(-50%);
    width: 150px;
    height: 40px;
    border-radius: 20px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    border: solid 1px black;
    font-weight: bold;
    transition: transform 0.3s ease;
    cursor: pointer;
  }
  .add:hover{
    transform: translateX(-50%) scale(1.1);
    color: hsl(14, 86%, 42%);
    border-color: hsl(14, 86%, 42%);
  }
  #add{
    width: 20px;
    height: auto;
    margin-right: 5px;
  }
  .card{
  margin-bottom: 30px; 
}
.twobuttons{
  width: 150px;
  height: 40px;
  border-radius: 20px;
  background-color: red;
  border: solid 1px red;
  position: absolute;
  left: 50%;
  bottom: -20px;
  transform: translateX(-50%);
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 5px;
  align-items: center;
  padding: 10px;
}
.plusiminus{
  width: 18px;
  height: 18px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  background-color: red;
  border: solid 1px hsl(13, 31%, 94%);
  transition: transform 0.1s ease;
  cursor: pointer;
}
.plusiminus:hover{
  transform: scale(1.2);
}
.iksic{
  width: 15px;
  height: 15px;
  transition: transform 0.1s ease;
  cursor: pointer;
}
.iksic:hover{
  transform: scale(1.2);
}
.obojaj{
  color: white;
}
.decrement{
  width: 10px;
  height: 3px;
}
.increment{
  width: 20px;
  height: 10px;
}
.cart-item{
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  margin-bottom: 15px;
}
.x-icon{
  display: flex;
  justify-content: flex-end;
}
.x-icon img{
  border-radius: 50%;
  border: solid 1px hsl(7, 20%, 60%);
  padding: 2px;
}
.total{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.carbon{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  background-color: hsl(13, 31%, 94%);
  border-radius: 10px;
  padding: 10px;
}
.tree{
  width: 15px;
  height: 15px;
}
.confirm{
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.confirmButton{
  border-radius: 30px;
  width: 100%;
  padding: 15px;
  background-color: hsl(14, 86%, 42%);
  color: white;
  border: none;
  font-weight: bold;
  cursor: pointer;
}
.confirmButton:hover{
  background-color: hsl(14, 100%, 19%);
}
.confirmedTick{
  width: 40px;
  height: 40px;
  margin-bottom: 15px;
}
.smallImage{
  width: 60px;
  height: 50px;
}
.smallImage img{
  width: 100%;
  height: 100%;
  border-radius: 7px;
}
.confirmedItem{
  display: flex;
  flex-direction: row;
}
.confirmedItemColumn{
  width: 100%;
  display: flex;
  flex-direction: column;
  margin-left: 10px;
}
.negativeTop{
  margin-top: -10px;
}
.startNew{
  border-radius: 30px;
  padding: 15px;
  background-color: hsl(14, 86%, 42%);
  cursor: pointer;
  color: white;
  font-weight: bold;
  border: none;
  margin-top: 30px;
}
.startNew:hover{
  background-color: hsl(14, 100%, 19%);
}
.overlay{
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 100;
}
.orange-border{
  border: solid 2px hsl(14, 86%, 42%);
  border-radius: 10px;
  box-sizing: border-box;
}

@media (max-width: 769px){
 
  #app{
    padding: 0;
  }
  .h1{
    margin-bottom: 10px;
  }
  .container{
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 15px 25px;
  }
  .desserts{
    width: 100%;
  }
  .img-and-button{
   margin-right: 0;
   height: auto;
  }
  .slike{
    width: 100%;
  }
  .card{
    width: 100%;
  }
  .emptyCart{
    min-width: 100%;
    width: 100%;
    box-sizing: border-box;
    max-height: fit-content;
    height: auto;
  }
  .inCart{
    min-width: 100%;
    
  }
  .orderConfirmed{
    position: absolute;
    top: 70px;
    left: 0;
    transform: translateX(0);
    min-width: 100%;
    max-height: 90vh;
    
  }
}

@media (min-width:769px) and (max-width: 993px) {
  #app{
    padding: 0;
  }
  .container{
    width: 85vw;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 15px 25px;
  }
  .img-and-button{
    height: auto;
    margin-right: 0;
  }
  .desserts{
    max-width: 100%;
  }
  .slike{
    max-width: 100%;
  }
  .card{
    width: 50%;
    padding: 15px;
  }
  .emptyCart{
    min-width: 100%;
    height: auto;
    max-height: fit-content;
    border-radius: 10px;
    max-width: 100%;
  }
  .inCart{
    min-width: 100%;
  }
  .orderConfirmed{
    position: absolute;
    top: 70px;
    left: 50;
    transform: translateX(-50%);
    min-width: 50%;
    max-height: 90vh;
    
  }
}

@media (min-width: 993px) and (max-width: 1201px) {
    #app{
      padding: 30px;
    }
    .container{
      width: 70vw;
      display: flex;
      flex-direction: row;
      justify-content: center;
      padding: 30px 15px;
    }
    .desserts{
      min-width: 545px;
      display: flex;
      flex-direction: column;
      flex-wrap: wrap;
    }
    .orderConfirmed{
    position: absolute;
    top: 70px;
    left: 50;
    transform: translateX(-50%);
    min-width: 50%;
    max-height: 90vh;
    
  }
}


</style>
