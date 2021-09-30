<template>
<div id="app">
    <h1> {{ title }} </h1>
    <form>
      <fieldset>
        <label>{{ budgetLabel }}</label>
        <input v-model.number="budget" />
      </fieldset>
    </form>

    <form @submit.prevent="add">
      <h2>{{ sectionTitle }}</h2>
      <fieldset>
        <label>{{ descriptionLabel }}</label>
        <input v-model="expense.description" />
 
        <label>{{ costLabel }}</label>
        <input v-model.number="expense.cost" />
      </fieldset>
      <button type="submit">{{ addBtn }}</button>
    </form>

    <p>{{ balanceLabel }} ${{ remainingBudget }}</p>

    <div class="items" v-for="(item, index) of expenses" :key="item.id">
      <h3> {{ item.description }} - ${{ item.cost }} </h3>
      <button class="removeBtn" @click="remove(index)">{{ removeBtn }}</button>
    </div>
    <h6> {{ footer }} </h6>
  </div> 
</template>

<script>
import { v4 as uuidv4 } from "uuid";
export default {
  name: "App",
  data() {
    return {
      title: "Simple Budget Tracker",
      budgetLabel:"Overall Budget",
      sectionTitle:"Add Item",
      descriptionLabel:"Item Description",
      costLabel:"Item Cost",
      addBtn:"Add to Budget",
      balanceLabel:"Remaining Budget:",
      removeBtn:"Remove",
      expense: {
        description: "",
        cost: 0,
      },
      expenses: [],
      budget: 0,
      footer: "Lauren Elliott-Manheim • 2015-2022 • Ex astris, scientia"
    };
  },
  computed: {
    expenseValid() {
      const { cost, description } = this.expense;
      return +cost >= 0 && description.length > 0;
    },
    remainingBudget() {
      const { budget, expenses } = this;
      const totalExpenses = expenses
        .map(({ cost }) => cost)
        .reduce((a, b) => a + b, 0);
      return budget - totalExpenses;
    },
  },
  methods: {
    add() {
      if (!this.expenseValid) {
        return;
      }
      this.expenses.push({
        id: uuidv4(),
        ...this.expense,
      });
      this.expense = {};
    },
    remove(index) {
      this.expenses.splice(index, 1);
    },
  },
};
</script>

<style lang="scss">
$border: 2px solid #938C8E;
$size1: 8px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 36px;
$size6: 48px;
$background:#594F51;
$text:#F2F2F2;
$primary:#F26052;
$accent:#A66249;
body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: $background;
	color: $text;
	#app {
		max-width: 600px;
    margin-top:10px;
		margin-left: auto;
		margin-right: auto;
		padding: 20px;
    h1, h2, h6 {color:$primary;}
    p{ 
      color:$text;
      font-weight:bold;

    }
    h1 {
			font-weight: bold;
			font-size: 28px;
			text-align: center;
      margin-bottom:30px;
		}
    h2{
      font-size:22px;
      margin:40px auto 10px;
      text-align:center;
      }
    h6 {
  		font-weight:400;
			letter-spacing:2.25px;
			text-transform:uppercase;
			padding:16px;
			text-align:center;
		}
    form {
			display: flex;
			flex-direction: column;
			width: 100%;
      fieldset{
        border-radius:$size1;
        border-color:$accent;
        padding:20px 30px;
      }
			label {
				font-size: 14px;
				font-weight: bold;
        display:flex;
        text-indent:6px;
        line-height:15px;
			}
			input,
			button {
				box-shadow: none;
				outline: none;
				padding-left: $size2;
				padding-right: $size2;
				border-radius: $size1;
				font-size: 18px;
				margin-top: $size1;
				margin-bottom: $size2;
			}
			input {
        height: $size5;
				background-color: transparent;
				border: $border;
				color: inherit;
        &:focus {
          background-color:darken($background, 4%);
          transition: all 0.65s ease;
        }
			}
      button {
        height:$size6;
        color:$text;
        background-color: $primary;
        border: none;
        font-weight: bold;
        outline: none;
        border-radius: $size1;
        margin-top:30px;
        &:hover {
          cursor:pointer;
          background-color: lighten($primary, 3%);
          transition: all 0.65s ease;
        }
      }
    }
    .items {
      display:flex;
      h3{
        margin: 10px auto 10px 0;
      }
      .removeBtn{
        height:$size5;
        font-size:18px;
        background-color:transparent;
        outline:none;
        border-radius:$size1;
        color:$text;
        outline:none;
        box-shadow:none;
        border:$border;
        padding-left: $size2;
        padding-right: $size2;
        margin-top:10px;
        margin-bottom:10px;
        &:hover {
          cursor:pointer;
          border-color:$primary;
          background-color:$primary;
          transition: all 0.65s ease;
        }
      }
    }
  }
}
</style>