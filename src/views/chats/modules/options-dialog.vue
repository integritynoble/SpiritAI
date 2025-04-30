<script setup lang="ts">
import { computed, ref } from 'vue';
import { useAppStore } from '@/store/modules/app';

defineOptions({
  name: 'OptionsDialog'
});

const appStore = useAppStore();

const gap = computed(() => (appStore.isMobile ? 0 : 16));

const modelOptions = computed(() => [
  {
    label: 'GPT-4o',
    value: 'GPT-4o'
  },
  {
    label: 'DeepSeek R1',
    value: 'DeepSeek R1'
  },
  {
    label: 'GPT-4o mini',
    value: 'GPT-4o mini'
  }
]);

const modelValue = ref<string | null>(null);

modelValue.value = modelOptions.value[0].value;

type ButtonType = 'ai_button' | 'depot_button' | 'people_button';

const buttonStyle = ref({
  ai_button: {
    strong: false,
    secondary: true,
    type: 'primary' as const
  },
  depot_button: {
    strong: true,
    secondary: false,
    type: 'default' as const
  },
  people_button: {
    strong: true,
    secondary: false,
    type: 'default' as const
  },
});

function clickButton(type: String) {
  for (const key in buttonStyle.value) {
    const buttonKey = key as ButtonType;
    if (key === type) {
      buttonStyle.value[buttonKey].strong = false;
      buttonStyle.value[buttonKey].secondary = true;
      buttonStyle.value[buttonKey].type = 'primary';
    }else{
      buttonStyle.value[buttonKey].strong = true;
      buttonStyle.value[buttonKey].secondary = false;
      buttonStyle.value[buttonKey].type = 'default';
    }
  }
}

</script>

