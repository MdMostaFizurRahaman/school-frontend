<template>
  <div class="home w-full">
    <div class="flex justify-end">
      <router-link
        to="/create"
        class="bg-green-500 text-white px-6 py-2 rounded-full hover:bg-green-400"
        >Create Account</router-link
      >
    </div>
    <table
      class="rounded-lg overflow-hidden m-5 w-full mx-auto bg-gray-200 text-gray-800"
      v-if="!loading"
    >
      <tr class="text-left border-b-2 border-gray-300">
        <th class="px-4 py-3">Account Name</th>
        <th class="px-4 py-3">Bank</th>
        <th class="px-4 py-3">Account No</th>
        <th class="px-4 py-3">Branch</th>
        <th class="px-4 py-3">Account Type</th>
        <th class="px-4 py-3">Action</th>
      </tr>

      <tr
        class="bg-gray-100 border-b border-gray-200"
        v-for="(account, index) in accounts"
        :key="index"
      >
        <td class="px-4 py-3">{{ account.account_name }}</td>
        <td class="px-4 py-3">{{ account.financial_organization.name }}</td>
        <td class="px-4 py-3">{{ account.account_no }}</td>
        <td class="px-4 py-3">{{ account.branch }}</td>
        <td class="px-4 py-3">{{ accountType(account.account_type) }}</td>
        <td class="px-4 py-3">
          <router-link :to="{name:'Edit', params: {id: account.id}}" class="text-blue-500 cursor-pointer">Edit</router-link> |
          <span class="text-red-500 cursor-pointer" @click="deleteAccount(index, account.id)"
            >Delete</span
          >
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      accounts: [],
      baseUrl: "http://school.com/api/",
      loading: true,
    };
  },

  methods: {
    accountType(type) {
      if (type == "1") {
        return "Saving Account";
      }

      if (type == "2") {
        return "Current Account";
      }

      if (type == "3") {
        return "Join Account";
      }
    },
    deleteAccount(index, id) {
      axios.delete(this.baseUrl + "bank-accounts/" + id).then((res) => {
        this.accounts.splice(index, 1);
      })
      .catch(err => console.log(err));
    },
  },
  async created() {
    const res = await axios.get(this.baseUrl + "bank-accounts");
    // console.log(res);
    this.accounts = res.data;
    this.loading = false;
  },
};
</script>
