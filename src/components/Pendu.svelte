<script>
	import { fade } from 'svelte/transition';

	let userLettersStock = [
		'A',
		'B',
		'C',
		'D',
		'E',
		'F',
		'G',
		'H',
		'I',
		'J',
		'K',
		'L',
		'M',
		'N',
		'O',
		'P',
		'Q',
		'R',
		'S',
		'T',
		'U',
		'V',
		'W',
		'X',
		'Y',
		'Z'
	];
	let inputValue = '';

	let displayWordToFind = '';
	let guessedLetters = [];
	let winMessage = '';
	let wordStockedRandom = '';
	let badAnswer = 0;
	let loseMessage = '';
	let penduParts = [];
	let wrapperPendu = false;
	let hidden = false;
	let inputValueStock = [];

	const wordStock = [
		'jouet',
		'poivre',
		'sel',
		'cartable',
		'meringue',
		'couteau',
		'mitraillette',
		'fusil',
		'Bougie',
		'Ballon'
	];
	const wordTab = ['tableau'];

	initializeGame();

	function userLettersInInputValue(letter) {
		inputValue = letter;
		isLetterOk();
		console.log(letter);
		console.log(inputValue);
	}

	function initializeGame() {
		guessedLetters = [];
		inputValue = '';
		winMessage = '';
		let newWord = wordStock[Math.floor(Math.random() * wordStock.length)];
		wordTab.length = 0;
		wordTab.push(newWord);
		displayWordToFind = null;
		loseMessage = '';
		badAnswer = 0;
		inputValueStock = [];
		displayWordToFind = wordTab[0]
			.split('')
			.map((el, index) => {
				if (index === 0 || index === wordTab[0].length - 1) {
					return el.toUpperCase();
				}
				return '-';
			})
			.join('');
		console.log('Nouveau mot à deviner :', displayWordToFind);
	}

	function isLetterOk() {
		const letterRegex = /^[a-zA-Z]$/;

		if (!letterRegex.test(inputValue)) {
			console.log('Veuillez saisir uniquement une lettre.');
			inputValue = ''; // Réinitialise l'entrée si ce n'est pas une lettre
			return;
		}

		let userLetter = inputValue.toLowerCase();
		inputValueStock.push(userLetter );
		inputValueStock = [...inputValueStock];
		if (!inputValue) return;
		const allLetters = wordTab[0].split('');
		if (allLetters.includes(inputValue.toLowerCase())) {
			let letterOk = inputValue.toLowerCase();
			guessedLetters.push(letterOk);
			displayWordToFind = wordTab[0]
				.split('')
				.map((el, index) => {
					if (index === 0 || index === wordTab[0].length - 1) {
						return el.toUpperCase();
					}
					return guessedLetters.includes(el) ? el.toUpperCase() : '-';
				})
				.join('');
		} else {
			console.log('Lettre non trouvée');
			badAnswer++;
		}

		setTimeout(() => {
			inputValue = '';
		}, 500);

		if (displayWordToFind === wordTab[0].toUpperCase()) {
			winMessage = 'Bravo!';
			console.log(winMessage);
		}

		if (badAnswer == 12) {
			loseMessage = 'Perdu!';
			loseMessage = wordTab[0].toUpperCase();
		}
	}
</script>

