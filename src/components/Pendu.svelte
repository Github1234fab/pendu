<script>
	import { fade } from 'svelte/transition';

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

	const wordStock = [
		'stock',
		'bateau',
		'rugby',
		'homme',
		'papillote',
		'chalutier',
		'fourchette',
		'bureau',
		'anis',
		'sortir'
	];
	const wordTab = ['tableau'];

	initializeGame();


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
	<div class="wrapper-interface">
		<label for="word">Saisissez une lettre</label>
		<input bind:value={inputValue} on:input={isLetterOk}  type="text" placeholder="" name="word " />
		<!-- <button on:click={isLetterOk}>Valider</button> -->

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
	.wrapper-pendu {
		background-color: rgb(43, 43, 43);
		min-height: 250px;
		max-width: 300px;
		position: relative;
		margin: 20px auto;
		box-shadow: inset 0 0 10px black;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.wrapper-interface {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		width: 100%;
		height: 200px;
	}
	label {
		font-size: 0.7rem;
		color: white;
		margin-top: 20px;
		margin-bottom: 5px;
	}
	input {
		margin-bottom: 10px;
		padding: 5px;
		font-size: 2rem;
		box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.443);
		max-width: 40px;
		color: black;
		text-transform: uppercase;
	}
	input::placeholder {
		font-size: 1rem;
		text-align: center;
	}

	.display-word {
		font-size: 1rem;
		color: white;
		letter-spacing: 5px;
		margin-top: 10px;
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
		background-color: rgba(0, 128, 0, 0.968);
		padding: 100px 70px;
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
		transition: opacity 0.3s ease-in-out; /* Animation pour l'apparition */
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
		left: 42%;
		height: 50px;
		width: 10px;
	}

	.center {
		top: 50%;
		left: 50%;
		width: 10px;
		height: 150px;
	}

	.potence {
		rotate: 90deg;
		top: 12%;
		left: 47%;
		height: 52px;
		width: 10px;
	}

	.down {
		top: 24.5%;
		left: 65%;
		height: 30px;
		width: 10px;
	}

	.circle {
		top: 35%;
		left: 65%;
		height: 30px;
		width: 30px;
		border-radius: 50%;
	}

	.body-center {
		top: 54%;
		left: 65%;
		width: 10px;
		height: 60px;
	}

	.body-left {
		rotate: 40deg;
		top: 50%;
		left: 57%;
		width: 10px;
		height: 32px;
	}

	.body-right {
		rotate: -35deg;
		top: 48%;
		left: 71%;
		width: 10px;
		height: 30px;
	}

	.body-right-down {
		rotate: 30deg;
		top: 70%;
		left: 58%;
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
