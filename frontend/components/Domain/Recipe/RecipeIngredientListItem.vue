<template>
  <div class="ma-0 pa-0 text-subtitle-1 dense-markdown ingredient-item">
    <SafeMarkdown v-if="parsedIng.quantity" class="d-inline" :source="parsedIng.quantity" />
    <template v-if="parsedIng.unit">{{ parsedIng.unit }} </template>
    <SafeMarkdown v-if="parsedIng.note && !parsedIng.name" class="text-bold d-inline" :source="parsedIng.note" />
    <template v-else>
      <SafeMarkdown v-if="parsedIng.name" class="text-bold d-inline" :source="parsedIng.name" />
      <SafeMarkdown v-if="parsedIng.note" class="note" :source="parsedIng.note" />
    </template>
  </div>
</template>
<script lang="ts">
import { computed, defineComponent, toRefs } from "@nuxtjs/composition-api";
import { RecipeIngredient } from "~/lib/api/types/group";
import { useParsedIngredientText } from "~/composables/recipes";

export default defineComponent({
  props: {
    ingredient: {
      type: Object as () => RecipeIngredient,
      required: true,
    },
    disableAmount: {
      type: Boolean,
      default: false,
    },
    scale: {
      type: Number,
      default: 1,
    },
  },
  setup(props) {
    const parsedIng = computed(() => {
      return useParsedIngredientText(props.ingredient, props.disableAmount, props.scale);
    });

    return {
      parsedIng,
    };
  },
});
</script>
<style lang="scss">
.ingredient-item {
  .d-inline {
    & > p {
      display: inline;
    }
  }

  .text-bold {
    font-weight: bold;
  }
}

.note {
  line-height: 0.8em;
  font-size: 0.8em;
  opacity: 0.7;
}
</style>
