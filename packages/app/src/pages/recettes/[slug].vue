<script lang="ts" setup>
import type { RecipeData } from '~/models/recipes.model'

const { findOne } = useStrapi4()
const route = useRoute()

const { data: recipe, pending } = useAsyncData(
  'recipe',
  () => findOne<RecipeData>(`recipes/${route.params.slug}`),
)
</script>
<template>
  <div class="container mx-auto py-8">
    <template v-if="pending">
      <p class="text-9xl text-center text-gray-500">
        ...
      </p>
    </template>
    <template v-else>
      <NuxtImg
        :src="recipe.data.image.url" alt="" aria-hidden="true"
        class="h-[500px] object-contain object-center w-full bg-white rounded-md shadow-md mb-6"
      />
      <div class="max-w-4xl mx-auto">
        <!-- Ajout du bouton de retour en arrière -->
        <router-link to="/" class="text-blue-500 hover:underline mb-4 inline-block">
          Retour à la liste des recettes
        </router-link>
        <!-- Fin du bouton de retour en arrière -->
        <h1 class="text-4xl md:text-5xl font-semibold mb-4 text-gray-800">{{ recipe.data.title }}</h1>
        <div class="flex items-center gap-x-2 mb-4">
          <span v-for="tag in recipe.data.tags" :key="tag.id" class="py-1 px-2 bg-blue-200 text-blue-800 rounded-md">
            {{ tag.name }}
          </span>
        </div>
        <p class="text-gray-700 mb-6">{{ recipe.data.description }}</p>
        <div class="flex flex-col md:flex-row">
          <div class="flex flex-col w-full md:w-1/2 mr-4 mb-6">
            <h2 class="text-2xl md:text-3xl font-semibold mb-2 text-gray-800">
              Ingrédients
            </h2>
            <ul class="list-none gap-2 p-0">
              <li v-for="ingredient in recipe.data.ingredients" :key="ingredient.id" class="mb-2">
                <p class="font-bold text-gray-800">
                  {{ ingredient.name }}
                </p>
                <p class="text-gray-600">{{ ingredient.quantity }}</p>
              </li>
            </ul>
          </div>
          <div class="flex flex-col w-full md:w-1/2">
            <h2 class="text-2xl md:text-3xl font-semibold mb-2 text-gray-800">
              Instructions
            </h2>
            <ol class="list-decimal gap-2 p-0">
              <li v-for="(step, index) in recipe.data.instructions" :key="index" class="mb-2">
                <p class="text-gray-800">
                  {{ step }}
                </p>
              </li>
            </ol>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>
