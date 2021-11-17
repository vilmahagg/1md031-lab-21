<template>
  <!-- <div>
    {{ burger.name }} {{ burger.kCal }}
  </div> -->


  <div class="burger">
    <h3>{{burger.name}}</h3>
    <img v-bind:src="burger.img">
    <ul>
      <section class="allergies">
        <li>{{burger.kCal}} kCal</li>
        <li  v-if="burger.gluten==true">Contains Gluten</li>
        <li v-else>Gluten Free</li>
        <li  v-if="burger.lactose==true">Contains Lactose</li>
        <li v-else>Lactose Free</li>
      </section>
    </ul>
    <p>Amount: {{amountOrdered}}</p>
    <div class="burgerButtons">
    <button type="button" v-on:click="addBurger">
      Add Burger
    </button>
     <button type="button" v-on:click="removeBurger">
      Remove Burger
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
}
.burger img {
  width: 16em;
}
ul{
  text-align: left;
}
.allergies {
  font-size: 0.8em; /*not a task but looks better */
  font-weight: bold;
}
.burgerButtons{
  display: grid;
  grid-gap: 0.1em;
  grid-template-columns: 50% 50%;
}

.burgerButtons button{
  height: 3em;
  background-color:peru;
}

.burgerButtons button:hover {
  background-color: gray;
  cursor: pointer;
}

</style>
