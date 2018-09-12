<template>
	<div>
		<b-jumbotron>
			<template slot="header">Vue Context Props Usage</template>
			<template slot="lead">
				These are some demos of how to use some of the props available
				to the <code>vue-context</code> component. On each of the demos,
				right click on the items to trigger the context menu.
			</template>
			<p>
				For more information, please visit the documentation.
			</p>
			<b-btn variant="primary"
			       href="https://github.com/rawilk/vue-context#props"
			       target="_blank"
			>
				More Info
			</b-btn>
		</b-jumbotron>

		<h3>Demo #1</h3>
		<p>
			<strong>Prop:</strong> <code>closeOnClick</code>
		</p>
		<p>
			If this prop is set to <code>false</code>, the menu will not automatically close when clicked on.
			You can toggle this feature on and off in this demo by using the checkbox below.
		</p>

		<div class="mb-2">
			<b-checkbox v-model="closeOnClick">
				Close on Click
			</b-checkbox>
		</div>

		<b-list-group>
			<b-list-group-item
				v-for="(item, index) in items"
				:key="index"
				@contextmenu.prevent="$refs.menu1.open($event)"
			>
				{{ item }}
			</b-list-group-item>
		</b-list-group>

		<vue-context ref="menu1" :close-on-click="closeOnClick">
			<ul>
				<li>
					{{ closeOnClick ? 'I will close on click' : 'I will stay open on click' }}
				</li>
			</ul>
		</vue-context>

		<hr>
		<h3>Demo #2</h3>
		<p>
			<strong>Prop:</strong> <code>closeOnScroll</code>
		</p>
		<p>
			If this is set to <code>true</code>, the context menu will close automatically when the window is
			scrolled. You can toggle this feature on and off in this demo by using the checkbox below.
		</p>

		<div class="mb-2">
			<b-checkbox v-model="closeOnScroll">
				Close on Scroll
			</b-checkbox>
		</div>

		<b-list-group>
			<b-list-group-item
					v-for="(item, index) in items"
					:key="index"
					@contextmenu.prevent="$refs.menu2.open($event)"
			>
				{{ item }}
			</b-list-group-item>
		</b-list-group>

		<vue-context ref="menu2" :close-on-scroll="closeOnScroll">
			<ul>
				<li>
					{{ closeOnScroll ? 'I will hide when the window is scrolled' : 'I will stay visible when the window is scrolled' }}
				</li>
			</ul>
		</vue-context>

		<p class="mt-4">
			<b-link href="https://github.com/rawilk/vue-context/blob/gh-pages/src/tabs/props-usage.vue">
				View demo source
			</b-link>
		</p>
	</div>
</template>

<script>
	import usageMixin from '../mixins/usageMixin';

	export default {
		name: 'props-usage',

		mixins: [usageMixin],

		data () {
		    return {
		        closeOnClick: false,
			    closeOnScroll: true
		    };
		}
	};
</script>