<script lang="ts" setup>
import type { IRecipe } from '../models/recipes.model'

defineProps({
  recipe: {
    type: Object as PropType<IRecipe>,
    required: true,
  },
})
</script>

<template>
  <li class="rounded-lg overflow-hidden shadow-md bg-white transition transform hover:scale-105 duration-300">
    <NuxtImg :src="recipe.image.url" alt="" class="h-[400px] object-cover" />
    <div class="p-4">
      <h3 class="text-2xl font-bold mb-2">
        {{ recipe.Title }}
      </h3>
      <div class="flex flex-wrap gap-2 mb-2">
        <span v-for="tag in recipe.tags" :key="tag.id" class="py-1 px-2 bg-gray-200 rounded">
          {{ tag.name }}
        </span>
      </div>
      <p class="text-gray-700 mb-4">
        {{ recipe.description }}
      </p>
      <NuxtLink :to="`/recettes/${recipe.slug}`" prefetch class="text-blue-500 hover:underline">
        Voir la recette
      </NuxtLink>
    </div>
  </li>
</template>
