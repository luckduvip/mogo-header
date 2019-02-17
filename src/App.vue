<template>
<div>
	<header class="header" :class="theme">
		<div class="header-main">
			<div :class="{auto:autoWidth==1}" class="left-wrap flex" @click="leftClick">
				<slot name="left">
					<img v-if="theme == 'light'" src="../images/header-back-1.png"/>
					<img v-else src="../images/header-back-2.png"/>
				</slot>
			</div>
			<div class="header-h1"><slot>标题</slot></div>
			<div :class="{auto:autoWidth==1}" class="right-wrap flex" @click="rightClick" >
				<slot name="right"></slot>
			</div>
		</div>
	</header>
	<div class="keep" v-if="keep"></div>
</div>
</template>

<script>
export default{
	data(){
		return {
			headerHeight: 0,
		}
	},
	computed: {
	},
	props: {
		/**主题名称，黑色，白色**/
		theme: {type: String, default: 'dark'},
		/**是否position:fixed
			0: position: relative
			1: position: fixed
			**/
		keep: { type: String, default: '1', },
		/**自动宽度**/
		autoWidth: { type: String,default: '1' },
	},
	methods: {
		initHeader(){
			/*
			 *var header = this.$refs.header;
			 *this.$api.fixStatusBar(header);
			 *this.headerHeight = this.getHeaderHeight();
			 *api.setStatusBarStyle({
			 *    style: 'dark'
			 *});
			 */
		},
		getHeaderHeight(){
			return this.$refs.header.offsetHeight;
		},
		initHome(){
		},
        // 左侧按钮事件
        // 可通过设置 back 函数阻止默认后退行为
		leftClick () {
			// 通过判断 $liteners 是否有定义 back 事件
			// 参考：https://cn.vuejs.org/v2/api/#vm-listeners
			if (typeof this.$listeners.left !== 'undefined') {
				this.$emit('left')
			} else {
				window.api && api.closeWin();
			}
		},
        // 右侧按钮事件
        rightClick () {
            // 如果是 a 标签，且有 href, 不进行回调
			if (this.$slots.right && this.$slots.right[0].tag === 'a' && this.$slots.right[0].data.attrs.href.indexOf('javascript:') === -1) return
            if (typeof this.$listeners.right !== 'undefined') {
				this.$emit('right')
            } else {
            }
        }
	},
	mounted(){
		this.initHeader();
	}
}
</script>

<style scoped lang="scss">
*{
	padding: 0; margin: 0; box-sizing: border-box; 
}
.header{
	position: fixed; left: 0; top: 0;
	width: 100%; 
	&-main{
		display: flex; align-items: center;
		height: 88px;
	}
	.left-wrap{
		display: block; height: 88px; padding: 22px 0 22px 20px; 
		&.auto{
			width: 100px;
		}
		img{
			display: block; width: 40px; height: 40px;
		}
	}
	.right-wrap{
		display: block; font-size: 24px; color: #666; text-align: right; padding-right: 20px;
		&.auto{
			width: 100px;
		}
	}
	&-h1{
		flex: 1; -webkit-flex: 1;
		display: block; text-align: center; font-size: 32px; color: #222;
	}
	&.light{
		background: #fff;
		.header-h1{
			color: #222;
		}
		.right-wrap{
			color: #666;
		}
	}
	&.dark{
		background: #000;
		.header-h1{
			color: #fff;
		}
		.right-wrap{
			color: #999;
		}
	}
}
</style>
