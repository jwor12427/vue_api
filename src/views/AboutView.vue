<!-- @format -->

<template>
	<div>
		<HeaderCont />
		<TitleCont name1="about" name2="me" />
		<div class="about__inner">
			<div class="container">
				<div class="about01">
					<img src="../assets/img/aboutImg01.png" alt="" class="about__img" />
					<div class="about__desc"></div>
				</div>
				<div class="about02">
					<img src="" alt="" class="about__img" />
					<div class="about__desc"></div>
				</div>
				<div class="about03">
					<img src="" alt="" class="about__img" />
					<div class="about__desc"></div>
				</div>
			</div>
		</div>
		<ContactCont />
		<FooterCont />
	</div>
</template>

<script>
import HeaderCont from '@/components/HeaderCont.vue';
import FooterCont from '@/components/FooterCont.vue';
import TitleCont from '@/components/TitleCont.vue';
import ContactCont from '@/components/ContactCont.vue';
export default {
	components: {
		HeaderCont,
		FooterCont,
		TitleCont,
		ContactCont,
	},
};
</script>
