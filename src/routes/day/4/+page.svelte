<script>
	async function load() {
		let answerPart1 = 0;
		let answerPart2 = 2;

		return [answerPart1, answerPart2];
	}
</script>

{#await load()}
	Loading
{:then [part1, part2]}
	Answer part1: {part1} <br />
	Answer part1: {part2}
{/await}
