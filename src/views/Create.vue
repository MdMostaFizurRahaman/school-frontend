<template>
  <div class="bg-gray-100 p-5" v-show="!loading" @submit.prevent="save">
    <form>
      <div class="w-full flex">
        <label for="" class="w-64">Account Name</label>
        <div class="w-full">
          <input
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.account_name"
          />
          <span class="text-red-500" v-if="errors.account_name">{{
            errors.account_name[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Bank</label>
        <div class="w-full">
          <select
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.financial_organization_id"
          >
            <option value="">Select One</option>
            <option v-for="bank in banks" :key="bank.id" :value="bank.id">
              {{ bank.name }}
            </option>
          </select>

          <span class="text-red-500" v-if="errors.financial_organization_id">{{
            errors.financial_organization_id[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Account No</label>
        <div class="w-full">
          <input
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.account_no"
          />
          <span class="text-red-500" v-if="errors.account_no">{{
            errors.account_no[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Branch</label>
        <div class="w-full">
          <input
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.branch"
          />
          <span class="text-red-500" v-if="errors.branch">{{
            errors.branch[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Account Type</label>
        <div class="w-full">
          <select
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.account_type"
          >
            <option value="">Select One</option>
            <option value="1">Savings Account</option>
            <option value="2">Current Account</option>
            <option value="3">Join Account</option>
          </select>
          <span class="text-red-500" v-if="errors.account_type">{{
            errors.account_type[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Swift Code</label>
        <div class="w-full">
          <input
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.swift_code"
          />
          <span class="text-red-500" v-if="errors.swift_code">{{
            errors.swift_code[0]
          }}</span>
        </div>
      </div>

      <div class="w-full flex mt-4">
        <label for="" class="w-64">Route No</label>
        <div class="w-full">
          <input
            type="text"
            class="border w-full border-gray-500 py-2 px-4 rounded-full"
            v-model="formData.route_no"
          />
          <span class="text-red-500" v-if="errors.route_no">{{
            errors.route_no[0]
          }}</span>
        </div>
      </div>

      <div class="w-full mt-5 flex">
        <router-link to="/" class="bg-gray-200 py-2 px-6 rounded-full"
          >Cancel</router-link
        >
        <button
          type="submit"
          class="bg-green-500 text-white px-6 py-2 rounded-full ml-auto hover:bg-green-400"
        >
          Save/Update
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      banks: [],
      errors: [],
      loading: true,
      baseUrl: "http://school.com/api/",
      account: {},
      edit: false,
      formData: {
        account_name: null,
        financial_organization_id: '',
        account_no: null,
        branch: null,
        account_type: '',
        swift_code: null,
        route_no: null,
      },
    };
  },

  methods: {
    save() {
      if (this.edit) {
        axios
          .put(this.baseUrl + "bank-accounts/"+this.$route.params.id, this.formData)
          .then((res) => {
            this.$router.push({ name: "Home" });
          })
          .catch((err) => {
            console.log(err);
            if (err.response.status === 422) {
              this.errors = err.response.data.errors;
            }
          });
      } else {
        axios
          .post(this.baseUrl + "bank-accounts", this.formData)
          .then((res) => {
            this.$router.push({ name: "Home" });
          })
          .catch((err) => {
            console.log(err);
            if (err.response.status === 422) {
              this.errors = err.response.data.errors;
            }
          });
      }
    },
  },
  async mounted() {
    if (this.$route.name == "Edit") {
      const res = await axios.get(
        this.baseUrl + "bank-accounts/" + this.$route.params.id
      );
      console.log(res.data);
      this.formData = res.data;
      this.loading = false;
      this.edit = true;
    }
  },

  async created() {
    const res = await axios.get(this.baseUrl + "financial-organizations");
    // console.log(res);
    this.banks = res.data;
    this.loading = false;
  },
};
</script>

<style lang="scss" scoped>
</style>