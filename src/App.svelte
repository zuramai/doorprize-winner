<script>
	export let name;
	import './app.css';
	
	let winner = "??";
	let win = false;
	let searchingWinner = false;

	let participants = ['Ilham', 'Beluga'];
	let participantsCanWin = participants;

	let addParticipantName = "";

	// Delete a participant from array
	function deleteParticipant(deleteName) {
		participants = participants.filter(name => name !== deleteName);
		participantsCanWin = participantsCanWin.filter(name => name !== deleteName);
	}

	// Add participants to array
	function addParticipant() {
		participants = [...participants, addParticipantName];
		participantsCanWin = [...participantsCanWin, addParticipantName];
		
		addParticipantName = "";
	}

	// Change font size depends on winner size
	function changeFontSize() {
		let letterCount = winner.length;
		let winnerDiv = document.querySelector('.pemenang');

		if(letterCount >= 6) {
			winnerDiv.style.fontSize = "6rem";
			winnerDiv.style.lineHeight = "8rem";
		}
	}

	// Validate input length to make sure the text doesn't exit the window
	function validateLength(e,maxLength) {
		let val = e.originalTarget.value;
		if(val.length > maxLength){
			//Listening to keyup event is too late for calling preventDefault
			e.preventDefault();
		}
	}

	// Start random choose winner
	function acak() {
		win = false;
		searchingWinner = true;
		let i = 0;
		let acakWaiting = setInterval(() => {
			changeFontSize()
			winner = participants[i];
			if(i == participants.length-1) {
				i = 0;
			}else{
				i++;
			}
		}, 50);

		setTimeout(() => {
			winner = participantsCanWin[Math.floor(Math.random() * participantsCanWin.length)];
			participantsCanWin = participantsCanWin.filter(name => name !== winner);

			if(winner == undefined) {
				participantsCanWin = participants;
				winner = participantsCanWin[Math.floor(Math.random() * participantsCanWin.length)];
			}
			win = true;
			searchingWinner = false;
			clearInterval(acakWaiting);
		}, 3000);
	}
</script>

<main>
	<div class="text-center">
		<h1 class='title font-bold text-red-600 mt-5'>UNDIAN DOORPRIZE</h1>
		<p> Pemenang:</p>
		<h1 class='pemenang  my-8' class:text-green-500={win==true}>{winner}</h1>
		{#if win==true}
			<p class="text-center text-green-500 ">Menang!</p>
		{/if}
		{#if !searchingWinner}
		<button  class="mt-5 px-10 py-4 text-lg bg-blue-600 shadow-lg border-0 rounded-md text-white hover:bg-blue-700 hover:shadow-xl transition duration-150" on:click={acak}>Acak Sekarang</button>
		{/if}
	</div>

	<div class="container px-5 mx-auto mt-16">
		<h3 class='text-2xl '>Daftar Peserta</h3>
		<div class="overflow-x-auto">
			<table class='table w-full mt-5'>
				<thead>
					<tr>
						<th>#</th>
						<th>Nama</th>
						<th></th>
					</tr>
				</thead>
				<tbody>
					{#each participants as participant, i}
					<tr>
						<td>{i+1}</td>
						<td>{participant}</td>
						<td class="flex justify-end">
							<button class="px-2 py-2 bg-red-600 shadow-md border-0 rounded-md text-white text-right hover:bg-red-700 hover:shadow-lg transition duration-150" on:click={() => deleteParticipant(participant)}>
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash3" viewBox="0 0 16 16">
									<path d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5ZM11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H2.506a.58.58 0 0 0-.01 0H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1h-.995a.59.59 0 0 0-.01 0H11Zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5h9.916Zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47ZM8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5Z"/>
								</svg>
							</button>
						</td>
					</tr>
					{/each}
					<tr>
						<td>{participants.length+1}</td>
						<td colspan="2">
							<form on:submit|preventDefault="{addParticipant}" class="flex w-full gap-2">
								<input type="text" on:keydown={e => validateLength(e,20)} required placeholder="Masukkan nama baru" class='px-2 py-1 w-full max-w-[500px] rounded-md border-gray-400 shadow-sm' bind:value={addParticipantName}>
								<button type="submit" class="px-4 py-1 bg-teal-600 shadow-md border-0 rounded-md text-white hover:bg-teal-700 hover:shadow-lg transition duration-150">+</button>
							</form>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>

	<p class='text-center my-2 mt-32'>Created by <a href="http://ahmadsaugi.com">Ahmad Saugi</a> &copy;</p>
</main>