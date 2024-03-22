<template>
	<div :style="{ background: color }">
		<VueColorPicker theme="light" :color="color" :sucker-hide="false" :sucker-canvas="suckerCanvas"
			:sucker-area="suckerArea" @changeColor="changeColor" />
	</div>
</template>

<script>
import { ColorPicker as VueColorPicker } from 'vue-color-kit'
import 'vue-color-kit/dist/vue-color-kit.css'

export default {
	components: {
		// Registering the imported ColorPicker as VueColorPicker
		VueColorPicker,
	},
	data() {
		return {
			color: '#59c7f9',
			suckerCanvas: null,
			suckerArea: [],
			isSucking: false,
		}
	},
	methods: {
		changeColor(color) {
			const { r, g, b, a } = color.rgba
			this.color = `rgba(${r}, ${g}, ${b}, ${a})`
		},
	},
}
</script>
