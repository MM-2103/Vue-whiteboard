<template>
	<color-picker v-bind="color" @input="onInput"></color-picker>
</template>

<script>
import { reactive } from 'vue';
import ColorPicker from '@radial-color-picker/vue-color-picker';

export default {
	components: { ColorPicker },
	setup() {
		const color = reactive({
			hue: 50,
			saturation: 100,
			luminosity: 50,
			alpha: 1,
		});

		return {
			color,
			onInput(hue) {
				color.hue = hue;
			},
		};
	},
};
</script>

<style>
@import '@radial-color-picker/vue-color-picker/dist/vue-color-picker.min.css';
</style>
