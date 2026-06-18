<template>
  <n-tag v-if="member" class="person-tag" :bordered="false" size="small" @click="goToMember">
    <n-avatar :size="16" :circle="true">
      <template #default>{{ member.name.charAt(0) }}</template>
    </n-avatar>
    <span class="person-name">{{ member.name }}</span>
  </n-tag>
  <n-tag v-else class="person-tag unknown" :bordered="false" size="small">
    <span>未知成员</span>
  </n-tag>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useRouter } from 'vue-router'
import { useFamily } from '@/hooks/useFamily'

const props = defineProps<{ memberId: string }>()
const router = useRouter()
const { getById } = useFamily()

const member = computed(() => getById(props.memberId))

function goToMember() {
  if (member.value) {
    router.push({ name: 'member-detail', params: { id: member.value.id } })
  }
}
</script>

<style scoped>
.person-tag {
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 4px;
  margin-right: 4px;
  margin-bottom: 4px;
  transition: all 0.2s ease;
}

.person-tag:hover {
  opacity: 0.8;
  transform: translateY(-1px);
}

.person-tag.unknown {
  opacity: 0.5;
  cursor: default;
}

.person-name {
  font-size: 12px;
}
</style>
