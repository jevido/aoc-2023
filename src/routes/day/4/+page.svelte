<script>
	async function load() {
		let answerPart1 = 0;
		let answerPart2 = 0;

		const text = await (await fetch('4/input-file')).text();
		const cards = text.split('\n');

		for (const card of cards) {
			const cardData = card.split(': ');
			const cardId = cardData[0].replace('Card ', '');
			const cardSplit = cardData[1].split(' | ');
			const cardWinners = cardSplit[0].split(' ');
			const cardPossible = cardSplit[1].split(' ');
			let cardPoints = 0;

			for (const winningCard of cardWinners) {
				if (winningCard === '') continue;

				if (cardPossible.includes(winningCard)) {
					if (cardPoints === 0) {
						cardPoints = 1;
					} else {
						cardPoints += cardPoints;
					}
				}
			}

			answerPart1 += cardPoints;
		}

		return [answerPart1, answerPart2];
	}
</script>

{#await load()}
	Loading
{:then [part1, part2]}
	Answer part1: {part1} <br />
	Answer part1: {part2}
{/await}
