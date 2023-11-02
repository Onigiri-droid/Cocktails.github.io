<script setup>
import AppLayout from '../components/AppLayout.vue';
import CocktailThumb from '../components/CocktailThumb.vue'
import { useRootStore } from '../stores/root';
import { storeToRefs } from 'pinia';

const rootStore = useRootStore();
rootStore.getIngredients();

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore);

function getCocktail() {
    rootStore.getCocktails(rootStore.ingredient)
}

function removeIngredient() {
    rootStore.setIngredient(null)
}
</script>
<!-- /src/assets/img/bg-1.jpg -->
<template>
    <AppLayout imgUrl="src/assets/img/dawa-cocktail.png" :backFunc="removeIngredient" :is-back-btn-visible="!!ingredient">
        <div class="wrapper">
            <div v-if="!ingredient || !cocktails" class="info">
                 <div class="title">Выберите свой напиток</div>
                 <div class="line"></div>
                 <div class="select-wrapper">
                    <el-select v-model="rootStore.ingredient" class="select" placeholder="Выберите ингредиент" size="large" filterable allow-create @change="getCocktail">
                    <el-option v-for="item in ingredients" :key="item.strIngredient1" :label="item.strIngredient1" :value="item.strIngredient1"/></el-select>
                 </div>
                 <div class="text">Попробуйте наши вкусные рецепты коктейлей на любой случай. Найдите рецепты вкусных коктейлей по ингредиентам с помощью нашего генератора коктейлей.</div>
                 <img src="src/assets/img/cocktails.png" alt="cocktails" class="img">
            </div>
            <div v-else class="info">
                <div class="title">Напиток с {{ ingredient }}</div>
                 <div class="line"></div>
                 <div class="cocktails">
                    <CocktailThumb v-for="cocktail in cocktails" :key="cocktail.idDrink" :cocktail="cocktail"/>
                 </div>
            </div>
        </div>
    </AppLayout>
</template>



<style lang="scss" scoped>
@import '../assets/styles/main';

.select-wrapper{
    padding-top: 50px;
}
.select{
    width: 220px;
}
.text{
    max-width: 516px;
    margin: 0 auto;
    padding-top: 50px;
    line-height: 36px;
    letter-spacing: 0.1em;
    color: $textMuted;
}
.img{
    margin-top: 60px;
}
.cocktails{
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    max-height: 400px;
    overflow-y: auto;
    margin-top: 60px;
    &::-webkit-scrollbar {
        width: 5px;
    }
    &::-webkit-scrollbar-track {
        -webkit-box-shadow: 5px 5px 5px -5px rgba(34, 60, 80, 0.2) inset;
        background-color: #f9f9fd;
        border-radius: 10px;
    }
    &::-webkit-scrollbar-thumb {
        border-radius: 10px;
        background: linear-gradient(180deg, $accent, #a90187);
    }
}
</style>