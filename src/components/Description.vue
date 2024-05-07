<script>
import "@/assets/tailwind.scss";
const balancedMessage = "The text is balanced.";
const UnbalancedMessage = "The text is not balanced.";
export default {
	data() {
		return {
			input: "",
			message: "",
		};
	},
	methods: {
		handleInput(event) {
			const val = event.target.value;
			if (val.toString().endsWith("?")) {
				const isBalanced = this.isBalanced(val);
				if (isBalanced) {
					this.message = balancedMessage;
				} else {
					this.message = UnbalancedMessage;
				}
			}
		},
		isBalanced(str) {
			const stack = [];
			const openBrackets = ["(", "{", "["];
			const closeBrackets = [")", "}", "]"];
			for (let i = 0; i < str.length; i++) {
				const char = str[i];
				if (openBrackets.includes(char)) {
					stack.push(char);
				} else if (closeBrackets.includes(char)) {
					const top = stack[stack.length - 1];
					const isTopCharMatch =
						openBrackets.indexOf(top) === closeBrackets.indexOf(char);
					if (isTopCharMatch) {
						stack.pop();
					} else {
						return false;
					}
				}
			}
			if (stack.length === 0) return true;
			return false;
		},
	},
};
</script>
<template>
	<main class="flex flex-col justify-center items-center w-full h-full bg-gray-700 flex-1">
		<input
			class="text-gray-300 bg-gray-700 border border-gray-600 rounded-lg w-full p-3 bg-gray-700 border-gray-600 placeholder-gray-500"
			id="description" v-model="input" @input="handleInput" />
		<span>message: {{ message }}</span>
	</main>
</template>
<style scoped lang="scss">
main {
	display: flex;
	flex-direction: column;
	min-height: 100vh;
	align-items: center;
	justify-content: center;
	padding: 30px;
}
</style>