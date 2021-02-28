<template>
  <div class="card mb-3" style="max-width: 540px; margin: auto">
    <div class="row no-gutters">
      <div class="col-md-4">
        <a href="#">
          <img class="card-img" :src="restaurant.image | emptyImage" />
        </a>
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{ restaurant.name }}</h5>
          <span class="badge badge-secondary"
            >收藏數：{{ restaurant.FavoriteCount }}</span
          >
          <p class="card-text">
            {{ restaurant.description }}
          </p>
          <a href="#" class="btn btn-primary mr-2">Show</a>

          <button
            type="button"
            class="btn btn-danger mr-2"
            @click.stop.prevent="deleteFavorited(restaurant.id)"
            v-if="restaurant.isFavorited"
          >
            移除最愛
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click.stop.prevent="addFavorited(restaurant.id)"
            v-else
          >
            加到最愛
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { emptyImageFilter } from './../utils/mixins'
import usersAPI from './../apis/user'
import { Toast } from './../utils/helpers'

export default {
  mixins: [emptyImageFilter],

  props: {
    initialRestaurant: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      restaurant: this.initialRestaurant,
    }
  },
  methods: {
    // addFavorited() {
    //   this.restaurant = {
    //     ...this.restaurant, // 保留餐廳內原有資料
    //     isFavorited: true,
    //   }
    // },
    // deleteFavorited() {
    //   this.restaurant = {
    //     ...this.restaurant, // 保留餐廳內原有資料
    //     isFavorited: false,
    //   }
    // },
    async addFavorited(restaurantId) {
      try {
        const { data } = await usersAPI.addFavorite({ restaurantId })
        if (data.status !== 'success') {
          throw new Error(data.message)
        }
        this.restaurant = {
          ...this.restaurant, // 保留餐廳內原有資料
          isFavorited: true,
        }
        console.log('data', data)
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法將餐廳加入最愛，請稍後再試',
        })
        console.log('error', error)
      }
    },
    async deleteFavorited(restaurantId) {
      try {
        const { data } = await usersAPI.deleteFavorite({ restaurantId })
        if (data.status !== 'success') {
          throw new Error(data.message)
        }
        this.restaurant = {
          ...this.restaurant, // 保留餐廳內原有資料
          isFavorited: false,
        }
        console.log('data', data)
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法將餐廳移除最愛，請稍後再試',
        })
        console.log('error', error)
      }
    },
  },
}
</script>