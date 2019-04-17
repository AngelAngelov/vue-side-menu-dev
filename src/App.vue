<template>
	<div id="app">
		<nav class="navbar navbar-light bg-light">
			<span @click="toggleSidemenu">
				<i class="fas fa-bars"></i>
			</span>
			<span @click="toggleSidemenu2">
				<i class="fas fa-bars"></i>
			</span>
			<a class="navbar-brand" href="#">Navbar</a>
		</nav>
		<side-menu v-model="open" position="left" ref="menu">
			<div slot="header">
				<div class="p-2">
					<router-link to="/account">
						<div class="user-info px-2">
							<span class="fa fa-user-circle fa-2x"></span>
							<span class="username ml-2">Username</span>
						</div>
					</router-link>

					<button class="btn btn-outline-danger btn-noborder">
						<i class="fa fa-reply"></i> logout
					</button>
					<router-link to="/login" class="btn btn-outline-primary btn-noborder">
						<i class="fa fa-sign-in-alt"></i> login
					</router-link>
				</div>
			</div>
			<div slot="body">
				<MenuItem to="/" icon="fa-home" index="7">Home</MenuItem>
				<MenuItem to="/about" icon="fa-calendar-alt" index="8">Planning</MenuItem>
				<MenuGroup icon="fa-chess-board" title="Chess board">
					<div slot="children">
						<MenuItem icon="fa-chess-knight" index="1">Child 1</MenuItem>
						<MenuItem icon="fa-chess-rook" index="2">Child 2</MenuItem>
						<MenuItem icon="fa-chess-pawn" index="3">Child 3</MenuItem>
						<MenuGroup icon="fa-chess-board" title="Chess board">
							<div slot="children">
								<MenuItem icon="fa-chess-knight" index="4">Child 1</MenuItem>
								<MenuItem icon="fa-chess-rook" index="5">Child 2</MenuItem>
								<MenuItem icon="fa-chess-pawn" index="6">Child 3</MenuItem>
							</div>
						</MenuGroup>
					</div>
				</MenuGroup>
				<MenuItem to="/stats" icon="fa-chart-pie" index="9">Statistics</MenuItem>
			</div>
			<div slot="footer">This is the footer</div>
		</side-menu>
		<div class="container p-0" style="width: 50vw !important; height: 50vh !important">
			<select v-model="selected" class="form-control">
				<option value="1">1</option>
				<option value="2">2</option>
				<option value="3">3</option>
				<option value="4">4</option>
				<option value="5">5</option>
			</select>
			<side-menu v-model="open2" position="right" mode="child">
				<div slot="header">
					<div class="p-2">
						<router-link to="/account">
							<div class="user-info px-2">
								<span class="fa fa-user-circle fa-2x"></span>
								<span class="username ml-2">Username</span>
							</div>
						</router-link>

						<button class="btn btn-outline-danger btn-noborder">
							<i class="fa fa-reply"></i> logout
						</button>
						<router-link to="/login" class="btn btn-outline-primary btn-noborder">
							<i class="fa fa-sign-in-alt"></i> login
						</router-link>
					</div>
				</div>
				<div slot="body">
					<MenuItem to="/" icon="fa-home" index="7">Home</MenuItem>
					<MenuItem to="/about" icon="fa-calendar-alt" index="8">Planning</MenuItem>
					<MenuGroup icon="fa-chess-board" title="Chess board">
						<div slot="children">
							<MenuItem icon="fa-chess-knight" index="1">Child 1</MenuItem>
							<MenuItem icon="fa-chess-rook" index="2">Child 2</MenuItem>
							<MenuItem icon="fa-chess-pawn" index="3">Child 3</MenuItem>
							<MenuGroup icon="fa-chess-board" title="Chess board">
								<div slot="children">
									<MenuItem icon="fa-chess-knight" index="4">Child 1</MenuItem>
									<MenuItem icon="fa-chess-rook" index="5">Child 2</MenuItem>
									<MenuItem icon="fa-chess-pawn" index="6">Child 3</MenuItem>
								</div>
							</MenuGroup>
						</div>
					</MenuGroup>
					<MenuItem to="/stats" icon="fa-chart-pie" index="9">Statistics</MenuItem>
				</div>
				<div slot="footer">This is the footer</div>
			</side-menu>
		</div>

		<router-view/>
	</div>
</template>

<script>
import SideMenu from './components/sidemenu/sidemenu.vue';
import MenuItem from './components/sidemenu/menu-item.vue';
import MenuGroup from './components/sidemenu/menu-group.vue';

export default {
	components: {
		'side-menu': SideMenu,
		MenuItem,
		MenuGroup
	},
	data() {
		return {
			open: false,
			open2: false,
			selected: null
		};
	},
	methods: {
		toggleSidemenu() {
			this.open = true;
		},
		toggleSidemenu2() {
			this.open2 = true;
		}
	},
	watch: {
		selected: function(val) {
			this.$refs.menu.setActiveIndex(val);
		}
	}
};
</script>

<style lang="scss">
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
}
#nav {
	padding: 30px;
	a {
		font-weight: bold;
		color: #2c3e50;
		&.router-link-exact-active {
			color: #42b983;
		}
	}
}

.container {
	border: 1px solid red;
	position: relative;
}

.user-info {
	color: rgba(70, 80, 90, 0.7);
	text-decoration: none;
}

.btn-noborder {
	border: none !important;
}

a:hover {
	text-decoration: none;
}

.user-info {
	display: flex;
	.username {
		align-self: center;
	}
}
</style>
