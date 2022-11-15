<!-- @format -->

<template>
	<div>
		<HeaderCont />
		<TitleCont name1="css" name2="reference api" />
		<section class="cont__refer">
			<div class="container">
				<div class="refer__inner">
					<h2>CSS</h2>
					<ul class="refer__list">
						<li v-for="refer in refers" :key="refer.title">
							<a href="#">
								<span class="num">{{ refer.num }}</span>
								<span class="name">{{ refer.title }}</span>
								<span class="desc">{{ refer.desc }}</span>
								<span class="star">{{ refer.descStar }}</span>
							</a>
						</li>
					</ul>
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
		const refers = ref([]);

		const reference = () => {
			fetch('https://jwor12427.github.io/react_api/src/utils/reference.json')
				.then(response => response.json())
				// .then(result => console.log(result))
				.then(result => (refers.value = result.cssRefer))
				.catch(error => console.log('error', error));
		};
		reference();

		return {
			refers,
			reference,
		};
	},
};
</script>
<style lang="scss">
.refer__list {
	border-bottom: 1px solid var(--bg-light-border);
	li {
		border-bottom: 1px solid var(--bg-light-border);
		box-sizing: border-box;
		a {
			display: flex;
			align-items: center;
			width: 100%;
			color: var(--black);

			span {
				display: inline-block;
				padding: 15px 20px;
				box-sizing: border-box;
			}

			.num {
				flex: 1 1 5%;
				text-align: center;
			}
			.name {
				flex: 1 1 18%;
			}
			.desc {
				flex: 1 1 72%;

				font-family: var(--font-kor2);
				font-size: 14px;
			}
			.star {
				flex: 1 1 5%;
				text-align: center;
			}
		}
	}
}
</style>
