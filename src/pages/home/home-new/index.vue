<template>
	<view>
		内容3
    </view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		mounted () {
			uni.setNavigationBarTitle({ title: '最新' })
		},
		methods: {

		}
	}
</script>

<style lang="sass">
	
</style>
