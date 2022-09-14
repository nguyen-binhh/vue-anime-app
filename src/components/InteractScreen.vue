<template>
  <h1>Interact Components</h1>
  <card-item
    v-for="(card, index) in cardsContext"
    :key="index"
    :imgBackFaceUrl="`images/${card}.png`"
    :ref="`card-${index}`"
    :card="{ index, value: card }"
    @onFlip="checkRules($event)"
  ></card-item>
</template>

<script>
import CardItem from "./CardItem.vue";
export default {
  emits: ["onFlip", "onFinish"],
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
      required: true,
    },
  },
  components: {
    CardItem,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Right");
        // add class 'disabled' to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisabledMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisabledMode();

        // reset rules to []
        this.rules = [];

        const disabledEls = document.querySelectorAll(".screen .card.disabled");
        if (
          disabledEls &&
          disabledEls.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("wrong");
        // close two card
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);

        // reset rules to []
      } else return false;
    },
  },
};
</script>
