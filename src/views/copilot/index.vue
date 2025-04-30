<script setup lang="ts">
import { ref } from 'vue';
import { NCard, NLayout, NLayoutContent, NLayoutSider, NSpace, NText, useMessage } from 'naive-ui';

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
    isExpanded: false,
    children: [
      {
        label: 'Agent收费方案',
        key: 'agent-pricing',
        isExpanded: false,
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
        isExpanded: false,
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
    if (item.isExpanded) {
      message.info(`展开了: ${item.label}`);
    }
  }
}

// 处理菜单项点击
function handleItemClick(item: MenuItem, event: MouseEvent) {
  event.stopPropagation();
  if (item.children) {
    // 如果是项目或项目实例，显示提示消息
    message.info(`点击了: ${item.label}`);
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
        <div class="h-full flex flex-col">
          <!-- 菜单内容 -->
          <div class="menu-content">
            <div class="menu-list">
              <template v-for="item in menuItems" :key="item.key">
                <div class="menu-item-container">
                  <div
                    class="menu-item"
                    :class="[{ selected: selectedKey === item.key }]"
                    :style="{ paddingLeft: '16px' }"
                    @click="handleItemClick(item, $event)"
                  >
                    <span class="menu-item-label">{{ item.label }}</span>
                    <span v-if="item.children" class="menu-item-arrow" @click="handleExpandClick(item, $event)">
                      {{ item.isExpanded ? '▼' : '▶' }}
                    </span>
                  </div>
                  <div v-if="item.children && item.isExpanded" class="menu-children">
                    <template v-for="child in item.children" :key="child.key">
                      <div class="menu-item-container">
                        <div
                          class="menu-item"
                          :class="[{ selected: selectedKey === child.key }]"
                          :style="{ paddingLeft: '32px' }"
                          @click="handleItemClick(child, $event)"
                        >
                          <span class="menu-item-label">{{ child.label }}</span>
                          <span v-if="child.children" class="menu-item-arrow" @click="handleExpandClick(child, $event)">
                            {{ child.isExpanded ? '▼' : '▶' }}
                          </span>
                        </div>
                        <div v-if="child.children && child.isExpanded" class="menu-children">
                          <template v-for="grandChild in child.children" :key="grandChild.key">
                            <div
                              class="menu-item"
                              :class="[{ selected: selectedKey === grandChild.key }]"
                              :style="{ paddingLeft: '48px' }"
                              @click="handleItemClick(grandChild, $event)"
                            >
                              <span class="menu-item-label">{{ grandChild.label }}</span>
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
.h-full {
  height: 100%;
}

.menu-content {
  flex: 1;
  overflow-y: auto;
}

.menu-list {
  padding: 8px 0;
}

.menu-item-container {
  width: 100%;
}

.menu-item {
  display: flex;
  align-items: center;
  height: 40px;
  padding: 0 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.menu-item:hover {
  background-color: var(--n-item-color-hover);
}

.menu-item.selected {
  background-color: var(--n-item-color-active);
  color: var(--n-item-text-color-active);
}

.menu-item-label {
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.menu-item-arrow {
  margin-left: 8px;
  font-size: 12px;
  color: var(--n-text-color-secondary);
  cursor: pointer;
  padding: 4px;
}

.menu-children {
  overflow: hidden;
}

:deep(.n-layout-sider) {
  background-color: var(--n-color);
}
</style>
