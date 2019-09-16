<script>
	import { fade } from 'svelte/transition';
	// Init variables
	const buttons = ['red', 'blue', 'green', 'yellow'];

	let gameOn = false;
	let round = 0
	let currentCall = '';	
	let buttonsActive = false;
	let callList = [];
	let message = '';

	// Game Loop
	function startGame() {
		message = ''
		gameOn = true;
		round = 1;
		callList = [];

		startRound();	
	}
	
	function startRound() {
		callLoop();
		responseLoop();
	};

	function callDelay() {
		setTimeout(() => {
			
		}, 1000);
	}

	// Call Loop
	function callLoop() {
		buttonsActive = false;
		let buttonIndex = parseInt(Math.random() * 4, 10);
		let callIndex = 0;
		console.log('callLoop init callList:' + callList);
		callDelay = setInterval(() => {
			currentCall = buttons[buttonIndex];
			callList = callList.concat([buttonIndex]);
			console.log('callList in callDelay: ' + callList);
			callIndex += 1;

			if (callIndex >= round) {
				clearInterval(callDelay);
			}

		}, 1000);

	}	

	// Response Loop
	function responseLoop() {
		buttonsActive = true;
	}

	// Click Handler
	function buttonPunch(buttonIndex) {
		console.log('button '+ buttons[buttonIndex] + ' punched');
		console.log('callList: ' + callList[0], 'buttonIndex: ' + buttonIndex);
		console.log('buttonsActive: ' + buttonsActive);

		if (buttonsActive === true) {
			if (buttonIndex === callList[0]) {
				callList.shift();
				console.log('Button is correct');
				console.log('callList: ' + callList);
			} else {
				gameOver();
			}
		}

		if (callList.length === 0) {
			round += 1
			startRound();
		}
	}

	// Game Over
	function gameOver() {
		gameOn = false;
		message = 'Game Over';
	}
</script>

<style>
	
</style>

<h1>Number Punch</h1>
{#if currentCall.length > 0}
	<div class="call" out:fade="{{delay: 250, duration: 500}}">{currentCall}</div>
{/if}

{#each buttons as button, buttonIndex}
	<button on:click={() => buttonPunch(buttonIndex)}>{button}</button>
{/each}

{#if !gameOn}
	<button on:click={() => startGame()}>Start Game</button>
{/if}

{#if message.length > 0}
	<div>{message}</div>
{/if}