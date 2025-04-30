<script setup lang="ts">
import { ref } from 'vue';
import { NCard, NIcon, NLayout, NLayoutContent, NLayoutSider, NSpace, NText, useMessage } from 'naive-ui';
import { ChevronDown } from '@vicons/ionicons5';

const message = useMessage();

// 菜单数据结构
interface MenuItem {
  label: string;
  key: string;
  children?: MenuItem[];
  isExpanded?: boolean;
}

// 菜单数据
const menuItems = ref<MenuItem[]>([
  {
    label: '常见IDE介绍',
    key: 'ide-intro'
  },
  {
    label: '北京必去景点',
    key: 'beijing-spots'
  },
  {
    label: '项目',
    key: 'projects',
    isExpanded: true,
    children: [
      {
        label: 'Agent收费方案',
        key: 'agent-pricing',
        isExpanded: true,
        children: [
          {
            label: '订阅制收费方案',
            key: 'subscription-pricing'
          }
        ]
      },
      {
        label: '番茄土豆炖牛腩配菜',
        key: 'beef-stew',
        isExpanded: true,
        children: [
          {
            label: '牛肉的选择',
            key: 'beef-selection'
          },
          {
            label: '番茄的用量',
            key: 'tomato-amount'
          },
          {
            label: '把土豆炖化的方法',
            key: 'potato-cooking'
          },
          {
            label: '香料的种类',
            key: 'spices'
          }
        ]
      }
    ]
  }
]);

// 选中的菜单项
const selectedKey = ref<string | null>(null);

// 处理展开按钮点击
function handleExpandClick(item: MenuItem, event: MouseEvent) {
  event.stopPropagation();
  if (item.children) {
    item.isExpanded = !item.isExpanded;
  }
}

// 处理菜单项点击
function handleItemClick(item: MenuItem, event: MouseEvent) {
  event.stopPropagation();
  if (item.children) {
    // 如果是项目或项目实例，显示提示消息并选中
    message.info(`点击了: ${item.label}`);
    selectedKey.value = item.key;
  } else {
    // 如果是对话实例，选中该项
    selectedKey.value = item.key;
    message.info(`选择了: ${item.label}`);
  }
}
</script>

<template>
  <div class="h-full">
    <NLayout has-sider class="h-full">
      <!-- 侧边栏 -->
      <NLayoutSider bordered :width="280" :native-scrollbar="false" class="h-full">
        <div class="h-full flex flex-col select-none">
          <!-- 菜单内容 -->
          <div class="flex-1 overflow-y-auto py-2">
            <div class="px-0">
              <template v-for="item in menuItems" :key="item.key">
                <div class="w-full">
                  <div
                    class="menu-item mx-2 h-10 flex cursor-pointer items-center rounded px-4 pl-4"
                    :class="{ selected: selectedKey === item.key }"
                    @click="handleItemClick(item, $event)"
                  >
                    <span class="flex-1 truncate text-sm leading-normal">{{ item.label }}</span>
                    <span
                      v-if="item.children"
                      class="expand-button ml-2 h-6 w-6 flex cursor-pointer items-center justify-center rounded p-1.5 text-xs"
                      @click="handleExpandClick(item, $event)"
                    >
                      <NIcon
                        :component="ChevronDown"
                        class="text-base transition-transform duration-200"
                        :class="{ 'transform rotate-180': item.isExpanded }"
                      />
                    </span>
                  </div>
                  <div v-if="item.children && item.isExpanded" class="overflow-hidden transition-all duration-300">
                    <template v-for="child in item.children" :key="child.key">
                      <div class="w-full">
                        <div
                          class="menu-item mx-2 h-10 flex cursor-pointer items-center rounded px-4 pl-8"
                          :class="{ selected: selectedKey === child.key }"
                          @click="handleItemClick(child, $event)"
                        >
                          <span class="flex-1 truncate text-sm leading-normal">{{ child.label }}</span>
                          <span
                            v-if="child.children"
                            class="expand-button ml-2 h-6 w-6 flex cursor-pointer items-center justify-center rounded p-1.5 text-xs"
                            @click="handleExpandClick(child, $event)"
                          >
                            <NIcon
                              :component="ChevronDown"
                              class="text-base transition-transform duration-200"
                              :class="{
                                'transform rotate-180': child.isExpanded
                              }"
                            />
                          </span>
                        </div>
                        <div
                          v-if="child.children && child.isExpanded"
                          class="overflow-hidden transition-all duration-300"
                        >
                          <template v-for="grandChild in child.children" :key="grandChild.key">
                            <div
                              class="menu-item mx-2 h-10 flex cursor-pointer items-center rounded px-4 pl-12"
                              :class="{
                                selected: selectedKey === grandChild.key
                              }"
                              @click="handleItemClick(grandChild, $event)"
                            >
                              <span class="flex-1 truncate text-sm leading-normal">{{ grandChild.label }}</span>
                            </div>
                          </template>
                        </div>
                      </div>
                    </template>
                  </div>
                </div>
              </template>
            </div>
          </div>
        </div>
      </NLayoutSider>

      <!-- 主内容区域 -->
      <NLayoutContent class="h-full">
        <div class="h-full flex items-center justify-center">
          <NCard class="w-2/3">
            <NSpace vertical align="center" class="py-8">
              <NText class="text-2xl">欢迎使用 SpiritAI Copilot</NText>
              <NText depth="3" class="text-center">这是一个智能助手，可以帮助你完成各种任务。</NText>
            </NSpace>
          </NCard>
        </div>
      </NLayoutContent>
    </NLayout>
  </div>
</template>

<style scoped>
:deep(.n-layout-sider) {
  background-color: var(--n-color);
  border-right: 1px solid var(--n-border-color);
  user-select: none;
}

:deep(.n-layout-content) {
  background-color: var(--n-color);
}

/* 菜单项基础样式 */
.menu-item {
  --uno: relative flex items-center gap-8px px-12px py-4px rounded-6px cursor-pointer transition-all-300 mb-[0.375rem];

  &:hover {
    background-color: #f3f3f5;
  }

  &.selected {
    background-color: #fef1e7;
    color: rgb(var(--primary-color));
    --uno: font-medium;
  }
}

.expand-button {
  --uno: p-1.5 w-6 h-6 rounded-4px transition-colors-300;

  &:hover {
    background-color: #f3f3f5;
  }

  .icon {
    --uno: text-base transition-transform-300;
  }
}

/* 选中状态下的展开/收起按钮 */
.menu-item.selected .expand-button {
  &:hover {
    background-color: #fef1e7;
  }
}
</style>
