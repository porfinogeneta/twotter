<template>
  <div class="user_profile">
    <div class="user_profile__user_panel">
      <h1 class="user_profile_username">@{{ user.username }}</h1>
      <div class="user_profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user_profile__admin-badge" v-else>
        Not Admin
      </div>
      <div class="user_profile_follower_count">
        <strong>Followers</strong> {{ followers }}
      </div>
      <CreateTwootPanel @add_twoot="addTwoot"/>
    </div>
    <div class="user_profile__twoots-wrapper">
      <twootItem
          v-for="twoot in user.twoots"
          :key="twoot.id"
          :username="user.username"
          :twoot="twoot"
          @favourite="toggleFavourite"/>
    </div>
  </div>
</template>

<script>
import twootItem from "@/components/twootItem";
import CreateTwootPanel from "@/components/CreateTwootPanel";


export default {
  name: 'App',
  components: {CreateTwootPanel, twootItem},
  data() {
    return {
      followers: 0,

      user: {
        id: 1,
        username: 'JohnK',
        firstName: 'John',
        lastName: 'Kowalski',
        email: 'jkowalski@gmail.com',
        isAdmin: true,
        twoots: [
          { id: 1, content: "Twotter is amazing"},
          { id: 2, content: "Twotter is bad"},
        ]
      },
    }
  },
  watch: {
    followers(new_FollowerCount, old_FollowerCount) {
      if (old_FollowerCount < new_FollowerCount) {
        console.log(`${this.user.username} has gained a follower`)
      }
    }
  },
  methods: {
    addFollowers() {
      this.followers ++;
    },
    toggleFavourite(id) {
      console.log(`Favourited twoot ${id}`)
    },
    addTwoot(twoot) {
      this.user.twoots.unshift({id: this.user.twoots.length + 1, content: twoot});
    }
  },
  mounted() {
    this.addFollowers()
  }
}
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
