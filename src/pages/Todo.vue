<template>
  <q-page class="bg-grey-3">
    <div class="q-pa-lg q-gutter-sm">
      <q-btn round color="primary" icon="add" @click="dialog = true" />
    </div>

    <div class="q-pa-lg">
      <q-table
        :dense="$q.screen.lt.md"
        title="Datas"
        :rows="rows"
        :columns="columns"
        no-data-label="I didn't find anything for you"
        row-key="id"
      >
        <template v-slot:header="props">
          <q-tr :props="props">
            <q-th v-for="col in props.cols" :key="col.name" :props="props">
              {{ col.label }}
            </q-th>
            <q-th auto-width />
          </q-tr>
        </template>

        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td v-for="col in props.cols" :key="col.name" :props="props">
              {{ col.value }}
            </q-td>
            <q-td auto-width>
              <q-btn
                size="sm"
                color="cyan"
                round
                dense
                @click="confirmDelete = true"
                icon="edit"
              />
              |
              <q-btn
                size="sm"
                color="red"
                round
                dense
                @click="confirmDelete = true"
                icon="delete"
              />
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </div>

    <div class="flex flex-center">
      <img
        alt="Quasar logo"
        src="~assets/quasar-logo-vertical.svg"
        style="width: 200px; height: 200px"
      />
    </div>

    <q-dialog v-model="dialog" persistent>
      <q-card style="min-width: 450px">
        <q-card-section>
          <div class="text-h6">New Company</div>
        </q-card-section>

        <q-form @submit="handleSubmit()">
          <q-card-section class="q-pt-none">
            <!-- <q-input
              dense
              v-model="address"
              autofocus
              @keyup.enter="prompt = false"
            /> -->
            <q-input dense v-model="form.company" autofocus label="Company" />
            <q-input dense v-model="form.contact" label="Contact" />
            <q-input dense v-model="form.mail" label="Mail" />
            <q-input dense v-model="form.phone" label="Phone" />
            <q-input dense v-model="form.address" label="Address" />
            <q-input dense v-model="form.city" label="City" />
            <q-input dense v-model="form.country" label="Country" />
          </q-card-section>

          <q-card-actions align="right" class="text-primary">
            <q-btn flat label="Cancel" v-close-popup />
            <q-btn
              flat
              label="Add company"
              type="submit"
              @submit="handleSubmit()"
              v-close-popup
            />
          </q-card-actions>
        </q-form>
      </q-card>
    </q-dialog>

    <q-dialog v-model="confirmDelete" persistent>
      <q-card>
        <q-card-section class="row items-center">
          <q-avatar icon="warning_amber" color="primary" text-color="white" />
          <span class="q-ml-sm">Are you sure remove this data?</span>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="Cancel" color="primary" v-close-popup />
          <q-btn flat label="Accept" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { date } from "quasar";

export default defineComponent({
  name: "IndexPage",
  setup() {
    return {
      form,
      columns,
      rows,
      formattedString,
      handleSubmit,

      alert: ref(false),
      confirmDelete: ref(false),
      dialog: ref(false),

      // company: ref(""),
      // contact: ref(""),
      // mail: ref(""),
      // phone: ref(""),
      // address: ref(""),
      // city: ref(""),
      // country: ref(""),
    };
  },
});

// console.log(props);
const timeStamp = Date.now();
const formattedString = date.formatDate(timeStamp, "YYYY-MM-DDTHH:mm");

const form = ref({
  company: "",
  contact: "",
  mail: "",
  phone: "",
  address: "",
  city: "",
  country: "",
});

const columns = [
  {
    name: "id",
    required: true,
    label: "ID",
    align: "left",
    field: (row) => row.id,
    format: (val) => `${val}`,
    sortable: true,
    sort: (a, b) => parseInt(a, 10) - parseInt(b, 10),
  },
  {
    name: "company",
    align: "center",
    label: "Company",
    align: "left",
    field: "company",
    sortable: true,
  },
  {
    name: "contact",
    label: "Contact",
    align: "left",
    field: "contact",
    sortable: true,
  },
  { name: "mail", label: "Mail", align: "left", field: "mail", sortable: true },
  { name: "phone", label: "Phone", align: "left", field: "phone" },
  { name: "city", label: "City", align: "left", field: "city" },
  { name: "country", label: "Country", align: "left", field: "country" },
  {
    name: "date",
    label: "Creation Date",
    field: "date",
    sortable: true,
    sort: (a, b) => parseInt(a, 10) - parseInt(b, 10),
  },
  // {
  //   name: "actions",
  //   label: "Actions",
  //   align: "right",
  //   field: "actions",
  //   sortable: true,
  // },
];

const rows = [
  {
    id: 1,
    company: "Frozen Yogurt",
    contact: "Juan Perez",
    mail: "frozen@mail.com",
    phone: "24242424",
    city: "FL",
    country: "EEUU",
    date: formattedString,
  },
  {
    id: 2,
    company: "Ice cream sandwich",
    contact: "Juan Perez",
    mail: "frozen@mail.com",
    phone: "24242424",
    city: "FL",
    country: "EEUU",
    date: formattedString,
  },
];

const handleSubmit = () => {
  console.log("Hello", form.value.company);
  // form = ref({
  //   company: "",
  // });

  console.log(rows);
  rows.push({
    id: rows.id + 1,
    company: form.value.company,
    contact: form.value.contact,
    mail: form.value.mail,
    phone: form.value.phone,
    city: form.value.city,
    country: form.value.country,
    calcium: "",
  });
  console.log(rows);

  form.value.company = "";
  form.value.contact = "";
  form.value.mail = "";
  form.value.phone = "";
  form.value.address = "";
  form.value.city = "";
  form.value.country = "";
};
</script>
