<template>
  <div class="home">
    <Table/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Table from '@/components/Table.vue'; // @ is an alias to /src

@Component({
  components: {
    Table,
  },
})
export default class Home extends Vue {}
</script>
