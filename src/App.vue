<template>
  <header-nav :totalIncome="state.totalIncome" />
  <form-input :state="state" @addIncome="AddIncome" />
  <income-list :state="state" @remove-item="removeItem" />
</template>

<script>
import { reactive } from "@vue/reactivity";
import HeaderNav from "./components/HeaderNav.vue";
import { computed } from "@vue/runtime-core";
import FormInput from "./components/FormInput.vue";
import IncomeList from "./components/IncomeList.vue";

export default {
  name: "App",
  components: { HeaderNav, FormInput, IncomeList },

  setup() {
    const state = reactive({
      income: [],
      totalIncome: computed(() => {
        let temp = 0;
        if (state.income.length > 0) {
          for (let i = 0; i < state.income.length; i++) {
            temp += state.income[i].value;
          }
        }
        return temp;
      }),

      // Sort income by date
      sortedIncome: computed(() => {
        let temp = [];

        temp = state.income.sort(function (a, b) {
          return b.date - a.date;
        });
        // temp = state.income.sort((a, b) => {
        //   return new Date(b.date) - new Date(a.date);
        // });

        return {
          temp,
        };
      }),
    });

    function AddIncome(data) {
      let d = data.date.split("-");
      let newDate = new Date(d[0], d[1], d[2]);
      state.income = [
        ...state.income,
        {
          id: Date.now(),
          desc: data.desc,
          value: parseInt(data.value),
          date: newDate.getTime(),
        },
      ];
    }

     function removeItem(id) {
      state.income = state.income.filter(v => v.id != id);
    }

    return {
      state,
      AddIncome,
      FormInput,
      HeaderNav,
      IncomeList,
      removeItem
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

body {
  background: #eee;
}
</style>