<div class="container">
	<h1 class="title">Le pendu</h1>
	<div class="wrapper-interface">
		<div class="wrapper__user-letter-selected">
			{#each userLettersStock as letter}
				<button class="lettres-choice-user" on:click={() => userLettersInInputValue(letter)}
					>{letter}</button
				>
			{/each}
		</div>
		<p class="letters-selected">{inputValueStock}</p>
		<p class="display-word">{displayWordToFind}</p>
		<p class="win-message">{winMessage}</p>
		{#if loseMessage}
			<div class="lose-message">{loseMessage}</div>
		{/if}
	</div>
	<div class="wrapper-pendu">
		{#if winMessage || loseMessage}
			<button class="button__new-game" on:click={initializeGame} transition:fade={{ duration: 500 }}
				>Nouvelle Partie</button
			>
		{/if}
		<div class="horizontal style" class:visible={badAnswer >= 1}></div>
		<div class="left style" class:visible={badAnswer >= 2}></div>
		<div class="right style" class:visible={badAnswer >= 3}></div>
		<div class="center style" class:visible={badAnswer >= 4}></div>
		<div class="potence style" class:visible={badAnswer >= 5}></div>
		<div class="down style" class:visible={badAnswer >= 6}></div>
		<div class="circle style" class:visible={badAnswer >= 7}></div>
		<div class="body-center style" class:visible={badAnswer >= 8}></div>
		<div class="body-left style" class:visible={badAnswer >= 9}></div>
		<div class="body-right style" class:visible={badAnswer >= 10}></div>
		<div class="body-right-down style" class:visible={badAnswer >= 11}></div>
		<div class="body-left-down style" class:visible={badAnswer >= 12}></div>
	</div>
</div>

<style>
	.title {
		font-family: roboto;
		letter-spacing: -1px;
		text-align: center;
		color: white;
		font-size: 2rem;
		font-weight: 100;
		margin-top: 20px;
	}
	.wrapper-interface {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		width: 100%;
		height: 200px;
		margin-top: 10px;
		padding: 8px;
	}
	.wrapper__user-letter-selected {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		flex-wrap: wrap;
		gap: 5px;
		margin-top: 60px;
	}
	.lettres-choice-user {
		background-color: brown;
		color: whitesmoke;
		font-size: 1rem;
		min-width: 20px;
		padding: 5px 2px;
		border-radius: 5px;
		border: none;
		box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.393);
	}
		.lettres-choice-user:hover {
		animation: bounce 0.2s ease-in-out;
	}
		.lettres-choice-user:focus{
border: 1px solid white;
	}
	.letters-selected{
		font-size: 1rem;
		color: greenyellow;
		text-align: center;
		margin-top: 20px;
	}

	@keyframes bounce {
		0% {
			transform: translateY(0);
		}
		50% {
			transform: translateY(-3px);
		}
		100% {
			transform: translateY(0);
		}
	}

	.wrapper-pendu {
		background-color: rgb(43, 43, 43);
		min-height: 250px;
		max-width: 280px;
		position: relative;
		margin: 80px auto;
		box-shadow: inset 0 0 10px black;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.display-word {
		font-weight: 600;
		font-size: 2rem;
		color: white;
		letter-spacing: 10px;
		margin-top: 50px;
		text-shadow: 0px 0px 20px white;
	}
	.win-message,
	.lose-message {
		font-size: 1rem;
		color: rgb(190, 213, 19);
		letter-spacing: 5px;
		margin: 0px auto;
		padding: 5px;
		font-weight: bold;
	}
	.button__new-game {
		margin-top: 0px;
		background-color: rgb(0, 128, 0);
		min-height: 228px;
		min-width: 270px;
		color: white;
		border: none;
		border-radius: 8px;
		box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.443);
		z-index: 2;
		opacity: 1;
		transition: opacity 0.5s ease-in-out;
		font-size: 1rem;
	}
	.style {
		z-index: 1;
		position: absolute;
		transform: translate(-50%, -50%);
		background-color: white;
		visibility: hidden;
		opacity: 0;
		transition:
			visibility 0s 0.4s,
			opacity 0.4s ease-in-out; /* Transition pour la visibilité */
	}
	.style.visible {
		visibility: visible;
		opacity: 1;
		transition: opacity 0.5s ease-in-out; /* Animation pour l'apparition */
	}

	.horizontal {
		rotate: 90deg;
		top: 85%;
		left: 40%;
		height: 50.5px;
		width: 10px;
	}

	.left {
		rotate: -29deg;
		top: 83%;
		left: 58%;
		height: 50px;
		width: 10px;
	}

	.right {
		rotate: 29deg;
		top: 85%;
		left: 43%;
		height: 50px;
		width: 10px;
	}

	.center {
		top: 50%;
		left: 50.5%;
		width: 10px;
		height: 150px;
	}

	.potence {
		rotate: 90deg;
		top: 12.8%;
		left: 47%;
		height: 52px;
		width: 10px;
	}

	.down {
		top: 25%;
		left: 66%;
		height: 30px;
		width: 10px;
	}

	.circle {
		top: 37%;
		left: 66%;
		height: 30px;
		width: 30px;
		border-radius: 50%;
	}

	.body-center {
		top: 54%;
		left: 66%;
		width: 10px;
		height: 60px;
	}

	.body-left {
		rotate: 40deg;
		top: 50%;
		left: 58%;
		width: 10px;
		height: 32px;
	}

	.body-right {
		rotate: -35deg;
		top: 48%;
		left: 72%;
		width: 10px;
		height: 30px;
	}

	.body-right-down {
		rotate: 30deg;
		top: 70%;
		left: 59%;
		width: 10px;
		height: 40px;
	}

	.body-left-down {
		rotate: -30deg;
		top: 68%;
		left: 72%;
		width: 10px;
		height: 40px;
	}
</style>
