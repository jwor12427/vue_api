<!-- @format -->

<template>
	<div>
		<HeaderCont />
		<TitleCont name1="unsplash" name2="reference api" />
		<section class="cont__refer">
			<div class="container">
				<div class="unsplash__inner">
					<div class="unsplash__slider"></div>
					<div class="movie__search">
						<div class="container">
							<h2>검색하기</h2>
							<form @submit.prevent="SearchSplashes()">
								<input
									type="search"
									id="search"
									placeholder="검색해주세요~"
									v-model="search"
								/>
								<button type="submit">검색</button>
							</form>
						</div>
					</div>
					<div class="unsplash__images">
						<ul>
							<li v-for="splash in splashes" :key="splash.id">
								<a :href="`https://unsplash.com/photos/${splash.id}`">
									<img :src="splash.urls.regular" :alt="splash.id" />
								</a>
							</li>
						</ul>
					</div>
				</div>
			</div>
		</section>
		<FooterCont />
		<ContactCont />
	</div>
</template>

<script>
import HeaderCont from '@/components/HeaderCont.vue';
import FooterCont from '@/components/FooterCont.vue';
import TitleCont from '@/components/TitleCont.vue';
import ContactCont from '@/components/ContactCont.vue';
import { ref } from 'vue';
export default {
	components: {
		HeaderCont,
		FooterCont,
		TitleCont,
		ContactCont,
	},

	setup() {
		const splashes = ref([]);
		const sliders = ref([]);
		const search = ref([]);

		const SearchSplashes = async () => {
			await fetch(
				`https://api.unsplash.com/search/photos?client_id=ARu1VkatUtWaDHDwjkYnEyEK_OAVZ-LOCf-cTrHBtQg&query=${search.value}&count=20`,
			)
				.then(response => response.json())
				// .then(result => console.log(result))
				.then(result => {
					splashes.value = result;
					search.value = '';
				})
				.then(error => console.log(error));
		};
		SearchSplashes();
		const RandomSplashes = async () => {
			await fetch(
				'https://api.unsplash.com/photos/random?client_id=ARu1VkatUtWaDHDwjkYnEyEK_OAVZ-LOCf-cTrHBtQg&count=10',
			)
				.then(response => response.json())
				.then(result => (sliders.value = result))
				.catch(error => console.log(error));
		};
		RandomSplashes();
		return {
			splashes,
			search,
			sliders,
			SearchSplashes,
			RandomSplashes,
		};
	},
};
</script>

<style lang="scss">
.unsplash__images {
	ul {
		column-count: 4;
		column-gap: 20px;
		width: 100%;
		li {
			margin-bottom: 20px;

			img {
				border-radius: 5px;
			}
		}
	}
}
</style>
