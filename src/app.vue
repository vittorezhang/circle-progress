<template>
	<div class="Circleprogress">
		<div class="ring">
			<svg
				:width="width"
				:height="width"
				:viewbox="`0 0 ${width} ${width}`"
				class="svgWrap"
			>
				<circle
					:cx="width * 0.5"
					:cy="width * 0.5"
					:r="width * 0.4"
					:stroke-width="width * 0.05"
					stroke-linecap="round"
					:stroke="backColor"
					fill="none"
					stroke-opacity="0.2"
					:transform="`matrix(0,-1,1,0,0,${width})`"
					:stroke-dasharray="
						`${0.75 * (width * 0.4 * Math.PI * 2)} ${0.25 *
							(width * 0.4 * Math.PI * 2)}`
					"
				></circle>
				<circle
					ref="progress"
					:cx="width * 0.5"
					:cy="width * 0.5"
					:r="width * 0.4"
					:stroke-width="width * 0.05"
					stroke-linecap="round"
					:stroke="lineColor"
					fill="none"
					:transform="`matrix(0,-1,1,0,0,${width})`"
					:stroke-dasharray="dasharray"
				></circle>
			</svg>
			<div class="ring_box">
				<div class="fraction">
					<div
						class="desc_"
						:style="{
							color: cssStyle.titleColor,
						}"
					>
						{{ title }}
					</div>
					<div
						class="text"
						:style="{
							color: cssStyle.textColor,
						}"
					>
						{{ content ? content : rate * 100 + '%' }}
					</div>
				</div>
				<div
					class="title font-center"
					@click="toComponent"
					:style="{
						color: cssStyle.viewDetailsColor,
					}"
				>
					<span>{{ viewDetails }}</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Circleprogress',
	props: {
		title: {
			type: String,
			default: '圆环标题',
		},
		content: {
			type: String,
			default: '画了个图',
		},
		viewDetails: {
			type: String,
			default: '查看一波',
		},
		rate: {
			type: Number,
			default: 1,
		},
		width: {
			type: Number,
			default: 100,
		},
		lineColor: {
			type: String,
			default: 'pink',
		},
		backColor: {
			type: String,
			default: 'yellow',
		},
		cssStyle: {
			type: Object,
			default: function() {
				return {
					titleColor: 'black',
					textColor: 'rgb(88, 218, 88)',
					viewDetailsColor: 'pink',
				}
			},
		},
	},
	data() {
		return {
			dasharray: `0 ${this.width * 0.4 * Math.PI * 2}`,
		}
	},
	mounted() {
		this.draw(this.rate)
	},
	methods: {
		draw(rate) {
			this.$nextTick(() => {
				let per = 0.75 * rate
				let circle = this.$refs['progress']
				let perimeter = circle.getTotalLength() //圆环的周长
				this.dasharray = perimeter * per + ' ' + perimeter * (1 - per)
			})
		},
		toComponent() {
			this.$emit('jumpPage', this.rate)
		},
	},
}
</script>
<style lang="less" scoped>
.Circleprogress {
	overflow: hidden;
	.svgWrap {
		transform: rotateZ(-135deg);
		circle {
			/* 圆环的过渡动画 */
			-webkit-transition: stroke-dasharray 800ms;
			transition: stroke-dasharray 800ms;
		}
	}
	.ring {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		position: relative;
	}
	.ring_box {
		width: 100%;
		height: 100%;
		position: absolute;
		text-align: center;
		top: 0;
		left: 0;
		.fraction {
			padding-top: 10%;
			.desc_ {
				font-size: 16px;
				font-weight: 400;
				color: powderblue;
				text-align: center;
			}
			.text {
				font-size: 16px;
				font-weight: bold;
				text-align: center;
				padding-top: 16px;
				color: rgb(88, 218, 88);
			}
		}

		.title {
			width: 72px;
			border-radius: 18px;
			border: 1px solid rgba(194, 193, 193, 0.8);
			color: blueviolet;
			font-size: 16px;
			font-weight: 400;
			position: absolute;
			left: 41%;
			bottom: 10px;
		}
	}
}
</style>
