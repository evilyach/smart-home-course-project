<template>
  <div class="q-pa-sm">
    <q-table
      title="Контроллируемые учетной записью устройства"
      :data="data"
      :columns="columns"
    >
      <template v-slot:body-cell-name="props">
        <q-td :props="props">
          <div>
            <q-badge color="$primary" :label="props.value" />
          </div>
          <div class="my-table-details">
            {{ props.row.details }}
          </div>
        </q-td>
      </template>
    </q-table>
  </div>
</template>

<script>
import axios from "axios";
import router from "../../router/routes-backend";

export default {
  data() {
    return {
      columns: [
        {
          name: "device_id",
          align: "center",
          label: "ID устройства",
          field: row => row.device_id,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: "user_account_id",
          align: "center",
          label: "ID учетной записи",
          field: row => row.user_account_id,
          format: val => `${val}`,
          sortable: true
        }
      ],
      data: [],
      getData() {
        axios
          .get(router.table("devices_by_user_account"))
          .then(res => {
            const data = res["data"];

            data.forEach(element => {
              const device_id = element[0][0];
              const user_account_id = element[0][1];

              this.data.push({
                device_id: device_id,
                user_account_id: user_account_id
              });
            });
          })
          .catch(e => {
            this.$q.notify({
              message: "Не удалось получить доступ к базе данных: " + e,
              color: "negative"
            });
          });
      }
    };
  },
  created() {
    this.getData();
  }
};
</script>

<style>
.my-table-details {
  font-size: 0.85em;
  font-style: italic;
  white-space: normal;
  color: #555;
  margin-top: 4px;
}
</style>
