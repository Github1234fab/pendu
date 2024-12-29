<script>
	let inputValue = '';
	let displayWordToFind = '';
	let guessedLetters = [];
	let winMessage = '';
	let wordStockedRandom = '';
	let badAnswer = 0;
	let loseMessage = '';
	console.log(badAnswer);

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
	const wordTab = ['tableau']; // Initialise avec un premier mot

	// Fonction pour initialiser ou réinitialiser une partie
	function initializeGame() {
		guessedLetters = [];
		inputValue = '';
		winMessage = ''; // Réinitialisation du message
		let newWord = wordStock[Math.floor(Math.random() * wordStock.length)]; // Nouveau mot
		wordTab.length = 0; // Vide le tableau précédent
		wordTab.push(newWord); // Ajoute le nouveau mot

		// Prépare l'affichage masqué (révèle seulement la 1ère et dernière lettre)
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

	// Initialise la première partie
	initializeGame();

	function isLetterOk() {
		if (!inputValue) return; // Ne rien faire si l'input est vide

		const allLetters = wordTab[0].split(''); // Récupère les lettres du mot
		console.log(allLetters);

		if (allLetters.includes(inputValue.toLowerCase())) {
			let letterOk = inputValue.toLowerCase();
			guessedLetters.push(letterOk);
			console.log(`Lettre trouvée : ${letterOk}`);

			// Met à jour l'affichage du mot
			displayWordToFind = wordTab[0]
				.split('')
				.map((el, index) => {
					if (index === 0 || index === wordTab[0].length - 1) {
						return el.toUpperCase();
					}
					return guessedLetters.includes(el) ? el.toUpperCase() : '-';
				})
				.join('');

			console.log(`Mot révélé : ${displayWordToFind}`);
		} else {
			console.log('Lettre non trouvée');
			badAnswer++;
			console.log(badAnswer);
			if (badAnswer === 1) {
				document.querySelector('.horizontal').style.display = 'block';
			}
		}

			// Attends 300 ms avant de vider l'input
	setTimeout(() => {
		inputValue = '';
	}, 1500);

		// Vérifie si le mot entier est deviné
		if (displayWordToFind === wordTab[0].toUpperCase()) {
			winMessage = 'Bravo, vous avez trouvé le mot !';
			console.log(winMessage);
		}
	}

	$: {
		if (badAnswer === 12) {
			loseMessage = wordTab[0].toUpperCase();
			console.log(loseMessage);
		}
	}
</script>

<div class="container">
<div class="wrapper-interface">
	<input bind:value={inputValue} on:input={isLetterOk} type="text" placeholder="Lettre" />
	<!-- <button on:click={isLetterOk}>Valider</button> -->

	<p class="display-word">{displayWordToFind}</p>
	<p class="win-message">{winMessage}</p>
	{#if loseMessage}
		<div class="lose-message">{loseMessage}</div>
	{/if}

{#if winMessage || loseMessage}
    <button on:click={initializeGame}>Nouvelle Partie</button>
{/if}
</div>
<div class="wrapper-pendu">
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
		min-height: 300px;
		max-width: 300px;
		position: relative;
		margin: 0 auto;
		box-shadow: inset 0 0 10px black;
	}
	.wrapper-interface {
		margin: 30px auto;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		width: 100%;
		height: 300px;
	}
	input {
		margin-bottom: 10px;
		padding: 5px;
		font-size: 2rem;
		box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.443);
		max-width: 150px;
                color: black;
	}
	input::placeholder {
		font-size: 1rem;
		text-align: center;
	}
	button {
		padding: 10px 20px;
		font-size: 1.5rem;
		background-color: #4caf50;
		color: white;
		border: none;
		cursor: pointer;
		max-width: 150px;
		border-radius: 5px;
		text-align: center;
		box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.443);
	}
	.display-word {
		font-size: 2rem;
		color: white;
		letter-spacing: 5px;
		margin-top: 30px;
	}
	.win-message,
	.lose-message {
		font-size: 2rem;
		color: white;
		letter-spacing: 5px;
		margin: 0px auto;
	}
	.style {
		position: absolute;
		transform: translate(-50%, -50%);
		background-color: white;
		visibility: hidden;
		opacity: 0;
		transition:
			visibility 0s 0.3s,
			opacity 0.3s ease-in-out; /* Transition pour la visibilité */
	}

	.style.visible {
		visibility: visible;
		opacity: 1;
		transition: opacity 0.3s ease-in-out; /* Animation pour l'apparition */
	}

	.horizontal {
		rotate: 90deg;
		top: 55.5%;
		left: 35.4%;
		height: 50.5px;
		width: 10px;
	}

	.left {
		rotate: -29deg;
		top: 54%;
		left: 53%;
		height: 50px;
		width: 10px;
	}

	.right {
		rotate: 29deg;
		top: 55.7%;
		left: 37%;
		height: 50px;
		width: 10px;
	}

	.center {
		top: 41%;
		left: 45.5%;
		width: 10px;
		height: 160px;
	}

	.potence {
		rotate: 90deg;
		top: 17%;
		left: 42.2%;
		height: 50px;
		width: 10px;
	}

	.down {
		top: 24.5%;
		left: 58.8%;
		height: 30px;
		width: 10px;
	}

	.circle {
		top: 31%;
		left: 59%;
		height: 30px;
		width: 30px;
		border-radius: 50%;
	}

	.body-center {
		top: 36%;
		left: 59%;
		width: 10px;
		height: 100px;
	}

	.body-left {
		rotate: 40deg;
		top: 40%;
		left: 52%;
		width: 10px;
		height: 32px;
	}

	.body-right {
		rotate: -35deg;
		top: 40%;
		left: 65%;
		width: 10px;
		height: 30px;
	}

	.body-right-down {
		rotate: 30deg;
		top: 50%;
		left: 54%;
		width: 10px;
		height: 30px;
	}

	.body-left-down {
		rotate: -30deg;
		top: 49%;
		left: 64%;
		width: 10px;
		height: 30px;
	}
</style>
