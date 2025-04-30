<script setup lang="ts">
import { computed, ref } from 'vue';
import { useAppStore } from '@/store/modules/app';
import CurrentDialog from './modules/current-dialog.vue';
import HistoryDialog from './modules/history-dialog.vue';
import OptionsDialog from './modules/options-dialog.vue';

import SendMessage from './modules/send-message.vue';

const appStore = useAppStore();

const gap = computed(() => (appStore.isMobile ? 0 : 16));

</script>

<template>
  <NSpace vertical :size="16">
    <NCard :bordered="false" class="card-wrapper">
      <NGrid :x-gap="gap" :y-gap="16" responsive="screen" item-responsive>
        <NGi span="24 s:24 m:4">
          <NButton quaternary class="w-full flex justify-start" type="primary">
            <template #icon>
              <icon-ic-round-plus class="text-icon" />
            </template>
            新聊天
          </NButton>
          <NButton quaternary class="w-full flex justify-start" type="primary">
            <template #icon>
              <NIcon>
                <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 20 20"><g fill="none"><path d="M4 4a2 2 0 0 1 2-2h3.586a1.5 1.5 0 0 1 1.06.44l3.915 3.914A1.5 1.5 0 0 1 15 7.414V14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V4zm2-1a1 1 0 0 0-1 1v10a1 1 0 0 0 1 1h7a1 1 0 0 0 1-1V8h-3.5A1.5 1.5 0 0 1 9 6.5V3H6zm4 .207V6.5a.5.5 0 0 0 .5.5h3.293L10 3.207zM17 9a1 1 0 0 0-1-1v6a3 3 0 0 1-3 3H6a1 1 0 0 0 1 1h6.06A3.94 3.94 0 0 0 17 14.06V9z" fill="currentColor"></path></g></svg>
              </NIcon>
            </template>
            知识库
          </NButton>
          <NScrollbar class="h-60vh">
            <HistoryDialog />
          </NScrollbar>
        </NGi>
        <NGi span="24 s:24 m:20">
          <div class="h-80vh relative">
            <OptionsDialog />
            <NScrollbar class="h-60vh">
              <div class="flex-y-center">
                <CurrentDialog />
              </div>
            </NScrollbar>

            <SendMessage class="absolute bottom-0 left-0" />

          </div>
        </NGi>
      </NGrid>
    </NCard>

  </NSpace>
</template>

<style scoped></style>
