<script setup lang="ts">
import { computed, ref } from 'vue';
interface RockType {
  name: string;
  hardness: number;
  location: string;
  price: number;
}
interface CartRockType {
  name: string;
  price: number;
  amount: number;
}
const myChoice = ref<RockType>({
  name: "",
  hardness: 0,
  location: "",
  price: 0
})
const cart = ref<[CartRockType]>([{
  name: "",
  price: 0,
  amount: 0
}])
const rocks: RockType[] = [

  {
    name: 'granite',
    hardness: 7,
    location: 'Seashore',
    price: 12,
  },
  {
    name: 'Topaz',
    hardness: 8,
    location: 'Mines',
    price: 22,
  },
  {
    name: 'Quartz',
    hardness: 7.5,
    location: 'Sand',
    price: 8,
  },
  {
    name: 'Sandstone',
    hardness: 6,
    location: 'lakeshore',
    price: 13,
  },
  {
    name: 'Marble',
    hardness: 2,
    location: 'rockface',
    price: 15,
  },
  {
    name: 'limestone',
    hardness: 3,
    location: "soil",
    price: 4,
  },

];
const amount = ref(0)
function add() {
  const itemIndex = cart.value.findIndex(o => o.name === myChoice.value.name)
  if (myChoice.value.name != "" && amount.value != 0) {
   if (itemIndex === -1) {
      cart.value.push({ name: myChoice.value.name, price: myChoice.value.price * amount.value, amount: amount.value})
    } else {
      const amountForCart = cart.value[itemIndex].amount + amount.value 
      if (amountForCart <= 20 ) {
        var count = cart.value[itemIndex].amount += amount.value
        var price = count * myChoice.value.price
        cart.value.splice(itemIndex, 1, { name: myChoice.value.name, price: price, amount: count})
      } else {
        cart.value.splice(itemIndex, 1, { name: myChoice.value.name, price: 20 * myChoice.value.price, amount: 20})
        window.alert("cannot purchase more than twenty of each rock")
      }
    }
    myChoice.value = {
      name: "",
      hardness: 0,
      location: "",
      price: 0
    }
    amount.value = 0
  }
}
const subtotal = computed(() => {
  let price = 0
  cart.value.forEach((rock) => {
   price += rock.price
  })
  return price
})
</script>

<template>
<div>
<h1>Choice</h1>
<hr>
Rock: {{myChoice.name}}
<br>
hardness: {{myChoice.hardness}}
<br>
location: {{myChoice.location}}
<br>
price: ${{myChoice.price}}
<br>
</div>
  <select v-model="myChoice">
    <option disabled hidden value=""> Select rock type</option>
    <option v-for="rock in rocks" :key="rock.name" :value="rock">{{ rock.name }}</option>
  </select>
   <select v-model="amount">
    <option disabled value="">quantity</option>
    <option v-for= "type in 20" :key="type" :value="type">{{type}}
    </option>
  </select>
  <br>
  <button @click="add()">add ({{myChoice.name}}) to cart</button>
  <h2> cart</h2>
  <hr>
  <p v-for="item in cart" :key="item.name">{{item.amount}} {{ item.name }} ${{item.price}}</p>
  <p>subtotal {{ subtotal }}</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
