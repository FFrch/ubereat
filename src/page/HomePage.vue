<template>
  <div class="home--page">
    <div class="header">
      <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" alt="">
      <input v-model="user_search_restaurant" type="search" placeholder="De quoi avez-vous envie?">
    </div>
    <div class="banner"></div>
    <restaurant-row v-for="(data, i) in data_restautant" :key="i" :three_restaurant="data"/>
  </div>

</template>

<script>
  // Imports
  import BDD from '../BDD.js'
  import { onMounted, ref, watch } from "vue";

  // Components
  import RestaurantRow from "../components/RestaurantRow.vue";

  export default {
    name: 'HomePage',
    components : {
      RestaurantRow,
    },
    setup() {
      class Restaurant {
        constructor(name, note, image, drive_time) {
          this.name = name
          this.note = note
          this.image = image
          this.drive_time = drive_time
        }
      }

      let data_restautant = ref([]);
      let all_restaurant = [];

      const makeDataRestaurant = () => {
        let three_restaurant = [];

        for (const restaurant of BDD){
          const new_restaurant = new Restaurant(restaurant.name, restaurant.note, restaurant.image, restaurant.drive_time);

          // Push all new restaurant in all_restaurant array
          all_restaurant.push(new_restaurant)

          if(three_restaurant.length === 2){
            three_restaurant.push(new_restaurant);
            data_restautant.value.push(three_restaurant);
            three_restaurant = [];
          } else {
            three_restaurant.push(new_restaurant);
          }
        }
      }

      // Searchbar
      let user_search_restaurant = ref('');

      watch(user_search_restaurant, new_value => {
        let regex = RegExp(new_value);
        let search_restaurant = all_restaurant.filter(restaurant => regex.test(restaurant.name))
        console.log(search_restaurant)
      })

      onMounted(makeDataRestaurant);

      return {
        data_restautant,
        user_search_restaurant,
      }
    }
  }
</script>

<style lang="scss">
  .home--page {
    .header {
      height: 120px;
      width: 100%;
      display: flex;
      justify-content: space-between ;
      align-items: center;

      img {
        width: 200px;
      }

      input {
        background-color: #f6f6f6;
        border: none;
        height: 60px;
        width: 400px;
        outline: none;
        padding-left: 20px;
      }
    }

    .banner {
      height: 200px;
      width: 100%;
      background-image: url("https://ubernewsroomapi.10upcdn.com/wp-content/uploads/sites/345/2016/03/blog_960x540.jpg");
      background-size: cover;
      background-position: top center;
    }
  }
</style>
