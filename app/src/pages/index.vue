<script setup lang="ts">
import { type Ref, ref } from 'vue';
import { useSeoMeta } from '@unhead/vue';
import { useAssets, useOrganisation, useProjects } from '~/composables/states';

const org: Ref<Organisation> = ref(useOrganisation().value);
const projects: Ref<Project[] | null> = ref(useProjects().value);
const assets: Ref<String | null> = ref('');

const updatePage = async () => {
  org.value = useOrganisation().value;
  projects.value = useProjects().value;
  assets.value = useAssets().value;

  useSeoMeta({
    title: org.value?.name || 'Taskimatti',
    description: org.value.description,
  });
};

while (true) {
  await updatePage();
  await new Promise((resolve) => setTimeout(resolve, 50));
  if (org.value && projects.value && assets.value) break;
}
</script>

<template>
  <div class="flex flex-col justify-center items-center min-h-[80vh]">
    <h1 class="text-3xl p-4">Select project</h1>
    <ol>
      <li class="m-6" v-for="project in projects" :key="project.id">
        <NuxtLink :to="project.id">
          <div class="rounded-lg shadow-lg overflow-hidden" :style="'background: ' + project.colorScheme">
            <NuxtImg
              :src="assets + project.image"
              class="w-full h-32 object-cover"
              width="384"
              height="128"
              quality="10"
              placeholder="/images/placeholder.svg"
              :alt="project.name"
            />
            <div class="p-6">
              <h2 class="text-2xl font-bold mb-2">{{ project.name }}</h2>
              <p class="text-gray-300">{{ project.description }}</p>
            </div>
          </div>
        </NuxtLink>
      </li>
    </ol>
  </div>
</template>

<style scoped></style>
