<template>
  <div style="width: 100%; height: 500px">
    <n-data-table
        ref="tableRef"
        :columns="columns"
        :data="data"
        :pagination="pagination"
        :bordered="false"
    />
  </div>
</template>

<script setup lang="ts">
import {h, defineComponent, ref, onMounted} from 'vue'
import { NButton, NDataTable } from 'naive-ui'
import type { DataTableColumns } from 'naive-ui'
import Sortable from "sortablejs";

type Song = {
  no: number
  title: string
  length: string
}

const data: Song[] = [
  { no: 1, title: 'Wonderwall', length: '4:18' },
  { no: 2, title: "Don't Look Back in Anger", length: '4:48' },
  { no: 3, title: '3333', length: '7:27' },
  { no: 4, title: '555', length: '7:27' },
  { no: 5, title: 'Champagne Supernova', length: '7:27' },
  { no: 6, title: 'Champagne Supernova', length: '7:27' },
  { no: 7, title: 'Champagne Supernova', length: '7:27' },
]


const createColumns = ({
                         play
                       }: {
  play: (row: Song) => void
}): DataTableColumns<Song> => {
  return [
    {
      title: 'No',
      key: 'no'
    },
    {
      title: 'Title',
      key: 'title'
    },
    {
      title: 'Length',
      key: 'length'
    },
    {
      title: 'Action',
      key: 'actions',
      render (row) {
        return h(
            NButton,
            {
              strong: true,
              tertiary: true,
              size: 'small',
              onClick: () => play(row)
            },
            { default: () => 'Play' }
        )
      }
    }
  ]
}


const pagination = ref(false)
const columns = ref(createColumns({
  play (row: Song) {
    data.push({ no: data.length + 1, title: 'Champagne Supernova', length: '7:27' })
    console.log(`Play ${row.title}`)
  }
}))

const tableRef = ref<InstanceType<typeof NDataTable>>()

onMounted(() => {
  const el: HTMLDivElement = tableRef.value?.$el
  console.log('el', el)
  const tbody: HTMLElement = el.querySelector('tbody.n-data-table-tbody')!
  const sorter = Sortable.create(tbody, {
    onEnd(evt) {
      const oldElem = data[evt.oldIndex!]
      data.splice(evt.oldIndex!, 1)
      data.splice(evt.newIndex!, 0, oldElem)
      console.log('drag end', evt.oldIndex, evt.newIndex, data)
    },
  })
  console.log('sorter', sorter)
})
</script>

<style scoped>

</style>
