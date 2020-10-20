<template>
  <div class="twoot-item" @click="favourtiteTwoot(twoot.id)">
    <div class="user-profile-twoot">
      <div class="twoot-item-user">
        @{{ username }}
      </div>
      <div class="twoot-item-content">
        {{ twoot.content}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "twootItem",
  props: {
    username: {
      type: String,
      required: true
    },
    twoot: {
      type: Object,
      required: true
    }
  },
  methods: {
    favourtiteTwoot(id) {
      this.$emit('favourite', id)
    }
  }
}
</script>

<style scoped>

.twoot-item {
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #DFE3E8;
  box-sizing: border-box;
  cursor: pointer;
  transition: all 0.25s ease;
}

.twoot-item:hover {
  transform:scale(1.1, 1.1);
}

.twoot-item-user {
  font-weight: bold;
}

</style>