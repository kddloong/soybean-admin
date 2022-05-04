<template>
  <div>
    <n-card title="表格" class="h-full shadow-sm rounded-16px">
      <n-space :vertical="true">
        <n-space>
          <n-button @click="getDataSource">有数据</n-button>
          <n-button @click="getEmptyDataSource">空数据</n-button>
        </n-space>
        <loading-empty-wrapper class="h-480px" :loading="loading" :empty="empty">
          <n-data-table :columns="columns" :data="dataSource" :flex-height="true" class="h-480px" />
        </loading-empty-wrapper>
      </n-space>
      <n-space align="center" size="large" class="pt-18px">
        <div>表格详情参数id：</div>
        <n-input v-model:value="tableDetailId" />
        <n-button @click="handleToTableDetail">跳转表格详情</n-button>
      </n-space>
    </n-card>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import type { DataTableColumn } from 'naive-ui';
import { routeName } from '@/router';
import { useRouterPush } from '@/composables';
import { useLoadingEmpty } from '@/hooks';
import { getRandomInteger } from '@/utils';

interface DataSource {
  name: string;
  age: number;
  address: string;
}

const { routerPush } = useRouterPush();

const { loading, startLoading, endLoading, empty, setEmpty } = useLoadingEmpty();

const columns: DataTableColumn[] = [
  {
    title: 'Name',
    key: 'name',
    align: 'center'
  },
  {
    title: 'Age',
    key: 'age'
  },
  {
    title: 'Address',
    key: 'address'
  }
];

const dataSource = ref<DataSource[]>([]);

function createDataSource(): DataSource[] {
  return Array(100)
    .fill(1)
    .map((_item, index) => {
      return {
        name: `Name${index}`,
        age: getRandomInteger(30, 20),
        address: '中国'
      };
    });
}

function getDataSource() {
  startLoading();
  setTimeout(() => {
    dataSource.value = createDataSource();
    endLoading();
    setEmpty(!dataSource.value.length);
  }, 1000);
}

function getEmptyDataSource() {
  startLoading();
  setTimeout(() => {
    dataSource.value = [];
    endLoading();
    setEmpty(!dataSource.value.length);
  }, 1000);
}

const tableDetailId = ref('123');

function handleToTableDetail() {
  routerPush({
    name: routeName('component_table-detail'),
    params: { module: tableDetailId.value },
    query: { name: 'soybean' },
    hash: '#app'
  });
}

onMounted(() => {
  getDataSource();
});
</script>
<style scoped></style>
