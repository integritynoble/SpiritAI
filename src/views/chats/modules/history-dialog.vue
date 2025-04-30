<script setup lang="ts">
import { computed, ref } from 'vue';

// 接收父组件传入的消息对象 { role: 'user'|'assistant', content: string }
const props = defineProps({
  role: String,
  content: String
});

defineOptions({
  name: 'HistoryDialog'
});

interface contentProps {
  title: string;
  value: string;
}

interface HistoryDialogProps {
  id: number;
  value: string;
  content: Array<contentProps>;
  img: string;
  imgPlacement: string; // 'left' | 'right'
}

const historyDialogData = computed<HistoryDialogProps[]>(() => [
  {
    id: 0,
    value: '请分析一下巴菲特的投资理念',
    img: '@/assets/imgs/soybean.jpg',
    imgPlacement: 'right',
    content:[]
  },
  {
    id: 1,
    value: '当然可以，巴菲特的投资理念是价值投资的代表之一，具有很强的逻辑性和实践指导性。以下是他的主要投资理念分析：',
    img: '@/assets/imgs/soybean.jpg',
    imgPlacement: 'left',
    content:[
      {
        title: '1. 价值投资（Value Investing）',
        value: '巴菲特深受本杰明·格雷厄姆的影响，坚信“以低于内在价值的价格买入优质资产”。核心思想是：\n 市场价格 ≠ 企业价值 \n 投资应该在“价格低于价值”的时候出手 \n 关注企业长期盈利能力，而非短期市场波动'
      },
      {
        title: '2. 护城河理论（Economic Moat）',
        value: '巴菲特偏好那些具有长期竞争优势的公司，即“护城河”深的企业。这些优势包括：\n 品牌（如可口可乐）\n 网络效应（如苹果生态系统）\n 成本优势（如沃尔玛）高转换成本（客户不容易更换供应商）'
      },
      {
        title: '3. 理解业务（Invest in What You Understand）',
        value: '他坚持只投资自己“看得懂”的业务。他认为，投资者不必“无所不知”，但必须对所投资的公司业务模式有深入理解。\n “我只投资那些即使傻瓜也能经营的公司，因为有一天可能真的由傻瓜来经营。”'
      },
      {
        title: '4. 长期持有（Buy and Hold）',
        value: '巴菲特的投资策略是长期持有优质股票。他认为，短期市场波动是不可预测的，但长期来看，优质公司的价值会反映在股价上。\n “如果你不打算持有一只股票十年，那么连十分钟都不要持有它。”'
      },
      {
        title: '5. 现金流和盈利能力（Cash Flow and Profitability）',
        value: '巴菲特非常重视公司的现金流和盈利能力。他认为，企业的真正价值在于其产生现金流的能力，而不仅仅是账面利润。\n “我们喜欢那些能产生大量现金流的公司。”'
      }
    ]
  }
]);

</script>

<template>
  <div v-for="item in historyDialogData" :key="item.id">
    <NButton quaternary class="w-full flex justify-start">
      <span class="ellipsis-text">
        {{ item.value }}
      </span>
    </NButton>
  </div>
</template>

<style scoped>
.ellipsis-text {
  display: inline-block;    /* 或者 block */
  width: 100%;              /* 占满按钮宽度 */
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>
