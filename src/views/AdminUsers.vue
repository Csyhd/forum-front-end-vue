<template>
  <div class="container py-5">
    <!-- AdminNav Component -->
    <!-- 1. 使用先前寫好的 AdminNav -->
    <AdminNav />

    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Email</th>
          <th scope="col">Role</th>
          <th scope="col" width="140">Action</th>
        </tr>
      </thead>
      <tbody v-for="adminUser in adminUsers" :key="adminUser.id">
        <tr>
          <th scope="row">{{ adminUser.id }}</th>
          <td>{{ adminUser.email }}</td>
          <td v-show="adminUser.isAdmin">admin</td>
          <td v-show="!adminUser.isAdmin">user</td>
          <td>
            <button
              type="button"
              class="btn btn-link"
              @click.stop.prevent="toggleRoleStatus(adminUser.id)"
              v-if="adminUser.id !== adminProfile.id && adminUser.isAdmin"
            >
              set as user
            </button>
            <button
              type="button"
              class="btn btn-link"
              v-show="!adminUser.isAdmin"
              @click.stop.prevent="toggleRoleStatus(adminUser.id)"
            >
              set as admin
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
const dummyUser = {
  profile: {
    id: 1,
    name: '蠔嗲油',
    email: 'root@example.com',
    password: '$2a$10$jBS/Y4.hceDXkEC5y9ZGne81Y7i5wNwNcy6wAKjNdBykCzlEfWmLm',
    isAdmin: true,
    image: 'https://i.imgur.com/3keAGHT.jpeg',
    createdAt: '2020-12-15T06:35:43.000Z',
    updatedAt: '2021-02-22T16:41:47.000Z',
  },
  users: [
    {
      id: 1,
      name: '蠔嗲油',
      email: 'root@example.com',
      password: '$2a$10$jBS/Y4.hceDXkEC5y9ZGne81Y7i5wNwNcy6wAKjNdBykCzlEfWmLm',
      isAdmin: true,
      image: 'https://i.imgur.com/3keAGHT.jpeg',
      createdAt: '2020-12-15T06:35:43.000Z',
      updatedAt: '2021-02-22T16:41:47.000Z',
    },
    {
      id: 2,
      name: 'user1',
      email: 'user1@example.com',
      password: '$2a$10$m11qLlDOol1b3XCa393Bwe.hW4mt/6DS.mUsgFtati5LW4BbX81EG',
      isAdmin: true,
      image: 'https://i.imgur.com/PhcKzNf.jpeg',
      createdAt: '2020-12-15T06:35:43.000Z',
      updatedAt: '2021-02-22T16:42:25.000Z',
    },
    {
      id: 3,
      name: 'user2',
      email: 'user2@example.com',
      password: '$2a$10$IgMneSD6HZiHt0C6we./cOPyq70YhAWNZEqC4YTtJHK8ejgS1J/3q',
      isAdmin: false,
      image: null,
      createdAt: '2020-12-15T06:35:43.000Z',
      updatedAt: '2020-12-15T06:35:43.000Z',
    },
    {
      id: 7,
      name: '123',
      email: 'ben7152000@gmail.com',
      password: '$2a$10$gEUc6f3gn62yaOuq89gQLeUr4FbzGkVyMegUmbvPLEMi4Co76LXni',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-12T09:16:05.000Z',
      updatedAt: '2021-02-12T09:16:05.000Z',
    },
    {
      id: 17,
      name: 'sa',
      email: '123@gmail.com',
      password: '$2a$10$7b76MIBXCOZwWQ0Idm1Ul.HKChUtn/.IjTAHkNMZRI/t//tvbREca',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-13T07:41:08.000Z',
      updatedAt: '2021-02-13T07:41:08.000Z',
    },
    {
      id: 27,
      name: 'root',
      email: 'root',
      password: '$2a$10$0tt4RHOVuM./uXJpobmPa.ypCUSn8sHT7QnsQX73K6IUK1RtqEqTu',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-19T03:51:00.000Z',
      updatedAt: '2021-02-19T03:51:00.000Z',
    },
    {
      id: 37,
      name: '熊班長',
      email: 'monalisa@example.com',
      password: '$2a$10$cKM74wfFPqDcby2dvTTlt.It4ENCscYRI3Wrxfs5XvWYbQuO0OkBu',
      isAdmin: true,
      image: 'https://i.imgur.com/MdRdcnZ.jpeg',
      createdAt: '2021-02-22T16:43:13.000Z',
      updatedAt: '2021-02-22T16:45:55.000Z',
    },
    {
      id: 47,
      name: 'max',
      email: 'wl00887404@gmail.com',
      password: '$2a$10$Tw8ZgJJRX/CQzfvsK2yX..5wMCOQE.QwFN2SsQGjUkIIy1yLFguk6',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-24T10:35:36.000Z',
      updatedAt: '2021-02-24T10:35:36.000Z',
    },
    {
      id: 57,
      name: 'TED',
      email: 'peter@parker.com',
      password: '$2a$10$CYwMLBc6gPia3vUrcDX0Yez3u.ZYxuatmvdktWCyy9nTeEzCzNdXa',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-25T04:42:33.000Z',
      updatedAt: '2021-02-25T04:42:33.000Z',
    },
    {
      id: 67,
      name: 'abcd',
      email: 'abcd@gmail.com',
      password: '$2a$10$J86cWm36XZX.DGfJMby8JOKt5YfkLb0YFJQLU9IphaU9eQmzYEO2.',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-25T05:10:58.000Z',
      updatedAt: '2021-02-25T05:10:58.000Z',
    },
    {
      id: 77,
      name: 'w',
      email: 'w@w.w',
      password: '$2a$10$Pl73ykEJnKnXVcj.P6vKz.Ga/mIHwcK3rbJRYtClBYgHkbBpzL/wK',
      isAdmin: false,
      image: null,
      createdAt: '2021-02-25T05:52:07.000Z',
      updatedAt: '2021-02-25T05:52:07.000Z',
    },
  ],
}
import AdminNav from './../components/AdminNav'
export default {
  components: {
    AdminNav,
  },
  data() {
    return {
      adminProfile: {},
      adminUsers: [],
    }
  },
  created() {
    this.fetchAdminUsersData()
  },
  methods: {
    fetchAdminUsersData() {
      this.adminUsers = dummyUser.users
      this.adminProfile = dummyUser.profile
    },
    toggleRoleStatus(adminUserId) {
      this.adminUsers = this.adminUsers.map((adminUser) => {
        if (adminUser.id === adminUserId) {
          return {
            ...adminUser,
            isAdmin: !adminUser.isAdmin,
          }
        }
        return adminUser
      })
    },
  },
}
</script>