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
			},
			_onMousedown: function(event){
				if(!this.$refs['select-box'].contains(event.target)){
					this.toggle(false);
				}
			}
		},
		created() {
			document.addEventListener('mousedown', this._onMousedown);
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
		},
		destroyed(){
			document.removeEventListener('mousedown', this._onMousedown);
		}
	}
</script>