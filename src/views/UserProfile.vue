<template>
  <div class="user_profile">
    <div class="user_profile__user_panel">
      <h1 class="user_profile_username">@{{ state.user.username }}</h1>
      <h3>{{ userID }}</h3>
      <div class="user_profile__admin-badge" v-if="state.user.isAdmin">
        Admin
      </div>
      <div class="user_profile__admin-badge" v-else>
        Not Admin
      </div>
      <div class="user_profile_follower_count">
        <strong>Followers</strong> {{ state.followers }}
      </div>
      <CreateTwootPanel @add-twoot="addTwoot"/>
    </div>
    <div class="user_profile__twoots-wrapper">
      <twootItem
          v-for="twoot in state.user.twoots"
          :key="twoot.id"
          :username="state.user.username"
          :twoot="twoot"
          @favourite="toggleFavourite"/>
    </div>
  </div>
</template>

<script>
import { reactive, watch, computed } from 'vue';
import { useRoute } from 'vue-router';
import {users} from "@/assets/users";
import twootItem from "@/components/twootItem";
import CreateTwootPanel from "@/components/CreateTwootPanel";



export default {
  name: 'App',
  components: {CreateTwootPanel, twootItem},
  setup() {
    const route = useRoute();
    const userID = computed(() => route.params.userID)

    const state = reactive({
      followers: 0,
      user: users[userID.value - 1] || users[0]
    })
    function addFollowers() {
      state.followers ++;
    }
    function toggleFavourite(id) {
      console.log(`Favourited twoot ${id}`)
    }
    function addTwoot(twoot) {
      state.user.twoots.unshift({id: state.user.twoots.length + 1, content: twoot});
    }
    watch(() => state.followers, (new_FollowerCount, old_FollowerCount) => {
      if (old_FollowerCount < new_FollowerCount) {
        console.log(`${state.user.username} has gained a follower`)
      }
    })

    return {
      state,
      addFollowers,
      toggleFavourite,
      addTwoot,
      userID
    }
  }
};


</script>

<style lang="scss" scoped>
.user_profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;

  .user_profile__user_panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;



    h1 {
      margin: 0;
    }

    .user_profile__admin-badge {
      background: rebeccapurple;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      font-weight: bold;
      margin-bottom: 10px;
    }

    .user_profile__twoots-wrapper {
      display: grid;
      grid-gap: 10px;
    }
  }
}
</style>
