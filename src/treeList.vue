
<template>
	<ul class="list">
		<li v-for="(option,index) in options" class="itm" :id="option.id"  :data-level="level" >
			<div class="itm-content" v-if="option.id != undefined" :class="{active: root.selected === option, disabled: disabled || option.disabled}" @click.stop="selectOption(option)" :style="{ paddingLeft: level * 10 + 'px'}">
				<span class="txt">{{option.name}}</span>
				<i class="u-icon-tickblue"></i>
			</div>
			<div v-else class="itm-label">
				<span class="txt">{{option.name}}</span>
				<i class="u-icon-tickblue"></i>
			</div>
			<sf-tree-list v-if="option.children && option.children.length" :options="option.children" :root="root" :disabled="option.disabled" :level="level+1"></sf-tree-list>
		</li>
	</ul>
</template>

<style type="text/css" lang="sass" scoped>
	.list{
	    position: relative;
	    max-height: 300px;
	    overflow: hidden;
	    background-color: #fff
	}
	.itm{
	    line-height: 30px;
	    box-sizing: border-box;
	    overflow: hidden;
	    word-wrap: normal;
	    white-space: nowrap;
	    text-overflow: ellipsis;
	    .itm-content{
	    	&:hover{
	    		background-color: #e8f1f8;
		    	color: #62a8ea;
	    	}
		&.active {
			.u-icon-tickblue{
				display: inline-block;
			}
			color: #62a8ea;
		}
			&.disabled{
				color: #999;
				background: #eee;
				cursor: not-allowed;
			}
	    }
	    .itm-label{
	    	color: #999;
	    	cursor: default;
	    }
		.txt{
			display: inline-block;
			margin-left: 5px;
			vertical-align: middle;
		}
		.u-icon-tickblue{
			display: none;
			width: 14px;
			height: 14px;
			margin-left: 5px;
			vertical-align: middle;
			background: url(/docs/res/img/icon-14.png);
			background-size: 120px auto;
			background-position: 0 -570px;
		}
	}
</style>
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