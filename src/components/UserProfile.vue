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
      <form class="user_profile__create_twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharacterCount > 180}">
        <label for="newTwoot"><strong>New Twoot</strong>({{ newTwootCharacterCount }} / 180)</label>
        <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>

        <div class="user_profile__create_twoot__type">
          <label for="newTwootType"><strong>Type: </strong></label>
          <select id="newTwootType" v-model="selectedTwootType">
            <option :value="option.value" v-for="(option, index) in TwootTypes" :key="index">
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>
          Twoot!
        </button>
      </form>
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


export default {
  name: 'App',
  components: {twootItem},
  data() {
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      followers: 0,
      TwootTypes: [
        { value: 'draft', name: 'Draft'},
        { value: 'instant', name: 'Instant Twoot'}
      ],
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
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    addFollowers() {
      this.followers ++;
    },
    toggleFavourite(id) {
      console.log(`Favourited twoot ${id}`)
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== 'draft') {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent
        })
        this.newTwootContent = '';
      }
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

    .user_profile__create_twoot {
      padding-top: 20px;
      display: flex;
      flex-direction: column;

      &.--exceeded {
        color: red;
        border-color: red;
        button {
          background-color: red;
          border: none;
          color: white;
        }
      }
    }

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
