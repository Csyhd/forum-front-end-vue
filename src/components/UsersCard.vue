<template>
  <div class="col-3">
    <a href="#">
      <img :src="user.image | emptyImage" width="140px" height="140px" />
    </a>
    <h2>{{ user.name }}</h2>
    <span class="badge badge-secondary"
      >追蹤人數：{{ user.followerCount }}</span
    >
    <p class="mt-3">
      <button
        type="button"
        class="btn btn-danger"
        @click.stop.prevent="deleteFollowing(user.id)"
        v-if="user.isFollowed"
      >
        取消追蹤
      </button>
      <button
        type="button"
        class="btn btn-primary"
        @click.stop.prevent="addFollowing(user.id)"
        v-else
      >
        追蹤
      </button>
    </p>
  </div>
</template>

<script>
import { emptyImageFilter } from './../utils/mixins'
import usersAPI from './../apis/user'
import { Toast } from './../utils/helpers'

export default {
  mixins: [emptyImageFilter],

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
    // follow() {
    //   const { isFollowed, FollowerCount } = this.user
    //   this.user.isFollowed = !isFollowed
    //   this.user.FollowerCount = FollowerCount + 1
    // },
    async addFollowing(userId) {
      // console.log(userId)
      try {
        const { data } = await usersAPI.addFollowing({ userId })

        if (data.status !== 'success') {
          throw new Error(data.message)
        }
        const { isFollowed, FollowerCount } = this.user
        this.user.isFollowed = !isFollowed
        this.user.FollowerCount = FollowerCount + 1
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法加入追蹤，請稍後再試',
        })
      }
    },
    // unFollow() {
    //   this.user = {
    //     ...this.user,
    //     isFollowed: false,
    //     FollowerCount: -1,
    //   }
    // },
    // unFollow() {
    //   const { isFollowed, FollowerCount } = this.user
    //   this.user.isFollowed = !isFollowed
    //   this.user.FollowerCount = FollowerCount - 1
    // },
    async deleteFollowing(userId) {
      try {
        const { data } = await usersAPI.deleteFollowing({ userId })

        if (data.status !== 'success') {
          throw new Error(data.message)
        }

        const { isFollowed, FollowerCount } = this.user
        this.user.isFollowed = !isFollowed
        this.user.FollowerCount = FollowerCount - 1
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取消追蹤，請稍後再試',
        })
      }
    },
  },
}
</script>
