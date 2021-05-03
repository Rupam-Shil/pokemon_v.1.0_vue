<template>
	<div class="w-full flex justify-center">
		<input
			type="text"
			placeholder="Enter a Pokemon Here"
			class="mt-10 p-2 border-blue-500 border-2"
			v-model="text"
		/>
	</div>
	<div class="mt-10 p-4 flex flex-wrap justify-center">
		<div
			v-for="(pokemon, idx) in filteredPokemon"
			:key="idx"
			class="ml-4 text-2x text-blue-500"
		>
			<router-link :to="`/about/${urlIdlookup[pokemon.name]}`">
				{{ pokemon.name }}
			</router-link>
		</div>
	</div>
</template>

<script>
import { computed, reactive, toRefs } from 'vue';
export default {
	name: 'Home',
	setup() {
		const state = reactive({
			pokemons: [],
			urlIdlookup: {},
			text: '',
			filteredPokemon: computed(() => updatePokemon()),
		});

		function updatePokemon() {
			if (!state.text) {
				return [];
			}

			return state.pokemons.filter((pokemon) =>
				pokemon.name.includes(state.text)
			);
		}

		fetch('https://pokeapi.co/api/v2/pokemon')
			.then((res) => res.json())
			.then((data) => {
				state.pokemons = data.results;
				state.urlIdlookup = data.results.reduce(
					(acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
					{}
				);
			});
		return { ...toRefs(state) };
	},
};
</script>
