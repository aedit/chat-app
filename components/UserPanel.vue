<template>
  <aside
    :class="[isUserPanelOpened ? 'w-1/3' : 'w-1/5', 'ease bg-white shadow']"
  >
    <div
      class="bg-blue-300 text-white p-2 flex justify-center cursor-pointer"
      @click="isUserPanelOpened = !isUserPanelOpened"
    >
      <span class="border-white border-2 rounded-full mr-2">
        <img class="w-5" :src="icon" alt="" />
      </span>
      Members
    </div>
    <div class="users-container flex flex-col items-center gap-y-3">
      <div
        v-for="(userArr, role) in usersMap"
        :key="role"
        class="user-card border-b-2 border-gray-200 mt-2"
      >
        <div
          class="user-role flex items-center justify-center capitalize text-gray-600 text-xs font-semibold"
        >
          <span>{{ role }}</span>
          <span class="user-role__badge text-xs rounded-full text-white ml-2">{{
            userArr.length
          }}</span>
        </div>
        <div
          v-for="(user, i) in userArr"
          :key="i"
          class="my-4 flex gap-y-2 gap-x-1 justify-center items-center"
        >
          <div class="user-hero relative flex flex-col items-center">
            <span class="relative inline-flex rounded-xl">
              <span
                v-if="user.isOnline"
                class="absolute bg-green-500 h-4 w-4 rounded-full -top-2 -right-2"
              ></span>
              <img
                v-if="user.picture"
                class="user-image rounded-xl"
                :src="user.picture"
                :alt="user.name"
              />
              <span v-else class="user-initials inline-flex justify-center items-center rounded-xl text-2xl text-bold">
                {{ getInitials(user.name) }}
              </span>
            </span>
            <span
              v-if="user.status"
              class="user-status__tag -mt-3 z-10 rounded-xl text-xs px-3 py-1 text-center"
              >{{ user.status }}</span
            >
          </div>
          <div
            v-if="isUserPanelOpened"
            class="user-info -mt-2 flex flex-1 flex-col gap-1 capitalize justify-center"
          >
            <span class="user-info__name">
              {{ user.name }}
              <span class="user-info__tag px-2">
                {{ user.department }}
              </span>
            </span>
            <span class="user-info__location text-sm">{{ user.location }}</span>
          </div>
          <div
            v-if="isUserPanelOpened"
            class="chat flex justify-center items-center"
          >
            <span
              class="p-2 rounded-lg border-2 border-gray-100 hover:border-gray-400 cursor-pointer"
            >
              <img :class="[isUserPanelOpened? 'active':'inactive']" class="chat-icon w-7" :src="chatIcon(user.isOnline)" alt="" />
            </span>
          </div>
        </div>
      </div>
    </div>
  </aside>
</template>

<script>
import leftArrow from '@/assets/icons/chevron_left.svg'
import rightArrow from '@/assets/icons/chevron_right.svg'
import chatActive from '@/assets/icons/chat.svg'
import chatInactive from '@/assets/icons/chatInactive.svg'

export default {
  data() {
    return {
      isUserPanelOpened: true
    }
  },
  computed: {
    icon() {
      return this.isUserPanelOpened ? rightArrow : leftArrow
    },
    chatIcon () {
      return (val) => {
        return val ? chatInactive: chatActive 
      }
    },
    usersMap() {
      const users = this.$store.state.users
      const usersMap = users.reduce((acc, el) => {
        acc[el.role] = [...(acc[el.role] || []), el]
        return acc
      }, {})
      return usersMap
    },
  },
  methods: {
    getInitials(name) {
      return name
        .split(' ')
        .map((n) => n[0].toUpperCase())
        .join('')
    },
  },
}
</script>

<style>
.ease {
  transition: width 0.3s ease-in;
}
.users-container {
  overflow-y: scroll;
  height: calc(100% - 45px);
}

.user-card {
  width: 90%;
}

.user-hero {
  min-width: 112px;
}

.user-role{
  color: #a3a3a3;
}

.user-role__badge{
  background: #A5A2A2;
  font-size: 11px;
  padding: 1px 8px;
}

.user-initials, .user-image {
  min-height: 65px;
  min-width: 65px;
  color: #F2994A;
  background: #F2C94C5E;
  font-weight: 500;
}

.user-status__tag{
background: #F9F9F9;
backdrop-filter: blur(3.5px);
font-size: 9px;
line-height: 14px;
}
.user-info__name{
  font-weight: 500;
  font-size: 15px;
  color: #000;
  text-transform: capitalize;
}
.user-info__tag{
  border: 1px solid #cfcfcf;
  background: 0 0;
  border-radius: 20px;
  font-size: 10px;
  color: #7b7d86;
  font-size: 11px;
  color: #9597A1;
}

.user-info__location{
  color: #9597A1;
}

</style>
