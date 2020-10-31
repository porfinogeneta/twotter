<template>
  <form class="user_profile__create_twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharacterCount > 180}">
        <label for="newTwoot"><strong>New Twoot</strong>({{ newTwootCharacterCount }} / 180)</label>
        <textarea class="text_area" id="newTwoot" rows="4" v-model="state.newTwootContent"></textarea>

        <div class="user_profile__create_twoot__type">
          <label for="newTwootType"><strong>Type: </strong></label>
          <select id="newTwootType" v-model="state.selectedTwootType">
            <option :value="option.value" v-for="(option, index) in state.TwootTypes" :key="index">
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>
          Twoot!
        </button>
      </form>
</template>

<script>
import { reactive, computed } from 'vue';

export default {
  name: "CreateTwootPanel",

  setup() {
    const state = reactive({
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft', name: 'Draft' },
        { value: 'instant', name: 'Instant Twoot'}
      ]
    })
    const newTwootCharacterCount = computed(() => state.newTwootContent.length)

    // function createNewTwoot() {
    //   if (state.newTwootContent && state.selectedTwootType !== 'draft') {
    //     ctx.emit('add-twoot', state.newTwootContent);
    //     state.newTwootContent = '';
    //   }
    // }

    return {
      state,
      newTwootCharacterCount,
      // createNewTwoot
    }
  }
};
</script>

<style lang="scss" scoped>

.user_profile__create_twoot {
  margin-top: 20px;
  padding: 20px 0;
  display: flex;
  flex-direction: column;
  textarea {
    border: 1px solid #DFE3E8;
    border-radius: 5px;
  }

  &.--exceeded {
    color: red;
    border-color: red;

      button {
        background-color: red;
        color: white;
      }

  }
}
</style>