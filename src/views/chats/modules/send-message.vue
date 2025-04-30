<script setup lang="ts">
import { computed, ref } from 'vue';
import { useAppStore } from '@/store/modules/app';
import { useAuthStore } from '@/store/modules/auth';
import { $t } from '@/locales';

defineOptions({
  name: 'SendMessage'
});

const appStore = useAppStore();
const authStore = useAuthStore();
const sendvalue = ref<string>('');

const gap = computed(() => (appStore.isMobile ? 0 : 16));
const sendMessage = () => {
  // Here you would typically send the message to your server or chat API
  console.log('Sending message:', sendvalue.value);
  // Optionally, you can clear the input field after sending the message
};
</script>

<template>
  <NCard :bordered="false" class="card-wrapper">
    <NGrid :x-gap="gap" :y-gap="16" responsive="screen" item-responsive>
      <NGi span="24 s:24 m:24">
        <div class="flex-y-center">
          <NInput placeholder="请输入..." type="textarea" v-model:value="sendvalue" ></NInput>
        </div>
      </NGi>
    </NGrid>
  </NCard>
</template>

<style scoped></style>
