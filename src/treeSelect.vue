/**
 * sf-treeSelect 组件
 *
 * @author:   sch
 * @props  {String} prop         -参数         
 * 
 */

<template>
	<div class="m-select" ref="select-box">
		<div class="select-sel" @mousedown="_onToggle($event)" :id="selected.id" :class="{disabled: disabled}">
			<span>{{selected.name}}</span>
			<i class="u-icon-down"></i>
		</div>
		<div class="select-cnt" v-show="isShow">
			<sf-tree-list :options="options" :root="root" :level="0"></sf-tree-list>
		</div>
	</div>
</template>

<style lang="sass" scoped>
	.m-select{
	    display: inline-block;
	    position: relative;
	    margin: 0 auto;
	    height: 32px;
	    vertical-align: middle;
	    background: #fafafa;
	    border: 1px solid #ebebeb;
	    border-radius: 2px;
	    box-shadow: 0 1px 2px -3px #000;
	    cursor: pointer;
	    font-size: 14px;
	    .select-sel{
	        height: 32px;
	    	line-height: 32px;
	        padding: 0 50px 0 40px;
	        color: #000;
	        border: none;
	        span{
	            line-height: 32px;
	            vertical-align: top;
	        }
	        &.disabled{
	        	color: #999;
	        	background: #eee;
	        	cursor: not-allowed;
	        }
	    }
	    .select-cnt{
	        position: absolute;
	        top: 36px;
	        left: 0;
	        width: 150px;
	        border: 1px solid #e6eaeb;
	        border-radius: 2px;
            box-shadow: 0 5px 10px rgba(0,0,0,0.2);
	        background-color: #fff;
	        z-index: 10;
	    }
	    .u-icon-down{
	        position: absolute;
	        top: 15px;
	        right: 10px;
	        width: 12px;
	        height: 12px;
	        background: url(/docs/res/img/icon-12.png);
	        background-position: 0 -540px;
	        background-size: 120px auto;
	    }
	}
</style>

<script>
	import sfTreeList from './treeList.vue'
	export default {
		name: 'sf-tree-select',
		componentName: 'treeSelect',
		components: {
			'sf-tree-list': sfTreeList
		},
		computed: {

		},
		props: {
			options: Array,
			defid: [String, Number],
			title: String,
			disabled: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				selected: {},
				isShow: false,
				root: this
			}
		},
		methods: {
			/**
			 * 展开/折叠下拉列表(主动)
			 * @param  {boolean} show 
			 * @return {void}     
			 */
			toggle(show) {
				if(this.disabled == true) return;
				if(show == undefined){
					this.isShow = !this.isShow;
				}else{
					this.isShow = show;
				}
				this.$emit('toggle', {show: this.isShow});
			},
			/**
			 * 展开/折叠下拉列表(被动)
			 * @param {event} event 
			 * @return {void}     
			 */
			_onToggle(event){
				this.toggle();
			},
			/**
			 * 选择某一项(主动)
			 * @param  {object} option 选中项
			 * @return {void}        
			 */
			select(option) {
				if(this.disabled == true || option.id == undefined) return;
				this.selected = option;
				this.toggle(false);
			},
			/**
			 * 某一项被选中(被动)
			 * @param  {object} option 选中项
			 * @return {void}        
			 */
			onOptionSelect(option) {
				this.selected = option;
				this.$emit('change', this.selected);
				this.toggle(false);
			}
		},
		created() {
			document.addEventListener('mousedown', (e) => {
				if(!this.$refs['select-box'].contains(e.target)){
					this.toggle(false);
				}
			})
			this.$on('selectOption', this.onOptionSelect);
			let getDefOption = (options) => {
				if(this.defid == undefined) return;
				for(let i =0; i < options.length; i++){
					let item = options[i];
					if(item.id == this.defid){
						return item;
					}else if(item.children && item.children.length > 0){
						let option = getDefOption(item.children);
						if(!!option) return option;
					}
				}
			}
			let option = getDefOption(this.options);
			if(!!option){
				this.selected = option;
			}else{
				this.selected = {
					name: this.title || '请选择'
				}
			}
		}
	}
</script>