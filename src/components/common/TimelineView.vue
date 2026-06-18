<template>
  <div class="timeline-view">
    <div v-for="item in sortedItems" :key="item.id" class="timeline-item">
      <time>{{ item.date }}</time>
      <div>
        <strong>{{ item.title }}</strong>
        <p>{{ item.description }}</p>
        <div v-if="item.peopleIds?.length" class="timeline-people">
          <span class="people-label">出镜：</span>
          <PersonTag v-for="pid in item.peopleIds" :key="pid" :member-id="pid" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import PersonTag from './PersonTag.vue'

export interface TimelineItem {
  id: string
  title: string
  description: string
  date: string
  peopleIds?: string[]
}

const props = defineProps<{ items: TimelineItem[] }>()

const sortedItems = computed(() => [...props.items].sort((a, b) => b.date.localeCompare(a.date)))
</script>

<style scoped>
.timeline-people {
  margin-top: 8px;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 4px;
}

.people-label {
  font-size: 12px;
  color: var(--n-text-color-2);
  margin-right: 4px;
}
</style>
