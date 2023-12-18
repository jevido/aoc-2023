<script>
	async function load() {
		let answerPart1 = 0;
		let answerPart2 = 0;
		const text = await (await fetch('3/input-file')).text();

		const lines = text.split('\n');
		const data = lines.map((line) => line.split(''));

		let currentNumber = '';

		for (let row in data) {
			row *= 1;
			for (let column in data[row]) {
				column *= 1;
				const character = data[row][column];

				if (isNumber(character)) {
					currentNumber += character;
				} else {
					// The current string can't possibly be at 0 it was recorded on the previous line
					const rowOffsetted = column === 0 ? row - 1 : row;
					const columnOffsetted = column === 0 ? data[rowOffsetted]?.length : column;

					if (currentNumber !== '') {
						// Snake around the currentnumber
						const numberLength = currentNumber.length;
						const leftChar = data[rowOffsetted][columnOffsetted - numberLength - 1];
						const rightChar = data[rowOffsetted][columnOffsetted];

						if (isSpecial(rightChar) || isSpecial(leftChar)) {
							save(currentNumber);
						} else {
							for (let index = -1; index < numberLength + 1; index++) {
								const currentlyAt = index + columnOffsetted - numberLength;

								const upperChar =
									data[rowOffsetted - 1] !== undefined
										? data[rowOffsetted - 1][currentlyAt] ?? '.'
										: '.';
								const lowerChar =
									data[rowOffsetted + 1] !== undefined
										? data[rowOffsetted + 1][currentlyAt] ?? '.'
										: '.';

								if (isSpecial(upperChar) || isSpecial(lowerChar)) {
									save(currentNumber);
									break;
								}
							}
						}
					}

					currentNumber = '';
				}
			}
		}

		function save(number) {
			console.debug(`${answerPart1} + ${number}`);
			answerPart1 += number * 1;
		}
		return [answerPart1, answerPart2];
	}

	function isNumber(char) {
		// doesn't work for empty strings
		return /[\d]/.test(char);
	}
	function isIgnored(char) {
		return char === '.';
	}
	function isSpecial(char) {
		if (char === undefined) return false;
		if (isNumber(char)) return false;
		if (!isIgnored(char)) {
			return true;
		}

		return false;
	}
</script>

{#await load()}
	Calulating...
{:then [part1]}
	Answer: {part1}
{/await}

<br /><br />
I have tried:
<ul>
	<li>533784</li>
	<li>533784</li>
</ul>
