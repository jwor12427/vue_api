<!-- @format -->

<template>
	<div>
		<HeaderCont />
		<TitleCont name1="unsplash" name2="reference api" />
		<section class="cont__refer">
			<div class="container">
				<div class="unsplash__inner">
					<div class="unsplash__slider">
						<swiper
							:effect="'cards'"
							:grabCursor="true"
							:modules="modules"
							class="mySwiper"
						>
							<swiper-slide v-for="slider in sliders" :key="slider.id"
								><a :href="`https://unsplash.com/photos/${slider.id}`">
									<img :src="slider.urls.regular" :alt="slider.id" /></a
							></swiper-slide>
							<swiper-slide>Slide 1</swiper-slide>
						</swiper>
					</div>
					<div class="movie__search">
						<div class="container">
							<h2>검색하기</h2>
							<form @submit.prevent="SearchSplash()">
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
							<li v-for="splash in splashes" :key="splash.links">
								<a :href="splash.links.download">
									<img
										:src="splash.urls.regular"
										:alt="splash.urls.alt_description"
									/>
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
import { Swiper, SwiperSlide } from 'swiper/vue';
import { EffectCards } from 'swiper';

import 'swiper/css';

import 'swiper/css/effect-cards';
import { ref } from 'vue';
export default {
	components: {
		HeaderCont,
		FooterCont,
		TitleCont,
		ContactCont,
		Swiper,
		SwiperSlide,
	},

	setup() {
		const splashes = ref([]);
		const sliders = ref([]);
		const search = ref('wallpaper');

		const SearchSplash = async () => {
			// 서치이미지
			await fetch(
				`https://api.unsplash.com/search/photos?client_id=ARu1VkatUtWaDHDwjkYnEyEK_OAVZ-LOCf-cTrHBtQg&query=${search.value}&per_page=24`,
			)
				.then(response => response.json())
				.then(result => {
					console.log(result);
					splashes.value = result.results;
					search.value = '';
				})
				.catch(error => console.log('error', error));
		};
		SearchSplash();
		// const RandomSplashes = async () => {
		// 	await fetch(
		// 		'https://api.unsplash.com/photos/random?client_id=ARu1VkatUtWaDHDwjkYnEyEK_OAVZ-LOCf-cTrHBtQg&count=20&query=wallpaper',
		// 	)
		// 		.then(response => response.json())
		// 		.then(result => (splashes.value = result))
		// 		.then(error => console.log(error));
		// };
		// RandomSplashes();
		const SliderSplashes = async () => {
			await fetch(
				'https://api.unsplash.com/photos/random?client_id=ARu1VkatUtWaDHDwjkYnEyEK_OAVZ-LOCf-cTrHBtQg&count=10',
			)
				.then(response => response.json())
				.then(result => (sliders.value = result))
				.catch(error => console.log(error));
		};
		SliderSplashes();
		return {
			splashes,
			search,
			sliders,
			SearchSplash,
			// RandomSplashes,
			SliderSplashes,
			modules: [EffectCards],
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
.unsplash__slider {
	margin-bottom: 30px;
	.swiper {
		width: 300px;
		height: 420px;
	}

	.swiper-slide {
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 18px;
		font-size: 22px;
		font-weight: bold;
		color: #fff;
	}
}
</style>
