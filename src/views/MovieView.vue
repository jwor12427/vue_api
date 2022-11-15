<!-- @format -->

<template>
	<div>
		<HeaderCont />
		<TitleCont name1="movie" name2="reference api" />
		<section class="cont__refer">
			<div class="container">
				<div class="movie__inner">
					<div class="movie__slider">
						<h2>Top Movies</h2>
						<swiper
							:effect="'coverflow'"
							:grabCursor="true"
							:centeredSlides="true"
							:slidesPerView="'auto'"
							:coverflowEffect="{
								rotate: 50,
								stretch: 0,
								depth: 100,
								modifier: 1,
								slideShadows: true,
							}"
							:autoplay="{
								delay: 3000,
								disableOnInteraction: false,
							}"
							:pagination="true"
							:modules="modules"
							:initialSlide="3"
							class="mySwiper"
						>
							<swiper-slide v-for="slider in sliders" :key="slider.id"
								><a :href="`https://www.themoviedb.org/movie/${slider.id}`">
									<img
										:src="`https://image.tmdb.org/t/p/w500/${slider.poster_path}`"
										:alt="slider.title"
									/>
									<em>
										<span class="title">{{ slider.title }}</span>
										<span class="star">{{ slider.vote_average }}</span></em
									></a
								></swiper-slide
							>
						</swiper>
					</div>
					<!-- //movie__slider -->
					<div class="movie__search">
						<div class="container">
							<h2>검색하기</h2>
							<form @submit.prevent="SearchMovies()">
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
					<!-- //movie__search -->
					<div class="movie__movies">
						<div class="container">
							<div class="movie__list">
								<ul>
									<li v-for="movie in movies" :key="movie.id">
										<a :href="`https://www.themoviedb.org/movie/${movie.id}`">
											<img
												:src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
												:alt="movie.title"
											/>
											<em>
												<span class="title">{{ movie.title }}</span>
												<span class="star">{{ movie.vote_average }}</span>
											</em>
										</a>
									</li>
								</ul>
							</div>
						</div>
					</div>
					<!-- //movie__movies -->
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
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue';
// Import Swiper styles
import 'swiper/css';
import 'swiper/css/effect-coverflow';
import 'swiper/css/pagination';
// import required modules
import { Autoplay, EffectCoverflow, Pagination } from 'swiper';
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
		const movies = ref([]);
		const sliders = ref([]);
		const search = ref('disney');

		const SearchMovies = async () => {
			await fetch(
				`https://api.themoviedb.org/3/search/movie?api_key=dab7e73d4b52c62b1d206d7f6bd38ec0&query=${search.value}`,
			)
				.then(response => response.json())
				// .then(result => console.log(result))
				.then(result => {
					movies.value = result.results;
					search.value = '';
				})
				.catch(error => console.log(error));
		};
		SearchMovies();

		const TopMovies = async () => {
			await fetch(
				'https://api.themoviedb.org/3/movie/popular?api_key=dab7e73d4b52c62b1d206d7f6bd38ec0&language=ko',
			)
				.then(response => response.json())
				.then(result => (sliders.value = result.results))
				.catch(error => console.log(error));
		};
		TopMovies();
		return {
			movies,
			sliders,
			search,
			SearchMovies,
			TopMovies,
			modules: [Autoplay, EffectCoverflow, Pagination],
		};
	},
};
</script>
<style lang="scss">
.movie__list {
	ul {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		li {
			width: 24%;
			margin-bottom: 40px;
			position: relative;
			img {
				height: 400px;
			}

			em {
				display: block;
				height: 40px;
				font-family: var(--font-kor2);

				.title {
					padding: 5px 0;
					display: inline-block;
				}
				.star {
					background: #fff;
					color: #000;
					position: absolute;
					left: 10px;
					top: 10px;
					width: 30px;
					height: 30px;
					border-radius: 100px;
					text-align: center;
					line-height: 30px;
					font-weight: 800;
				}
			}
		}
	}
}
.movie__search {
	margin-bottom: 100px;

	.container {
		position: relative;
	}
	h2 {
		color: var(--black);
		font-size: 40px;
		text-indent: -9999px;
		height: 0;
	}
	input {
		background: #ddd;
		border: 2px solid #ddd;
		border-radius: 50px;
		color: var(--black);
		width: 100%;
		padding: 14px 30px;
		font-family: var(--font-kor1);
	}
	button {
		position: absolute;
		right: 7px;
		top: 7px;
		height: 40px;
		width: 40px;
		border-radius: 50%;
		border: 0;
		font-family: var(--font-kor1);
		cursor: pointer;
		z-index: 1000;
		background-color: #fff;
		font-size: 14px;
	}
}
.movie__slider {
	.swiper {
		width: 100%;
		padding-top: 50px;
		padding-bottom: 50px;
		margin-bottom: 30px;
		.swiper-slide {
			background-position: center;
			background-size: cover;
			width: 300px;
			height: 500px;
			.swiper-slide img {
				display: block;
				width: 100%;
			}
			em {
				display: block;
				height: 40px;
				font-family: var(--font-kor2);

				.title {
					padding: 5px 0;
					display: inline-block;
				}
				.star {
					background: #fff;
					color: #000;
					position: absolute;
					left: 10px;
					top: 10px;
					width: 30px;
					height: 30px;
					border-radius: 100px;
					text-align: center;
					line-height: 30px;
					font-weight: 800;
				}
			}
		}
	}
	.swiper-pagination-bullet-active {
		background: #000 !important;
	}
}
</style>
