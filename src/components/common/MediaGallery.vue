<template>
  <div class="media-gallery">
    <button v-for="item in items" :key="item.id" class="media-card" type="button" @click="active = item">
      <img v-if="item.url" :src="item.url" :alt="item.caption" />
      <div v-else class="photo-placeholder">{{ item.year || '影像' }}</div>
      <span>{{ item.caption }}</span>
      <div v-if="item.peopleIds?.length" class="card-people-tags">
        <PersonTag v-for="pid in item.peopleIds.slice(0, 2)" :key="pid" :member-id="pid" />
        <n-tag v-if="item.peopleIds.length > 2" size="small" :bordered="false">+{{ item.peopleIds.length - 2 }}</n-tag>
      </div>
    </button>
    <n-modal v-model:show="showPreview" preset="card" class="preview-modal" :title="active?.caption">
      <img v-if="active?.url" class="preview-image" :src="active.url" :alt="active.caption" />
      <div v-else class="photo-placeholder large">{{ active?.year || '无图片' }}</div>
      <div v-if="active?.peopleIds?.length" class="people-section">
        <h4 class="people-title">出镜人物</h4>
        <div class="people-tags">
          <PersonTag v-for="pid in active.peopleIds" :key="pid" :member-id="pid" />
        </div>
      </div>
      <template #footer>
        <div class="preview-footer">
          <span>{{ active?.meta }}</span>
          <span v-if="active?.year">{{ active.year }}年</span>
        </div>
      </template>
    </n-modal>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import PersonTag from './PersonTag.vue'

export interface GalleryItem {
  id: string
  url: string
  caption: string
  year?: number
  meta?: string
  peopleIds?: string[]
}

defineProps<{ items: GalleryItem[] }>()

const active = ref<GalleryItem | null>(null)
const showPreview = computed({
  get: () => Boolean(active.value),
  set: (value) => {
    if (!value) active.value = null
  }
})
</script>

<style scoped>
.card-people-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 2px;
  padding: 4px 8px 8px;
}

.people-section {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid var(--n-border-color);
}

.people-title {
  margin: 0 0 8px 0;
  font-size: 14px;
  font-weight: 600;
  color: var(--n-text-color);
}

.people-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
}

.preview-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
