<template>
  <cs-container>
    <page-header
      slot="header"
      ref="header"
      :loading="loading"
      @submit="handleSubmit"/>

    <page-main
      :loading="loading"
      :table-data="table"
      @sort="handleSort"/>
  </cs-container>
</template>

<script>
import { getSupportList } from '@/api/aided/support'

export default {
  name: 'system-aided-support',
  components: {
    PageHeader: () => import('./components/PageHeader'),
    PageMain: () => import('./components/PageMain')
  },
  data() {
    return {
      loading: false,
      table: [],
      order: {
        order_type: undefined,
        order_field: undefined
      }
    }
  },
  mounted() {
    this.handleSubmit()
  },
  methods: {
    // 排序刷新
    handleSort(val) {
      this.order = val
      this.$nextTick(() => {
        this.$refs.header.handleFormSubmit()
      })
    },
    // 提交查询
    handleSubmit(form) {
      this.loading = true
      getSupportList({
        ...form,
        ...this.order
      })
        .then(res => {
          this.table = res.data || []
        })
        .finally(() => {
          this.loading = false
        })
    }
  }
}
</script>
