<template>
	<div class="body-item" @click="onClick">
		<router-link :to="to" class="content" :class="{active: isActive}">
			<i :class="`fa ${icon} content-icon`"></i>
			<slot></slot>
		</router-link>
	</div>
</template>

<script>
import bus from './event-bus';

export default {
	name: 'menuItem',
	props: {
		to: { type: String, default: '' },
		icon: { type: String, default: '' },
		index: { type: [String, Number], default: null }
	},
	data() {
		return {
			isActive: false
		};
	},
	methods: {
		onClick() {
			this.bus.$emit('side-menu-set-active', this.index);
			this.$emit('clicked');
		}
	},
	created() {
		this.bus = bus;
		this.bus.$on('side-menu-index-change', index => {
			if (index && index == this.index) {
				this.isActive = true;
				if (this.$parent.$options.name == 'menuGroup') {
					this.$parent.expanded = true;
				}
			} else {
				this.isActive = false;
			}
		});
	},
	beforeDestroy() {
		this.bus.$off('side-menu-index-change');
	}
};
</script>

<style scoped lang="scss">
.body-item {
	cursor: pointer;
	color: rgba(0, 0, 0, 0.6);
	&:hover {
		color: #009688;
		text-decoration: none;
	}
	.content {
		width: 100%;
		padding: 5px 10px 5px 10px;
		display: flex;
		-webkit-box-align: center;
		-ms-flex-align: center;
		align-items: center;
		color: inherit !important;
		.content-icon {
			width: 30px;
		}
		&:hover {
			color: #009688 !important;
			border-right: 3px solid #009688;
		}
		&.active {
			color: #009688 !important;
			border-right: 3px solid #009688;
		}
	}
}
</style>
