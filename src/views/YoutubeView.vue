<!-- @format -->
<template>
	<div>
		<HeaderCont />
		<TitleCont name1="youtube" name2="reference api" />
		<section class="cont__refer">
			<div>
				<div class="youtube__inner">
					<swiper
						:slidesPerView="3"
						:spaceBetween="30"
						:slidesPerGroup="3"
						:loop="true"
						:loopFillGroupWithBlank="true"
						:pagination="{
							clickable: true,
						}"
						:autoplay="{
							delay: 5000,
							disableOnInteraction: false,
						}"
						:initialSlide="3"
						:navigation="true"
						:modules="modules"
						class="mySwiper"
					>
						<swiper-slide v-for="slider in sliders" :key="slider.videoId"
							><a
								:href="`https://www.youtube.com/watch?v=${slider.videoId}`"
								class="slider__link"
							>
								<img
									:src="slider.snippet.thumbnails.high.url"
									class="slider__img"
								/>
							</a>
						</swiper-slide>
					</swiper>
					<div class="movie__search">
						<div class="container">
							<h2>검색하기</h2>
							<form @submit.prevent="YoutubeSearch()">
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
					<!-- //youtube__search -->
					<div class="youtube__conts">
						<div class="container">
							<div class="youtube__list">
								<ul>
									<li v-for="youtubes in youtube" :key="youtubes.videoId">
										<a
											:href="`https://www.youtube.com/watch?v=${youtubes.videoId}`"
										>
											<img
												:src="youtubes.snippet.thumbnails.medium.url"
												:alt="youtubes.videoId"
											/>
											<p class="youtube__title">{{ youtubes.snippet.title }}</p>
										</a>
									</li>
								</ul>
							</div>
						</div>
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
import { Autoplay, Pagination, Navigation } from 'swiper';
import 'swiper/css';

import 'swiper/css/pagination';
import 'swiper/css/navigation';
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
		const youtube = ref([]);
		const search = ref([]);
		const sliders = ref([]);

		const YoutubeSlider = () => {
			fetch('https://jwor12427.github.io/react_api/src/utils/youtube.json')
				.then(response => response.json())
				.then(result => (sliders.value = result.items))
				.catch(error => console.log(error));
		};
		YoutubeSlider();
		const YoutubeSearch = async () => {
			await fetch(
				`https://www.googleapis.com/youtube/v3/search?part=snippet&q=${search.value}&key=AIzaSyC87T5BNU0OPSsPAjb42R-Wb4Fd3GAFxDg&regionCode=KR&maxResults=20&type=video`,
			)
				.then(response => response.json())
				.then(result => {
					youtube.value = result.items;
					search.value = '';
				})
				.catch(error => console.log(error));
		};
		YoutubeSearch();
		return {
			youtube,
			search,
			sliders,
			YoutubeSlider,
			YoutubeSearch,
			modules: [Autoplay, Pagination, Navigation],
		};
	},
};
</script>
<style lang="scss">
.youtube__conts {
	ul {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		li {
			width: 24%;
			a {
				img {
					border-radius: 5px;
					margin-bottom: 10px;
				}
				.youtube__title {
					font-size: 14px;
					font-family: var(--font-kor1);
					width: 250px;
					white-space: nowrap;
					overflow: hidden;
					text-overflow: ellipsis;
					margin-bottom: 30px;
				}
			}
		}
	}
}
.youtube__inner {
	.swiper {
		margin-bottom: 30px;
		height: 350px;
		.swiper-slide {
			.slider__link {
				.slider__img {
					border-radius: 5px;
					height: 100%;
				}
			}
		}
		.swiper-button-next,
		.swiper-button-prev {
			color: #fff !important;
		}
		.swiper-pagination {
			bottom: 0;
			.swiper-pagination-bullet-active {
				background: #fff;
			}
		}
	}
}
</style>
