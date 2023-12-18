<script>
	async function load() {
		let answerPart1 = 0;
		let answerPart2 = 0;

		const text = await (await fetch('2/input-file')).text();
		const lines = text.split('\n');
		let bags = parseBags(lines);

		const required = {
			red: 12,
			green: 13,
			blue: 14
		};

		for (const [gameId, game] of Object.entries(bags)) {
			let bagIsAight = true;

			let maxRed = 0;
			let maxGreen = 0;
			let maxBlue = 0;

			for (const [_, bag] of Object.entries(game)) {
				if (bag.red > required.red || bag.green > required.green || bag.blue > required.blue) {
					bagIsAight = false;
				}

				if (bag.red) {
					maxRed = Math.max(bag.red, maxRed);
				}
				if (bag.green) {
					maxGreen = Math.max(bag.green, maxGreen);
				}
				if (bag.blue) {
					maxBlue = Math.max(bag.blue, maxBlue);
				}
			}

			if (bagIsAight) {
				answerPart1 += gameId * 1;
			}
			console.debug(gameId + ': ', maxRed, maxGreen, maxBlue);
			answerPart2 += maxRed * maxGreen * maxBlue;
		}

		console.debug({ answerPart1, answerPart2 });
		return [answerPart1, answerPart2];
	}

	function parseBags(lines) {
		const bags = {};
		for (const line of lines) {
			let remaining = line.split('Game ')[1];
			const [gameId, stringSet] = remaining.split(': ');
			const games = stringSet.split('; ');
			bags[gameId] = {};

			for (const index in games) {
				const game = games[index];
				const subsets = game.split(', ');
				bags[gameId][index] = {};

				for (const subset of subsets) {
					const [amount, color] = subset.split(' ');
					bags[gameId][index][color] = amount * 1;
				}
			}
		}
		return bags;
	}
</script>

{#await load()}
	Calculating...
{:then [part1, part2]}
	Part 1: {part1} <br />
	Part 2: {part2}
{/await}
