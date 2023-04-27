<template>
  <aside
    :class="[isUserPanelOpened ? 'w-1/3' : 'w-1/5', 'ease bg-white shadow']"
  >
    <div
      class="bg-blue-500 text-white p-2 flex justify-center cursor-pointer"
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
        class="border-b-2 border-gray-200 w-4/5 mt-2"
      >
        <div
          class="flex items-center justify-center capitalize text-gray-600 text-sm font-semibold"
        >
          {{ role }}
          <span class="text-xs rounded-lg bg-gray-200 px-2 ml-2">{{
            userArr.length
          }}</span>
        </div>
        <div
          v-for="(user, i) in userArr"
          :key="i"
          class="my-4 flex gap-y-2 justify-center items-center"
        >
          <div class="user-hero relative flex flex-col items-center">
            <span class="relative inline-flex rounded-xl">
              <span
                v-if="user.isOnline"
                class="absolute bg-green-500 h-4 w-4 rounded-full -top-2 -right-2"
              ></span>
              <img
                v-if="user.picture"
                class="rounded-xl"
                :src="user.picture"
                :alt="user.name"
              />
              <span v-else class="user-initials inline-flex justify-center items-center bg-blue-200 rounded-xl text-lg text-blue-800 text-bold">
                {{ getInitials(user.name) }}
              </span>
            </span>
            <span
              v-if="user.status"
              class="bg-gray-300 -mt-1 z-10 rounded-xl text-xs px-3 py-1 text-center"
              >{{ user.status }}</span
            >
          </div>
          <div
            v-if="isUserPanelOpened"
            class="user-info flex flex-1 flex-col gap-2 justify-center"
          >
            <span class="text-md"
              >{{ user.name }}
              <span class="border-gray-200 border-2 text-xs rounded-lg px-2">{{
                user.department
              }}</span></span
            >
            <span class="text-gray-800 text-sm">{{ user.location }}</span>
          </div>
          <div
            v-if="isUserPanelOpened"
            class="chat flex justify-center items-center"
          >
            <span
              class="p-2 rounded-lg border-2 border-gray-100 hover:border-gray-400 cursor-pointer"
            >
              <img :class="[isUserPanelOpened? 'active':'inactive']" class="chat-icon w-7" :src="chatIcon" alt="" />
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
import chatIcon from '@/assets/icons/chat.svg'

export default {
  data() {
    return {
      isUserPanelOpened: true,
      chatIcon,
    }
  },
  computed: {
    icon() {
      return this.isUserPanelOpened ? rightArrow : leftArrow
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

.user-hero {
  min-width: 112px;
}

.user-initials {
  min-height: 60px;
  min-width: 60px;
}
</style>
