<template>
	<transition
		name="toggle"
		enter-active-class="animated fadeIn faster"
		leave-active-class="animated fadeOut faster"
		v-on:after-enter="onToggle"
		v-on:after-leave="onToggleOut"
	>
		<div class="side-menu-wrapper" :class="{'full-size-menu': mode === 'full'}" v-show="value">
			<div class="sidemenu-overlay" @click="onClose"></div>
			<transition
				name="slide"
				:enter-active-class="enterTransition"
				:leave-active-class="exitTransition"
				v-on:after-leave="afterSlideOut"
				v-on:after-enter="onShown"
			>
				<div class="side-menu" :class="{'sm-left': position ==='left', 'sm-right': position ==='right'}" v-show="slideMenu">
					<a v-if="showCloseButton" @click="onClose" href="#" class="close-btn">
						<i class="fa fa-times"></i>
					</a>
					<div class="side-menu-header">
						<slot name="header"></slot>
					</div>
					<div class="side-menu-body">
						<slot name="body"></slot>
					</div>
					<div class="side-menu-spacer"></div>
					<div class="side-menu-footer">
						<slot name="footer"></slot>
					</div>
				</div>
			</transition>
		</div>
	</transition>
</template>

<script>
import bus from './event-bus';
export default {
	name: 'menuBody',
	props: {
		value: Boolean,
		position: {
			type: String,
			default: 'left'
		},
		showCloseButton: {
			type: Boolean,
			default: false
		},
		mode: {
			type: String,
			default: 'full'
		},
		overlay: {
			type: Boolean,
			default: true
		}
	},
	data() {
		return {
			slideMenu: false,
			activeIndex: null
		};
	},
	computed: {
		enterTransition() {
			let sideAmin = 'slideInLeft';
			if (this.position === 'right') {
				sideAmin = 'slideInRight';
			}

			return `animated ${sideAmin} faster`;
		},
		exitTransition() {
			let sideAmin = 'slideOutLeft';
			if (this.position === 'right') {
				sideAmin = 'slideOutRight';
			}

			return `animated ${sideAmin} faster`;
		}
	},
	methods: {
		onClose() {
			this.slideMenu = false;
			this.$emit('onClose');
		},
		onToggle(el, done) {
			this.slideMenu = true;
			this.$emit('onShow');
		},
		onToggleOut() {
			this.$emit('hidden');
		},
		afterSlideOut(el, done) {
			this.$emit('input', false);
		},
		onShown() {
			this.$emit('shown');
		},
		onChildClick() {
			this.onClose();
		},
		setActiveIndex(index) {
			this.activeIndex = index;
			this.bus.$emit('side-menu-index-change', index);
		}
	},
	created() {
		this.bus = bus;
		this.bus.$on('side-menu-set-active', index => {
			this.$emit('menu-item-clicked', index);
			this.setActiveIndex(index);
		});
	},
	beforeDestroy() {
		this.bus.$off('side-menu-set-active');
	}
};
</script>

<style lang="scss" scoped>
a:hover {
	text-decoration: none;
}
.close-btn {
	text-align: right;
	text-decoration: none;
	padding: 5px;
}

.side-menu-wrapper {
	position: absolute;
	top: 0;
	left: 0;
	height: 100vh;
	width: 100vw;
	max-width: 100%;
	max-height: 100%;
	z-index: 2000;
	overflow: hidden;
	&.full-size-menu {
		position: fixed !important;
		z-index: 3000;
	}

	.sidemenu-overlay {
		position: absolute;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.2);
	}

	.side-menu {
		position: absolute;
		border: 1px solid grey;
		max-width: 275px;
		width: 275px;
		background: whitesmoke;
		height: 100%;
		z-index: 3001;
		display: -webkit-box;
		display: -ms-flexbox;
		display: flex;
		-webkit-box-orient: vertical;
		-webkit-box-direction: normal;
		-ms-flex-direction: column;
		flex-direction: column;
		overflow-y: auto;

		&.sm-left {
			left: 0;
		}

		&.sm-right {
			right: 0;
		}

		.side-menu-header {
			border-bottom: 1px solid rgba(0, 0, 0, 0.06);
		}

		.side-menu-spacer {
			padding: 0;
			margin: 0;
			-webkit-box-flex: 1 !important;
			-ms-flex-positive: 1 !important;
			flex-grow: 1 !important;
			background: transparent;
		}

		.side-menu-footer {
			border-top: 1px solid rgba(0, 0, 0, 0.06);
		}
	}
}
</style>
