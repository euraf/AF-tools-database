<script>
module.exports = {
  name: "checkbox",
  props: [ 'filter', 'search', 'tools' ],
	data() {
		return {
		}
	},
	computed: {
		value: {
			get() {
				return this.search[this.filter.id]
			},
			set(newValue) {
				this.$set(this.search, this.filter.id, newValue)
				VueBus.$emit('updateFilteredTools')
			}
		},
		cleanedAnwers() {
			return this.removeOther(this.filter.answers)
		}
	},
	methods: {
		removeOther(arr) {
			if (arr[arr.length-1] == "Other...") {
				return arr.slice(0, -1) //this.orderByFrequency(arr.slice(0, -1))
			} else {
				return arr //this.orderByFrequency(arr)
			}
		},
		/*orderByFrequency(arr) { // does not work properly with v-for, needs review
			return arr.sort((a, b) => this.numberAvailable(a) > this.numberAvailable(b) ? -1 : 1)
		},*/
		numberAvailable(answer) {
			var filter_id = this.filter.id

			return this.tools.filter(function(tool) {
				var toolAnswer = tool[filter_id]
				if (Array.isArray(toolAnswer)) {
					return toolAnswer.includes(answer)
				} else {
					return tool[filter_id] == answer
				}
			}).length
		}
	}
}
</script>

<template>
	<div>
		<div class="form-check" v-for="(answer, index) in cleanedAnwers" :key="index">
			<input class="form-check-input" type="checkbox" :disabled="numberAvailable(answer) == 0"
				:value ="answer"
				v-model="value">
			<label class="form-check-label">{{ answer }} ({{ numberAvailable(answer) }})</label>
		</div>
	</div>
</template>