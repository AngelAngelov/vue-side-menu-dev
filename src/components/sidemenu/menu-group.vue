<template>
	<div class="body-group">
		<div class="content" @click="toggle">
			<i :class="`fa ${icon} content-icon`"></i>
			<span>{{title}}</span>
			<div class="spacer"></div>
			<span class="expander fa fa-chevron-down test" :class="{rotate: expanded}"></span>
		</div>
		<transition name="children">
			<div class="children" v-show="expanded">
				<slot name="children"></slot>
			</div>
		</transition>
	</div>
</template>

<script>
import MenuItem from './menu-item.vue';
export default {
	name: 'menuGroup',
	components: {
		MenuItem
	},
	props: {
		icon: { type: String, default: '' },
		title: { type: String, default: '' }
	},
	data() {
		return {
			expanded: false
		};
	},
	methods: {
		toggle() {
			this.expanded = !this.expanded;
		}
	},
	computed: {
		chevronClass() {
			const suffix = this.expanded ? 'up' : 'down';
			return `fa-chevron-${suffix}`;
		}
	},
	watch: {
		expanded: function(val) {
			if (this.$parent.$options.name == 'menuGroup') {
				this.$parent.expanded = true;
			}
		}
	}
};
</script>

<style scoped lang="scss">
.body-group {
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
		.expander {
			float: right;
			transition: transform 0.3s ease-in-out;
		}

		.rotate {
			transform: rotate(180deg);
		}
	}

	.spacer {
		padding: 0;
		margin: 0;
		-webkit-box-flex: 1 !important;
		-ms-flex-positive: 1 !important;
		flex-grow: 1 !important;
		background: transparent;
	}

	.children {
		overflow: hidden;
		width: calc(100% - 1em);
		margin-left: 1em;
		border-left: 1px solid lightgrey;
	}

	.children-enter-active,
	.children-leave-active {
		transition: all 0.3s;
		max-height: 500px;
	}
	.children-enter, .children-leave-to, /* .fade-leave-active below version 2.1.8 */ {
		max-height: 0px;
	}
}
</style>
