<script setup>
import { reactive, ref } from 'vue';

const search_input = ref("hola");
let gifs = reactive([]);
async function getSearch() {
    const API_KEY = import.meta.env.VITE_API_TENOR;
    const CLIENT_KEY = "test-app";
    const URL = "https://tenor.googleapis.com/v2/search?q=";
    const LIMIT = 10;

    try {
        const response = await fetch(
            `${URL}${search_input.value}&key=${API_KEY}&client_key=${CLIENT_KEY}&limit=${LIMIT}`,
            {
                method: "GET"
            }
        );

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        console.log(data);
        gifs.length = 0;
        gifs.push(...data.results)

        // Handle the retrieved GIFs as needed, e.g., update a reactive variable
        // that you can use to display the GIFs in the template.
    } catch (error) {
        console.error("Error fetching GIFs:", error.message);
    }
}
</script>
<template>
    <div>
        <input v-model="search_input" type="text" placeholder="Type here">
        <button @click="getSearch">Find gifs</button>
        <div class="gifs">
            <ul>
                <li v-for="gif in gifs">
                    <a :href="gif.url">
                        <img :src="gif.media_formats.tinygif.url" alt="">
                    </a>
                </li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
.gifs ul {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

ul li {
    box-sizing: border-box;
    list-style-type: none;
}

img {
    display: block;
    box-sizing: border-box;
    border-radius: 8px;
    border: 1px solid;
    padding: 0.6em 1.2em;
    font-family: inherit;
    cursor: pointer;
}
</style>
  
