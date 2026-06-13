<script setup lang="ts">
import type { Topic } from '@/types'
import { TOPIC_EMOJIS } from '@/types'
import { formatDate } from '@/utils/helpers'

defineProps<{
  topic: Topic
  canDelete?: boolean
}>()

const emit = defineEmits<{
  (e: 'delete'): void
  (e: 'pickSameType', type: Topic['type']): void
}>()
</script>

<template>
  <div 
    class="topic-card bg-white rounded-xl p-4 shadow-md hover:shadow-lg transition-all border-l-4 relative overflow-hidden group"
    :style="{ borderLeftColor: topic.color }"
  >
    <div 
      class="absolute top-0 right-0 w-20 h-20 rounded-full -mr-8 -mt-8 opacity-10"
      :style="{ backgroundColor: topic.color }"
    ></div>
    
    <div class="relative z-10">
      <div class="flex justify-between items-start mb-2">
        <span 
          class="px-2 py-1 rounded-lg text-xs font-medium text-white"
          :style="{ backgroundColor: topic.color }"
        >
          {{ TOPIC_EMOJIS[topic.type] }} {{ topic.type }}
        </span>
        
        <button 
          v-if="canDelete"
          class="opacity-0 group-hover:opacity-100 transition-opacity text-gray-400 hover:text-red-500 p-1"
          @click.stop="emit('delete')"
        >
          ✕
        </button>
      </div>
      
      <p class="text-gray-800 font-medium mb-3 leading-relaxed">
        {{ topic.content }}
      </p>
      
      <div class="flex justify-between items-center text-xs text-gray-500">
        <span>
          {{ topic.isAnonymous ? '🎭 匿名' : `👤 ${topic.author}` }}
        </span>
        <span>{{ formatDate(topic.createdAt) }}</span>
      </div>
      
      <div 
        v-if="topic.isFlipped"
        class="absolute inset-0 bg-green-500/10 flex items-center justify-center rounded-xl pointer-events-none"
      >
        <span class="bg-green-500 text-white px-4 py-1 rounded-full text-sm font-medium transform rotate-12">
          ✓ 已聊过
        </span>
      </div>
      
      <div 
        v-if="topic.isFlipped"
        class="absolute bottom-3 right-3 opacity-0 group-hover:opacity-100 transition-opacity"
      >
        <button 
          class="px-3 py-1.5 bg-gradient-to-r from-purple-500 to-pink-500 text-white rounded-full text-xs font-medium hover:opacity-90 transition-opacity flex items-center gap-1 shadow-lg pointer-events-auto"
          @click.stop="emit('pickSameType', topic.type)"
        >
          <span>🎲</span>
          <span>再来一个同款</span>
        </button>
      </div>
    </div>
  </div>
</template>
