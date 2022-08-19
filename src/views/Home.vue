<template>
    <Banner />
    <div class="mainContainer">
        <h1>Monte seu Burger</h1>
        <BurgerForm />
    </div>
</template>
<script>
import Banner from '../components/Banner.vue';
import BurgerForm from '../components/BurgerForm.vue';
export default {
    components: { Banner, BurgerForm }
}
</script>
<style>
</style>