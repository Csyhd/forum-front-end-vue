<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">人氣餐廳</h1>
    <hr />
    <RestaurantsTopCard
      v-for="restaurant in restaurants"
      :key="restaurant.id"
      :initial-restaurant="restaurant"
    />
  </div>
</template>

<script>
import NavTabs from './../components/NavTabs'
import RestaurantsTopCard from './../components/RestaurantsTopCard'
import restaurantsAPI from './../apis/restaurants'
import { Toast } from './../utils/helpers'

export default {
  components: {
    NavTabs,
    RestaurantsTopCard,
  },
  data() {
    return {
      restaurants: [],
    }
  },
  created() {
    const { page = '', categoryId = '' } = this.$route.query
    this.fetchRestaurants({ queryPage: page, queryCategoryId: categoryId })
  },
  methods: {
    async fetchRestaurants() {
      try {
        const response = await restaurantsAPI.getRestaurants({
          page: '',
          categoryId: '',
        })
        console.log('restaurants', response)

        if (response.statusText !== 'OK') {
          throw new Error(response.statusText)
        }
        console.log('response', response)
        const { restaurants } = response.data
        this.restaurants = restaurants
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳資料，請稍後再試',
        })
      }
    },
  },
}
</script>