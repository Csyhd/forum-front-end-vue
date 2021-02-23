  <template>
  <div class="card mb-3">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img :src="profile.image" width="300px" height="300px" />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{ profile.name }}</h5>
          <p class="card-text">{{ profile.email }}</p>
          <ul class="list-unstyled list-inline">
            <li>
              <strong>{{ profile.Comments.length }}</strong> 已評論餐廳
            </li>
            <li>
              <strong>{{ profile.FavoritedRestaurants.length }}</strong>
              收藏的餐廳
            </li>
            <li>
              <strong>{{ profile.Followers.length }}</strong> followings
              (追蹤者)
            </li>
            <li>
              <strong>{{ profile.Followings.length }}</strong> followers
              (追隨者)
            </li>
          </ul>
          <p></p>
          <form
            action="/following/2?_method=DELETE"
            method="POST"
            style="display: contents"
          >
            <button type="submit" class="btn btn-danger" v-show="user.isAdmin">
              Edit
            </button>
            <button
              type="submit"
              class="btn btn-danger"
              v-show="user.isAdmin === false"
              v-if="isFollowed"
              @click.stop.prevent="deleteFollowing"
            >
              取消追蹤
            </button>
            <button
              type="submit"
              class="btn btn-danger"
              v-show="user.isAdmin === false"
              v-else
              @click.stop.prevent="addFollowing"
            >
              追蹤
            </button>
          </form>
          <p></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const dummyUser = {
  id: 1,
  name: 'root',
  email: 'root@example.com',
  isAdmin: true,
}
export default {
  props: {
    initialProfile: {
      type: Object,
      required: true,
    },
    initialIsFollowed: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      profile: this.initialProfile,
      isFollowed: this.initialIsFollowed,
      user: dummyUser,
    }
  },
  methods: {
    addFollowing() {
      this.isFollowed = true
    },
    deleteFollowing() {
      this.isFollowed = false
    },
  },
}
</script>
