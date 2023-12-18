<script>
	const replaceTable = {
		0: 0,
		1: 1,
		2: 2,
		3: 3,
		4: 4,
		5: 5,
		6: 6,
		7: 7,
		8: 8,
		9: 9,
		one: 1,
		two: 2,
		three: 3,
		four: 4,
		five: 5,
		six: 6,
		seven: 7,
		eight: 8,
		nine: 9
	};

	async function load() {
		const text = await (await fetch('./1/input-file')).text();
		const lines = text.split('\n');
		let answer = 0;

		const regex = /(one|two|three|four|five|six|seven|eight|nine|\d)/;
		const xeger = /(\d|enin|thgie|neves|xis|evif|ruof|eerht|owt|eno)/;

		for (const line of lines) {
			const matches = line.match(regex);
			const matchesReversed = line.split('').reverse().join('').match(xeger);

			const firstDigit = replaceTable[matches[0]];
			const lastDigit = replaceTable[matchesReversed[0].split('').reverse().join('')];

            answer += (firstDigit + "" + lastDigit) * 1;
		}

		return answer;
	}
</script>

{#await load()}
	loading...
{:then answer}
	Answer: {answer}
{/await}