<template>
  <div class="flex-y-center">
    <NGrid :x-gap="gap" :y-gap="16" responsive="screen" item-responsive>
      <NGi span="24 s:12 m:16">
        <NSelect class="w-200px" v-model:value="modelValue" :options="modelOptions"></NSelect>
      </NGi>
      <NGi span="24 s:12 m:8" class="flex-y-center justify-end">
        <NButton :quaternary="true"
        :strong="buttonStyle.ai_button.strong"
        :secondary="buttonStyle.ai_button.secondary"
        :type="buttonStyle.ai_button.type"
        @click="clickButton('ai_button')">
          <template #icon>
            <NIcon color="#0e7a0d">
              <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 20 20"><g fill="none"><path d="M6.13 2.793A3.91 3.91 0 0 1 8.5 2a1.757 1.757 0 0 1 1.5.78A1.757 1.757 0 0 1 11.5 2a3.91 3.91 0 0 1 2.37.793c.525.408.93.973 1.073 1.656c.328.025.628.161.88.366c.382.31.66.775.835 1.267c.274.765.348 1.74.064 2.57c.072.034.143.074.212.12c.275.183.484.445.638.754c.303.605.428 1.449.428 2.474c0 1.141-.435 1.907-.987 2.38a2.68 2.68 0 0 1-1.054.555c-.1.558-.38 1.204-.819 1.752C14.57 17.402 13.686 18 12.5 18c-.94 0-1.688-.52-2.174-1.03a4.252 4.252 0 0 1-.326-.385a4.245 4.245 0 0 1-.326.385C9.188 17.48 8.441 18 7.5 18c-1.186 0-2.069-.598-2.64-1.313a4.057 4.057 0 0 1-.819-1.752a2.68 2.68 0 0 1-1.054-.555C2.435 13.907 2 13.14 2 12c0-1.025.126-1.87.428-2.474c.154-.309.363-.57.638-.755a1.58 1.58 0 0 1 .212-.118c-.284-.832-.21-1.806.064-2.571c.175-.492.453-.957.835-1.267c.252-.205.552-.34.88-.366c.144-.683.549-1.248 1.074-1.656zM9.5 4.5V4.49l-.002-.05a2.744 2.744 0 0 0-.154-.764a1.222 1.222 0 0 0-.309-.49A.76.76 0 0 0 8.5 3a2.91 2.91 0 0 0-1.756.582C6.28 3.943 6 4.432 6 5a.5.5 0 0 1-.658.474c-.188-.062-.356-.027-.535.117c-.196.16-.387.444-.524.827c-.279.782-.25 1.729.133 2.305A.5.5 0 0 1 4.5 9h.75a2.25 2.25 0 0 1 2.25 2.25v.335a1.5 1.5 0 1 1-1 0v-.335c0-.69-.56-1.25-1.25-1.25H3.5a.499.499 0 0 1-.175-.032l-.003.006C3.124 10.369 3 11.025 3 12c0 .859.315 1.343.638 1.62c.347.298.732.38.862.38a.5.5 0 0 1 .5.5c0 .368.2 1.011.64 1.563c.429.535 1.046.937 1.86.937c.56 0 1.062-.313 1.45-.72c.191-.2.34-.407.437-.577a1.573 1.573 0 0 0 .113-.236V7.5H8.415a1.5 1.5 0 1 1 0-1H9.5v-2zm1 9.999v.967a1.575 1.575 0 0 0 .113.236c.098.17.246.377.436.577c.389.407.892.72 1.451.72c.814 0 1.431-.402 1.86-.937c.44-.552.64-1.195.64-1.563a.5.5 0 0 1 .5-.5c.13 0 .515-.082.862-.38c.323-.277.638-.761.638-1.62c0-.975-.125-1.63-.322-2.026a.923.923 0 0 0-.3-.37A.657.657 0 0 0 16 9.5a.5.5 0 0 1-.416-.777c.384-.576.412-1.523.133-2.305c-.137-.383-.328-.668-.524-.827c-.179-.144-.347-.18-.535-.117A.5.5 0 0 1 14 5c0-.568-.28-1.057-.745-1.418A2.91 2.91 0 0 0 11.5 3a.76.76 0 0 0-.535.186a1.22 1.22 0 0 0-.31.49a2.579 2.579 0 0 0-.155.814v9.01h.75c.69 0 1.25-.56 1.25-1.25v-1.835a1.5 1.5 0 1 1 1 0v1.835a2.25 2.25 0 0 1-2.25 2.25h-.75zM6.5 7a.5.5 0 1 0 1 0a.5.5 0 0 0-1 0zM13 9.5a.5.5 0 1 0 0-1a.5.5 0 0 0 0 1zm-6 3a.5.5 0 1 0 0 1a.5.5 0 0 0 0-1z" fill="currentColor"></path></g></svg>
            </NIcon>
          </template>
          AI对话
        </NButton>
        <NButton :quaternary="true"
        :strong="buttonStyle.depot_button.strong"
        :secondary="buttonStyle.depot_button.secondary"
        :type="buttonStyle.depot_button.type"
        @click="clickButton('depot_button')">
          <template #icon>
            <NIcon color="#f0a020">
              <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 20 20"><g fill="none"><path d="M4 4a2 2 0 0 1 2-2h3.586a1.5 1.5 0 0 1 1.06.44l3.915 3.914A1.5 1.5 0 0 1 15 7.414V14a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V4zm2-1a1 1 0 0 0-1 1v10a1 1 0 0 0 1 1h7a1 1 0 0 0 1-1V8h-3.5A1.5 1.5 0 0 1 9 6.5V3H6zm4 .207V6.5a.5.5 0 0 0 .5.5h3.293L10 3.207zM17 9a1 1 0 0 0-1-1v6a3 3 0 0 1-3 3H6a1 1 0 0 0 1 1h6.06A3.94 3.94 0 0 0 17 14.06V9z" fill="currentColor"></path></g></svg>
            </NIcon>
          </template>
          知识库
        </NButton>
        <NButton :quaternary="true"
        :strong="buttonStyle.people_button.strong"
        :secondary="buttonStyle.people_button.secondary"
        :type="buttonStyle.people_button.type"
        @click="clickButton('people_button')">
          <template #icon>
            <NIcon color="#2080f0">
              <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 20 20"><g fill="none"><path d="M11 10a2 2 0 0 1 2 2v1.5c0 2.054-2.088 3.5-5.5 3.5S2 15.554 2 13.5V12a2 2 0 0 1 2-2h7zm0 1H4a1 1 0 0 0-1 1v1.5C3 14.907 4.579 16 7.5 16c2.921 0 4.5-1.093 4.5-2.5V12a1 1 0 0 0-1-1zm5-1a2 2 0 0 1 2 2v.5c0 2.089-1.568 3.5-4.5 3.5c-.141 0-.28-.003-.414-.01c.208-.242.382-.502.522-.781l.097-.213h.039C15.976 14.931 17 13.96 17 12.5V12a1 1 0 0 0-1-1h-2.171a3 3 0 0 0-.594-1H16zM7.5 2a3.5 3.5 0 1 1 0 7a3.5 3.5 0 0 1 0-7zm7 2a2.5 2.5 0 1 1 0 5a2.5 2.5 0 0 1 0-5zm-7-1a2.5 2.5 0 1 0 0 5a2.5 2.5 0 0 0 0-5zm7 2a1.5 1.5 0 1 0 0 3a1.5 1.5 0 0 0 0-3z" fill="currentColor"></path></g></svg>
            </NIcon>
          </template>
          好友
        </NButton>
      </NGi>
    </NGrid>
  </div>
</template>

<style scoped>
</style>
