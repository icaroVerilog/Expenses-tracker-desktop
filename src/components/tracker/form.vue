<template>
	<div id="new-profit-expense-div" :style="[this.newTransactionState ? {'display': 'flex'} : {'display': 'none'}]" >
		<div id="main-div2">
				<div id="textfield-div">
					<v-text-field
						label="Value"
						prefix = "$"
						outlined
						type="number"
						@input="handleTextfieldChange"
						ref="value"
					></v-text-field>
					<v-text-field
						label="Description"
						prefix = ""
						outlined
						type="text"
						@input="handleDescriptionChange"
						ref="value"
					></v-text-field>
					<v-select
						label="Type"
						data-vv-name=""
						outlined
						required
						:items="selectorItems"
						@input="handleSelectorChange"
					></v-select>
<!--					<v-select-->
<!--						v-model="select"-->
<!--						:items="items"-->
<!--						:error-messages="errors"-->
<!--						label="Select"-->
<!--						data-vv-name="select"-->
<!--						outlined-->
<!--						required-->
<!--					></v-select>-->
					<v-text-field
						label="Label Text"
						value="1200-30-11"
						type="date"
						@input="handleDataChange"
						outlined
					></v-text-field>
					<v-checkbox
						:label="`Checkbox 1: ${formData.futureDate.toString()}`"
						@change="handleCheckboxChange"

					></v-checkbox>
				</div>
			<div id="btn-div">
				<v-btn
					x-large
					color="#E05F5F"
					v-on:click="handleCancelBtnClick"
				>
					<span>Cancel</span>
				</v-btn>
				<v-btn
					x-large
					color="#38C183"
					v-on:click="handleSubmitBtnClick"
				>
					<span>Confirm</span>
				</v-btn>
			</div>
		</div>
	</div>


</template>

<script>

	import DatabaseAPI from "../../persistence/databaseAPI";

	export default {
		name: 'ExpenseProfitForm',
		data: () => ({
			newTransactionState: false,
			futureFinancialPosting: true,
			formData: {
				futureDate: false,
				value: 0,
				type: "",
				isExpense: false,
				description: "",
				date: {
					day: 0,
					month: 0,
					year: 0
				}
			},
			selectorItems: ["Expense", "Profit"],
			valueError: false,
			typeError: false,
			dateError: false,
		}),

		mounted () {

		},

		methods: {
			handleCancelBtnClick: function () {
				this.setNewTransactionState(false)
			},
			handleSubmitBtnClick: function () {
				/*
					these group of conditional statement check if the input fiels have only the expected behavior
				*/

				if (this.formData.value === 0 || this.formData.value < 1){
					this.valueError = true
				}
				else if (this.formData.type === "") {
					this.typeError = true
				}
				else if (this.formData.date.day === 0 || this.formData.date.month === 0 || this.formData.date.year === 0){
					this.dateError = true
				}

				else {

					const databaseAPI = new DatabaseAPI()

					console.log("tipo")
					console.log(this.formData.type)

					if (this.formData.type === "Expense"){
						this.isExpense = true;
					}
					else {
						this.isExpense = false
					}

					databaseAPI.addTransaction(
						this.formData.value,
						this.formData.type,
						this.formData.description,
						this.formData.date,
						this.formData.date,
						this.isExpense
					)
				}

				this.setNewTransactionState(false)
				this.$parent.refreshData()
				this.$parent.refreshList()

			},
			setNewTransactionState: function (value) {
				this.newTransactionState = value
			},

			handleTextfieldChange: function (value) {
				this.formData.value = value
				// console.log(this.formData.value)
			},
			handleDescriptionChange: function (value) {
				this.formData.description = value
			},
			handleSelectorChange: function (value) {
				this.formData.type = value
				// console.log(this.formData.type)
			},
			handleDataChange: function (value) {
				let [year, month, day] = value.toString().split("-")

				this.formData.date.day = day
				this.formData.date.month = month
				this.formData.date.year = year

			},
			handleCheckboxChange: function (event) {
				this.formData.futureDate = event
			},

		},
	}
</script>

<style>
	#main-div2 {
		width: 100%;
		height: 100%;
		/*background-color: #00bcd4;*/
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		border-radius: 5px;
	}



	#btn-div {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 50%;
		height: 10%;
		background-color: #2B2E44;
	}

	/**/

	#textfield-div {
		padding-top: 5vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color:#2E3247;
		width: 50%;
		opacity: 1;
	}


	.v-text-field {
		width: 50%;

	}

	.v-label {
		color: white;
	}

	.v-text-field :hover{

	}

	.v-btn {
		width: 20%;
		height: 70%;
		border-radius: 4px;
		margin-left: 3%;
		margin-right: 3%;
		margin-top: 1vh;
		margin-bottom: 1vh;
	}

	#new-profit-expense-div {
		display: flex;
		align-items: center;
		justify-content: center;
		position: absolute;
		width: 100%;
		height: 100%;
		/*background-color: #2B2E44;*/
		/*background-color: rgb(0,0,0); !* Fallback color *!*/
		/*background-color: rgba(0,0,0, 0.1); !* Black w/opacity/see-through *!*/

	}

</style>