
<template>
	<ul class="list">
		<li v-for="(option,index) in options" class="itm" :id="option.id"  :data-level="level" >
			<div class="itm-content" v-if="option.id != undefined" :class="{active: root.selected === option, disabled: disabled || option.disabled}" @click.stop="selectOption(option)" :style="{ paddingLeft: level * 10 + 'px'}">
				<span class="txt">{{option.name}}</span>
				<i class="u-icon-tickblue"></i>
			</div>
			<div v-else class="itm-label" :style="{ paddingLeft: level * 10 + 'px'}">
				<span class="txt">{{option.name}}</span>
				<i class="u-icon-tickblue"></i>
			</div>
			<sf-tree-list v-if="option.children && option.children.length" :options="option.children" :root="root" :disabled="option.disabled" :level="level+1"></sf-tree-list>
		</li>
	</ul>
</template>

<script type="text/javascript">
	export default {
		name: 'sf-tree-list',
		componentName: 'treeList',
		components: {

		},
		computed: {

		},
		props: {
			options: Array,
			root: Object,
			level: Number,
			disabled: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {};
		},
		methods: {
			selectOption(option) {
				if(this.disabled || option.disabled) return;
				this.root.$emit('selectOption', option);
			}
		},
		created() {
      	}
	}
</script>