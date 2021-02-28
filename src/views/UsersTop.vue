<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">美食達人</h1>
    <hr />
    <div class="row text-center">
      <UsersCard v-for="user in users" :key="user.id" :initial-user="user" />
    </div>
    <!-- <UsersPagination
      v-if="totalPage.length > 1"
      :current-page="currentPage"
      :total-page="totalPage"
      :previous-page="previousPage"
      :next-page="nextPage"
    /> -->
  </div>
</template>

<script>
import NavTabs from './../components/NavTabs'
import UsersCard from './../components/UsersCard'
// import UsersPagination from './../components/UsersPagination'
import usersAPI from './../apis/user'
import { Toast } from './../utils/helpers'

export default {
  components: {
    NavTabs,
    UsersCard,
    // UsersPagination,
  },
  data() {
    return {
      users: [],
      currentPage: 1,
      totalPage: [],
      previousPage: -1,
      nextPage: -1,
    }
  },
  created() {
    this.fetchUsers()
  },
  methods: {
    async fetchUsers() {
      try {
        const { data } = await usersAPI.getTopUsers()
        this.users = data.users.map((user) => ({
          id: user.id,
          name: user.name,
          image: user.image,
          followerCount: user.FollowerCount,
          isFollowed: user.isFollowed,
        }))
        // console.log(data)
        // const { users, page, totalPage, prev, next } = data
        // this.users = users
        // this.currentPage = page
        // this.totalPage = totalPage
        // this.previousPage = prev
        // this.nextPage = next
      } catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取得美食達人,請稍後再試',
        })
      }
    },
  },
}
</script>