<template>
  <div class="col-3">
    <a href="#">
      <img :src="user.image" width="140px" height="140px" />
    </a>
    <h2>{{ user.name }}</h2>
    <span class="badge badge-secondary"
      >追蹤人數：{{ user.FollowerCount }}</span
    >
    <p class="mt-3">
      <button
        type="button"
        class="btn btn-danger"
        @click.stop.prevent="unFollow"
        v-if="user.isFollowed"
      >
        取消追蹤
      </button>
      <button
        type="button"
        class="btn btn-primary"
        @click.stop.prevent="follow"
        v-else
      >
        追蹤
      </button>
    </p>
  </div>
</template>

<script>
export default {
  props: {
    initialUser: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      user: this.initialUser,
    }
  },
  methods: {
    // follow() {
    //   this.user = {
    //     ...this.user,
    //     isFollowed: true,
    //     FollowerCount: +1,
    //   }
    // },
    follow() {
      const { isFollowed, FollowerCount } = this.user
      this.user.isFollowed = !isFollowed
      this.user.FollowerCount = FollowerCount + 1
    },
    // unFollow() {
    //   this.user = {
    //     ...this.user,
    //     isFollowed: false,
    //     FollowerCount: -1,
    //   }
    // },
    unFollow() {
      const { isFollowed, FollowerCount } = this.user
      this.user.isFollowed = !isFollowed
      this.user.FollowerCount = FollowerCount - 1
    },
  },
}
</script>
