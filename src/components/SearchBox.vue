<script setup>
import { ref, watch } from 'vue'
import axios from "axios";
import { debounce } from 'lodash/function'

const searchInput = ref('')
const cities = ref([])
const matchedCities = ref([])
const loading = ref(false)

watch(searchInput, debounce(async () => {
    await checkInput()
}, 100), { deep: true })

async function checkInput() {
    if (searchInput.value.trim() === '') {
        matchedCities.value = [];
    } else {
        await getCities()
    }
}

async function getCities() {
    const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';
    axios
        .get(endpoint)
        .then((response) => {
            if (response.status === 200 && response.data.length !== 0) {
                cities.value = response.data;
                findMatchedCities();
            } else if (response.data.length === 0) {
                cities.value = [];
            }
        })
        .catch((error) => {
            console.error('Error Message :', error.message);
        });
}

function findMatchedCities() {
    if (cities.value.length !== 0) {
        const queryCondition = searchInput.value.toLowerCase();
        matchedCities.value = cities.value.filter((item) => {
            const regex = new RegExp(queryCondition, 'gi');
            return item.city.match(regex) || item.state.match(regex);
        });
        highlight();
    }
}

function highlight() {
    matchedCities.value.forEach((item) => {
        item.cityIndexStart = -1;
        item.cityIndexEnd = -1;
        item.stateIndexStart = -1;
        item.stateIndexEnd = -1;

        const queryCondition = searchInput.value.toLowerCase();

        const cityIndex = item.city.toLowerCase().indexOf(queryCondition);
        if (cityIndex !== -1) {
            item.cityIndexStart = cityIndex;
            item.cityIndexEnd = cityIndex + queryCondition.length;
        }

        const stateIndex = item.state.toLowerCase().indexOf(queryCondition);
        if (stateIndex !== -1) {
            item.stateIndexStart = stateIndex;
            item.stateIndexEnd = stateIndex + queryCondition.length;
        }
    });
}

function formatNumber(population) {
    return population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

</script>

<template>
    <div class="search-form">
        <div>
            <input type="text" class="search" placeholder="City or State" v-model="searchInput" @input="(e) => {
                searchInput = e.target.value.trimStart()
            }" />
        </div>

        <div v-if="matchedCities.length !== 0">
            <ul class="suggestions">
                <li v-for="city in matchedCities" :key="city">
                    <div>
                        <span>
                            {{ city.city.substring(0, city.cityIndexStart) }}
                        </span>
                        <span class="highlight">
                            {{ city.city.substring(city.cityIndexStart, city.cityIndexEnd) }}
                        </span>
                        <span> {{ city.city.substring(city.cityIndexEnd) }}, </span>
                        <span>
                            {{ city.state.substring(0, city.stateIndexStart) }}
                        </span>
                        <span class="highlight">
                            {{ city.state.substring(city.stateIndexStart, city.stateIndexEnd) }}
                        </span>
                        <span> {{ city.state.substring(city.stateIndexEnd) }} </span>
                    </div>
                    <span>{{ formatNumber(city.population) }}</span>
                </li>
            </ul>
        </div>
        <div v-else>
            <div v-if="searchInput.length == 0">
                <ul class="suggestions">
                    <li>Filter for a city</li>
                    <li>or a state</li>
                </ul>
            </div>
            <div v-else>
                <ul class="suggestions">
                    <p>No data available</p>
                </ul>
            </div>
        </div>
    </div>
</template>