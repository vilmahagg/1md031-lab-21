<template>
  <header id="top">
    <h1>Välkommen till Vilmas Burgare</h1>
    <img src="https://www.beernews.se/wp-content/uploads/2018/07/pub-1.jpg" />
  </header>

  <main>
    <section id="menu">
      <h2>Select Burger</h2>
      <p>Select what you want to order</p>
      <div class="burgers">
        <Burger
          v-for="burger in burgers"
          v-bind:burger="burger"
          v-bind:key="burger.name"
          v-on:orderedBurger="addToOrder($event)"
        />
      </div>
    </section>

    <section id="info">
      <h2>Customer Information</h2>
      <p>Provide your info</p>
      <h3>Delivery info</h3>

      <form>
        <div>
          <label for="name">Full Name</label><br />
          <input
            v-model="name"
            type="text"
            id="name"
            name="n"
            required="required"
            placeholder="Full name"
          />
          <!-- <p>name is {{name}}</p> -->
        </div>
        <br />
        <div>
          <label for="email">Email</label><br />
          <input
            v-model="email"
            type="email"
            id="email"
            name="em"
            required="required"
            placeholder="Email Address"
          />
          <!-- <p>email is {{email}}</p> -->
        </div>
        <br/> 

        <div>
          <label for="payment">Payment Option</label><br />
          <select v-model="selected">
            <option disabled value=""></option>
            <option>Card</option>
            <option>Swish</option>
            <option>Invoice</option>
            <option>Bank transaction</option>
          </select>
          <!-- <p>{{selected}} is selected</p> -->
        </div>
       
        <div>
          <h3>Select Gender</h3>
          <input
            type="radio"
            id="female"
            name="gender"
            value="female"
            v-model="picked"
          />
          <label for="female">Female</label>
          <br>
          <input
            type="radio"
            id="male"
            name="gender"
            value="male"
            v-model="picked"
          />
          <label for="male">Male</label>
          <br />
          <input
            type="radio"
            id="no gender"
            name="gender"
            value="no gender"
            v-model="picked"
            checked="checked"
          />
          <label for="nogender">Do not wish to provide</label><br />
          <!-- <p>{{picked}} is picked</p> -->
        </div>
        <h3>Select address</h3>
        <div class="mapwrapper">
          <div id="map" v-on:click="setLocation">{{location}}
            <div
              v-bind:style="{
                left: location.x + 'px',
                top: location.y + 'px',
              }" 
            >
              T
            </div>
          </div>
        </div>
      </form>
    </section>

    <button v-on:click="sendOrder" class="sendbutton" type="submit">
      <img src="img/send.png" />
      Send Order
    </button>
  </main>
  <hr />
  <footer>
    <p>&copy; 2021 Vilmas Burgare Inc.</p>
  </footer>
</template>

<script>
import Burger from "../components/Burger.vue";
import io from "socket.io-client";
import menu from "../assets/menu.json";

const socket = io();

function MenuItem(name, url, kCal, gluten, lactose) {
  this.name = name;
  this.url = url;
  this.kCal = kCal + " kCal";
  this.gluten = gluten;
  this.lactose = lactose;
}


const burgerBeef = new MenuItem(
  "Beef Burger",
  "https://static01.nyt.com/images/2021/05/17/dining/kc-korean-bulgogi-burger/kc-korean-bulgogi-burger-mediumSquareAt3X.jpg",
  "5000",
  false,
  true
);
const burgerChicken = new MenuItem(
  "Chicken Burger",
  "https://www.kitchensanctuary.com/wp-content/uploads/2019/08/Crispy-Chicken-Burger-square-FS-4518.jpg",
  "200",
  false,
  false
);
const burgerHalloumi = new MenuItem(
  "Halloumi Burger",
  "https://www.thelastfoodblog.com/wp-content/uploads/2017/04/Halloumi-Burgers-thumb.jpg",
  300,
  true,
  true
);

let burgerArr = [burgerBeef, burgerChicken, burgerHalloumi];

console.log(burgerArr);

export default {
  name: "Home",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: menu, //burgerArr,
      name: "",
      email: "",
      street: "",
      house: "",
      selected: "Swish", 
      picked: "",
      personalDetails: {},
      orderedBurgers: {},
      location: {x:0,y:0}

    };
  },
  methods: {
    sendOrder: function () {
        this.personalDetails = {
        name: this.name,
        email: this.email,
        payment: this.selected,
        gender: this.picked
      }

      console.log(this.personalDetails,this.orderedBurgers);
      
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: this.location,
        orderItems: this.orderedBurgers,
        personalInfo: this.personalDetails
      });
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location = {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
      };
    }
  },
};
</script>

<style>
#map {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
}

.mapwrapper {
  height: 300px;
  width: 600px;
  overflow: scroll;
}

#map div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

html {
  font-size: 30;
  font-family: arial;
  font-size: 1em;
}

#menu {
  background-color:rgb(233, 211, 182);
  color: black;
  margin: 1.2em;
  padding: 0.9em;
  border: 2px dashed white; 
}

#info {
  background-color:peru;
  margin: 1.2em;
  padding: 0.9em;
  border: 2px dashed white;
  color:black;
}


.burgers {
  display: grid;
  grid-gap: 0.3em;
  grid-template-columns: 33% 33% 33%;
}

.burgers div {
  padding: 0.5em;
}

.sendbutton:hover {
  background-color: rgb(153, 216, 153);
  cursor: pointer;
}

.sendbutton {
  margin: 2em;

}

.sendbutton img {
  width: 1.9em;
}

#top {
  margin-left: 1.2em;
  margin-right: 1.2em;
  height: 170px;
  overflow: hidden;
}

#top img {
  opacity: 0.6;
  width: 100%;
  height: auto;
}

#top h1 {
  position: absolute;
  padding: 0.7em;
  font-size: 3em;
}
</style>
