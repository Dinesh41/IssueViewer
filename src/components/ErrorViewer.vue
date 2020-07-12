<template>
  <div style="margin:10px">
    <section>
      <div style="text-align:right;">
        <b-checkbox
          v-for="(column, index) in columns"
          :key="index"
          v-model="column.visible"
        >{{ column.label }}</b-checkbox>
      </div>
      <b-table
        :data="errorLogs"
        :columns="columns"
        :selected.sync="selected"
        :mobile-cards="true"
        :loading="loading"
        defaultSortDirection="asc"
        sortIcon="arrow-up"
        sortIconSize="is-small"
        :paginated="true"
        per-page="10"
      >
        <template slot="empty">
          <div class="has-text-grey has-text-centered">
            <p>
              <i class="far fa-sad-cry" style="font-size:50px"></i>
            </p>
            <p>Nothing here.</p>
          </div>
        </template>
      </b-table>
    </section>
  </div>
</template>
                
<script>
export default {
  props: ["data", "loading"],
  data() {
    return {
      errorLogs: this.data,
      columns: [
        {
          field: "timestamp",
          label: "Date",
          searchable: true,
          sortable: true,
          visible: true
        },
        {
          field: "error",
          label: "Error",
          searchable: true,
          visible: true
        },
        {
          field: "description",
          label: "Description",
          searchable: true,
          visible: true
        }
      ],
      selected: this.data[0]
    };
  }
};
</script>
<style >
</style>