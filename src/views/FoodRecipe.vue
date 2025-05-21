<template>
    <nav class="fixed top-0 w-full h-16 bg-gradient-to-r from-blue-500 to-blue-300 shadow-md z-10">
        <div class="flex items-center justify-between h-full px-4">
            <button @click="back"
                class="text-white lg:bg-transparent lg:border-2 lg:border-yellow-600 lg:py-1 lg:px-6 hover:scale-105 transition-transform">Back</button>
            <!-- <div class="flex items-center">
                <input type="text" placeholder="Search for meal..." v-model="query" @input="fetchFood"
                    class="outline-none w-72 py-2 px-4 rounded-full bg-white shadow-md transition-colors duration-300 focus:bg-gray-100" />
                <div class="ml-2 cursor-pointer" @click="fetchFood">
                    <i class="pi pi-search text-black mt-1 text-lg"></i>
                </div>
            </div> -->
        </div>
    </nav>

    <div class="font-serif mb-20 pt-10">
        <img src="/background.jpg" alt="" class="fixed h-screen w-screen hidden lg:block" />
        <img src="/background2.jpg" alt="" class="fixed h-screen w-screen block lg:hidden">
        <ul v-if="!showDetail" class="relative lg:mt-2">
            <li v-for="meal in food" :key="meal.idMeal" class="mb-4 grid grid-cols-2 gap-4">
                <div class="text-green-200 lg:text-amber-600 ">
                    <div @click="selectMeal(meal)"
                        class="border-2 border-green-200 w-72 ml-10  flex lg:border-2 lg:border-green-200 lg:w-full lg:mx-72 lg:mr-64 hover:cursor-pointer hover:scale-105 transition-transform">
                        <img :src="meal.strMealThumb" alt="image"
                            class="w-20 h-20 rounded-full lg:w-64 lg:h-64 lg:rounded-full lg:ml-10">
                        <div class="lg:flex flex">
                            <p class="mt-4 lg:ml-12 lg:text-lg lg:mt-20 text-sm mr-2"><span
                                    class="lg:text-green-600 mr-2 ml-2 lg:text-xl text-sm ">Name:</span> <span
                                    class="ml-2"> {{ meal.strMeal }}</span></p>
                            <p class="mt-4 lg:ml-10 lg:text-lg lg:mt-20 text-sm mr-2"><span
                                    class="lg:text-green-600 mr-2 text-sm lg:text-xl ">Category:</span> <span
                                    class="ml-1">{{ meal.strCategory }}</span></p>
                            <p class="mt-4 lg:ml-10 lg:text-lg lg:mt-20 text-sm mr-2"><span
                                    class="lg:text-green-600 text-sm lg:text-xl">Area:</span> {{ meal.strArea }}</p>
                        </div>
                    </div>
                    <p class="cursor-pointer">{{ meal.strDrinkAlternate }}</p>
                </div>
            </li>
        </ul>

        <div v-if="selectedMeal && showDetail"
            class="meal-details relative bg-transparent text-white lg:mt-20 lg:ml-40">
            <div class="lg:flex mb-16">
                <div class="flex -mt-10 lg:mt-10">
                    <img :src="selectedMeal.strMealThumb" alt="Meal image"
                        class=" rounded-full ml-6 lg:rounded-full lg:-ml-20" />
                    <h1 class="ml-6 text-2xl font-bold mt-20 lg:ml-10 lg:mt-40 lg:animate-pulse lg:text-green-600">{{
                        selectedMeal.strMeal }}</h1>
                </div>
                <div class="w-64 border-2 border-green-200 rounded-md mt-2 lg:mt-28 ml-10 shadow-xl lg:ml- ">
                    <h2 class="text-green-600 font-bold lg:mt-10 lg:text-xl text-center">Ingredients</h2>
                    <ol class="lg:text-yellow-400 text-center px-10 ">
                        <li v-for="(ingredient, index) in getIngredients(selectedMeal)" :key="index">{{ ingredient }}
                        </li>
                    </ol>
                </div>
            </div>
            <div
                class="w-80 border-2 border-green-200 lg:border-none lg:w-full lg:h-full rounded-md mt-2 ml-2 shadow-xl">
                <h2 class="text-green-600 font-bold lg:text-xl">Instructions</h2>
                <p class="lg:w-1/2 lg:ml- lg:text-amber-400">{{ selectedMeal.strInstructions }}</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            food: [],
            query: '',
            selectedMeal: null,
            showDetail: false,
        };
    },
    mounted() {
        this.fetchFood();
    },
    methods: {
        fetchFood() {
            const url = `https://www.themealdb.com/api/json/v1/1/search.php?s=${this.query}`;

            axios.get(url)
                .then(res => {
                    console.log(res.data);
                    this.food = res.data.meals || [];
                    this.selectedMeal = null;
                })
                .catch(error => {
                    console.log('The problem is', error);
                });
        },
        selectMeal(meal) {
            this.selectedMeal = meal;
            this.showDetail = true;
            console.log('Selected Meal:', this.selectedMeal);
        },
        getIngredients(meal) {
            return Object.keys(meal)
                .filter(key => key.startsWith('strIngredient') && meal[key])
                .map(key => meal[key]);
        },
        back() {
            this.showDetail = false;
            this.query = '';
            this.fetchFood();
        }
    },
};
</script>

<style scoped>
.meal-details {
    margin-top: 20px;
    padding: 10px;
}
</style>