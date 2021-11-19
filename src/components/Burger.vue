<template>
  <div class="burger">
    <h3>{{burger.name}}</h3>
    <img v-bind:src="burger.img">
    <ul>
      <section class="burgerinfo">
        <li>{{burger.kCal}} kCal</li>
        <li v-if="burger.gluten==true">Contains <span class="allergy">Gluten</span></li>
        <li v-else>Gluten Free</li>
        <li v-if="burger.lactose==true">Contains <span class="allergy">Lactose</span></li>
        <li v-else>Lactose Free</li>
      </section>
    </ul>
    <p>Amount: {{amountOrdered}}</p>
    <div class="burgerButtons">
    <button type="button" v-on:click="addBurger">
      +
    </button>
     <button type="button" v-on:click="removeBurger">
      -
    </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Burger",
  props: {
    burger: Object, 
  },
  data: function() {
    return{
      amountOrdered: 0,
    }
   
  },
  methods: {
    addBurger: function(){
      this.amountOrdered ++;
      this.$emit('orderedBurger', 
      {name: this.burger.name,
      amount: this.amountOrdered
      }
      );
      
    },
    removeBurger: function(){
      if (this.amountOrdered === 0) return
      this.amountOrdered --;
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.burger{
  text-align: center;
  border: 2px solid rgb(255, 255, 255);
}
.burger h3{
  font-size: 30px;
  font-family: sans-serif;
  color:peru;
}
.burger img {
  width: 16em;
}

ul{
  text-align: left;
}
.burgerinfo{
  font-size: 0.8em; 
}
.allergy {
  
  font-weight: bold;
}
.burgerButtons{
  display: grid;
  grid-gap: 0.1em;
  grid-template-columns: 50% 50%;
}

.burgerButtons button{
  height: 30px;
  background-color:peru;
  border-radius: 10px;
  border: none;
  font-size: 20px;

}

.burgerButtons button:hover {
  background-color:white;
  cursor: pointer;
}

</style>
