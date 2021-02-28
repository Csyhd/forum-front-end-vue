<template>
  <div class="row">
    <div class="col-md-12 mb-3">
      <h1>{{ restaurant.name }}</h1>
      <p class="badge badge-secondary mt-1 mb-3">
        {{ restaurant.Category ? Category.name : '未分類' }}
      </p>
    </div>
    <div class="col-lg-4">
      <img
        class="img-responsive center-block"
        :src="restaurant.image | emptyImage"
        style="width: 250px; margin-bottom: 25px"
      />
      <div class="contact-info-wrap">
        <ul class="list-unstyled">
          <li>
            <strong>Opening Hour:</strong>
            {{ restaurant.opening_hours }}
          </li>
          <li>
            <strong>Tel:</strong>
            {{ restaurant.tel }}
          </li>
          <li>
            <strong>Address:</strong>
            {{ restaurant.address }}
          </li>
        </ul>
      </div>
    </div>
    <div class="col-lg-8">
      <p>{{ restaurant.description }}</p>
      <router-link
        class="btn btn-primary btn-border mr-2"
        :to="{ name: 'restaurant-dashboard' }"
        >Dashboard</router-link
      >

      <button
        type="button"
        class="btn btn-danger btn-border mr-2"
        @click.stop.prevent="deleteFavorited"
        v-if="restaurant.isFavorited"
      >
        移除最愛
      </button>
      <button
        type="button"
        class="btn btn-primary btn-border mr-2"
        @click.stop.prevent="addFavorited"
        v-else
      >
        加到最愛
      </button>
      <button
        type="button"
        class="btn btn-danger like mr-2"
        @click.stop.prevent="deleteLiked"
        v-if="restaurant.isLiked"
      >
        Unlike
      </button>
      <button
        type="button"
        class="btn btn-primary like mr-2"
        @click.stop.prevent="addLiked"
        v-else
      >
        Like
      </button>
    </div>
  </div>
</template>

<script>
import { emptyImageFilter } from './../utils/mixins'

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
    addFavorited() {
      this.restaurant = {
        ...this.restaurant, // 保留餐廳內原有資料
        isFavorited: true,
      }
    },
    deleteFavorited() {
      this.restaurant = {
        ...this.restaurant, // 保留餐廳內原有資料
        isFavorited: false,
      }
    },
    addLiked() {
      this.restaurant = {
        ...this.restaurant, // 保留餐廳內原有資料
        isLiked: true,
      }
    },
    deleteLiked() {
      this.restaurant = {
        ...this.restaurant, // 保留餐廳內原有資料
        isLiked: false,
      }
    },
  },
}
</script>