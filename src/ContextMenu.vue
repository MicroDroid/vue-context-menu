<template>
	<div class="container" ref="container" :style="{
		'display': isOpen ? 'block' : 'none',
		'left': `${position.x}px`,
		'top': `${position.y}px`,
	}">
		<slot :ctx="ctx" />
	</div>
</template>

<script>
	export default {
		data() {
			return {
				isOpen: false,
				position: {
					x: 0,
					y: 0,
				},
				ctx: null,
			};
		},

		methods: {
			open(event, ctx) {
				if (event) {
					this.position.x = event.pageX;
					this.position.y = event.pageY;
				}
				
				this.ctx = ctx;
				this.isOpen = true;
			},

			close() {
				this.isOpen = false;
			},
		},

		mounted() {
			document.addEventListener('click', e => {
				if (!this.$refs.container || !this.isOpen)
					return;

				const insideMenu = this.$refs.container.contains(e.target);

				if (!insideMenu)
					this.isOpen = false;
			});
		}
	};
</script>

<style scoped>
	.container {
		position: absolute;
	}
</style>