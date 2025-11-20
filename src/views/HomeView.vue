<template>
<header id="top-header">
        <h1> BRITAS BURGARE </h1>
    </header>
       <main>
         <section id="burgers"> 
            <h3>Choice of Burger</h3>
            <p>Make your choice</p>
            
        <div id="burger-items" > 
          <Burger
                v-for="burger in burgers"
                :key="burger.name"
                :burger="burger" 
                 @orderedBurger="addToOrder($event)"
                />
            </div>
            </section>

         <h3> Information </h3>
         <section id="contact"> 
            <form>
            <p>
                <label for="fullname">Full name</label><br>
                <input 
                type="text" 
                id="fullname" 
                v-model="fullName" 
                required="required" 
                placeholder="First- and Last name">
                
            </p>
            <p>
                <label for="email">E-mail</label><br>
                <input 
                type="email" 
                id="email" 
                v-model="email" 
                required="required" 
                placeholder="E-mail address">
            </p>
          
            <p>
                <label for="payment">Payment method</label><br>
                <select id="payment" v-model="payment">
                    <option value="Swish">Swish</option>
                    <option value="Card (Visa/Mastercard)">Card (Visa/Mastercard)</option>
                    <option value="Apple Pay">Apple Pay</option>
                    <option value="Invoice w Klarna">Invoice w Klarna</option>
                </select>

            </p>
            <p>
                <label>Gender:</label><br>

                <input type="radio" id="gender-male" v-model="gender" value="male">
                <label for="gender-male">Male</label><br>
                <input type="radio" id="gender-female" v-model="gender" value="female">
                <label for="gender-female">Female</label><br>
                <input type="radio" id="gender-na" v-model="gender" value="na">
                <label for="gender-na">Do not wish to provide</label>
            </p>
            </form>
         <section id="map-section">
            <p>Click on the map to choose where to deliver</p>

            <div id="map-container">
              <div id="map" v-on:click="setLocation">
                <div id="target"
                    v-bind:style="{left:location.x + 'px', top:location.y + 'px' }">
                  T
                </div>
              </div>
            </div>
          </section>   
         </section>

            <button type="button" v-on:click="placeOrder" >
            <img src="https://png.pngtree.com/png-vector/20230707/ourmid/pngtree-green-send-button-in-gradation-color-vector-png-image_7481753.png" width="100" > 
            </button>

        </main>

        <footer> 
            <hr> &copy; Brita Larsson ab
        </footer>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

function MenuItem(name, kCal, img, gluten, lactose) {
  this.name = name
  this.kCal = kCal
  this.img = img
  this.gluten = gluten
  this.lactose = lactose 
}
const burgers = menu
console.log(burgers)

const socket = io('http://localhost:3000')

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: burgers,
      
      orderedBurgers: {},
      fullName: '',
      email: '',
      payment: 'Swish',  
      location: {
         x: 0,
         y: 0
      },
      gender: 'male' 
    }
  },
  methods: {
    placeOrder() {
      console.log("----- ORDER INFO -----")
      console.log("Name:", this.fullName)
      console.log("Email:", this.email)
      console.log("Payment:", this.payment)
      console.log("Gender:", this.gender)
      console.log("Ordered burgers:", this.orderedBurgers)
      
     const items = { ...this.orderedBurgers }

    socket.emit("addOrder", { orderId: this.getOrderNumber(),
                              details: { x: this.location.x, y: this.location.y },
                              orderItems: items,
                              customer: {                     
                              name: this.fullName,
                              email: this.email,
                              payment: this.payment,
                              gender: this.gender
                                 }
                              })  
    },
  

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;},



    addToOrder(event) {
    this.orderedBurgers[event.name] = event.amount
    console.log('Nuvarande orderedBurgers:', this.orderedBurgers)
  }
}}
</script>

<style>
  
body {          
   font-family: "Times New Roman", serif;
   font-size: 40px;
   margin: 150px;
}

#top-header {
    position: relative;
    margin: 150px;
    height: 450px;  
    opacity: 0.8; 
    
    background-image: url("https://www.visitstockholm.se/media/original_images/pelikan-restaurang.jpg");
    background-size: cover;       
    background-position: center;  
    
    display: flex;                
    justify-content: center;       
    align-items: center;
    
    overflow: hidden; 
}

#top-header h1 {
    position: relative;            
    color: brown;
    font-size: 120px;
    margin: 0;
}
    

#burgers {
    background-color: black;
    color: white;
  
}

#burger-items {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;  
    grid-gap: 30px;
    padding: 20px 40px;
}

#burgers p {
    margin-left: 150px;
}
#burgers ul {
    list-style-position: inside;
    padding: 0;
    margin: 5px;  
    text-align: left;          
}

button {
    margin-left: 80px;
    margin-top: 1px;   
}
button:hover {
    background-color: lightgreen;   
    cursor: pointer;               
}

section {
    border: 2px dashed black;
    padding: 20px;
    margin: 150px;
    border-radius: 8px;  
}

h3 {
    margin-left: 150px;
    font-size: 55px;
    color: brown;

} 
#map-section p {
  margin: 0 0 5px 0;      
  text-align: left; }  

#map-section {
  border: None;
  padding: 20px 0 0 0;
  margin: 40px 0 0 0;
  border-radius: 8px;
} 


#map-container {
  width: 100%;       
  height: 600px;
  overflow: scroll;   
  margin: 0;
  border: 1px solid black;
}

#map {
  width: 100%;
  height: 1000px;
  background: url("/img/polacks.jpg"); 
  background-size: cover;
  position: relative;  } 

#target {
  position: absolute;
  font-weight: bold;
  color: red;
}



</style>