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
			answerPart1 += number * 1;
		}

		for (const rowIndexString in data) {
			const rowIndex = Number(rowIndexString);
			const row = data[rowIndex];
			// Find gears (*)

			for (const columnIndex in row) {
				const column = Number(columnIndex);
				const character = row[column];

				if (character === '*') {
					const topLeft = data[rowIndex - 1][column - 1];
					const topMiddle = data[rowIndex - 1][column];
					const topRight = data[rowIndex - 1][column + 1];
					const middleLeft = data[rowIndex][column - 1];
					const middleRight = data[rowIndex][column + 1];
					const bottomLeft = data[rowIndex + 1][column - 1];
					const bottomMiddle = data[rowIndex + 1][column];
					const bottomRight = data[rowIndex + 1][column + 1];

					let topNumbers = [];
					let middleNumbers = [];
					let bottomNumbers = [];

					if (isNumber(topLeft)) {
						const number = scanStringReverse(rowIndex - 1, column - 1);
						topNumbers.push(number);
					}

					if (isNumber(topMiddle)) {
						if (topNumbers.length !== 0) {
							topNumbers[0] += topMiddle;
						} else {
							topNumbers.push(topMiddle);
						}
					}

					if (isNumber(topRight)) {
						const number = scanString(rowIndex - 1, column + 1);
						if (isNumber(topMiddle)) {
							topNumbers[0] += number;
						} else {
							topNumbers.push(number);
						}
					}

					if (isNumber(middleLeft)) {
						const number = scanStringReverse(rowIndex, column - 1);
						middleNumbers.push(number);
					}
					if (isNumber(middleRight)) {
						const number = scanString(rowIndex, column + 1);
						middleNumbers.push(number);
					}

					if (isNumber(bottomLeft)) {
						const number = scanStringReverse(rowIndex + 1, column - 1);
						bottomNumbers.push(number);
					}

					if (isNumber(bottomMiddle)) {
						if (bottomNumbers.length !== 0) {
							bottomNumbers[0] += bottomMiddle;
						} else {
							bottomNumbers.push(bottomMiddle);
						}
					}

					if (isNumber(bottomRight)) {
						const number = scanString(rowIndex + 1, column + 1);
						if (isNumber(bottomMiddle)) {
							bottomNumbers[0] += number;
						} else {
							bottomNumbers.push(number);
						}
					}

					let totalNumbers = [...topNumbers, ...middleNumbers, ...bottomNumbers];

					if (totalNumbers.length == 2) {
						answerPart2 += totalNumbers[0] * totalNumbers[1];
					}
				}
			}
		}

		return [answerPart1, answerPart2];

		function scanStringReverse(row, column) {
			let number = '';

			for (let breakdown = column; breakdown >= 0; breakdown--) {
				const char = data[row][breakdown];

				if (isNumber(char)) {
					number = char + number;
				} else {
					break;
				}
			}

			return number;
		}

		function scanString(row, column) {
			let number = '';

			for (let index = column; index < data[row].length; index++) {
				const char = data[row][index];

				if (isNumber(char)) {
					number += char;
				} else {
					break;
				}
			}
			return number;
		}
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
{:then [part1, part2]}
	Answer part 1: {part1} <br />
	Answer part 2: {part2}
{/await}
