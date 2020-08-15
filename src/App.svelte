<script>
	export let name;
	import Tailwindcss from './Tailwindcss.svelte';
	
	let winner = "??";
	let win = false;
	let searchingWinner = false;

	let participants = ['Azis','Athar','Ogi'];
	let participantsCanWin = participants;

	let addParticipantName = "";

	function deleteParticipant(deleteName) {
		participants = participants.filter(name => name !== deleteName);
	}

	function addParticipant() {
		participants = [...participants, addParticipantName];
		addParticipantName = "";
	}

	function acak() {
		win = false;
		searchingWinner = true;
		let i = 0;
		let acakWaiting = setInterval(() => {
			winner = participants[i];
			if(i == participants.length-1) {
				i = 0;
			}else{
				i++;
			}
		}, 1);

		setTimeout(() => {
			winner = participantsCanWin[Math.floor(Math.random() * participantsCanWin.length)];;
			participantsCanWin = participantsCanWin.filter(name => name !== winner);

			if(winner == undefined) {
				participantsCanWin = participants;
				winner = participantsCanWin[Math.floor(Math.random() * participantsCanWin.length)];;
			}
			win = true;
			searchingWinner = false;
			clearInterval(acakWaiting)
		}, 3000);
	}
</script>

<main>
	<Tailwindcss />
	<div class="text-center">
		<h1 class='title'>UNDIAN DOORPRIZE</h1>
		<p> Pemenang:</p>
		<h1 class='pemenang' class:text-green-500={win==true}>{winner}</h1>
		{#if win==true}
			<p class="text-center text-green-500">Menang!</p>
		{/if}
		{#if !searchingWinner}
		<button  class="mt-5 px-10 py-4 text-lg bg-blue-600 shadow-lg border-0 rounded-md text-white hover:bg-blue-700 hover:shadow-xl transition duration-150" on:click={acak}>Acak Sekarang</button>
		{/if}
	</div>

	<div class="container px-5 mx-auto mt-32">
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
						<td>
							<button class="px-5 py-2 bg-red-600 shadow-md border-0 rounded-md text-white hover:bg-red-700 hover:shadow-lg transition duration-150" on:click={() => deleteParticipant(participant)}>Hapus</button>
						</td>
					</tr>
					{/each}
					<tr>
						<td>{participants.length+1}</td>
						<td>
							<form on:submit|preventDefault="{addParticipant}">
								<input type="text" placeholder="Masukkan nama baru" class='px-2 py-1 rounded-md border-gray-400 shadow-sm' bind:value={addParticipantName}>
								<button type="submit" class="px-2 py-1 bg-teal-600 shadow-md border-0 rounded-md text-white hover:bg-teal-700 hover:shadow-lg transition duration-150">+</button>
							</form>
						</td>
						<td>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</main>

...
<style>
	.title {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	.pemenang {
		font-size: 10rem;
	}
	.table tbody tr {
		@apply  border-b-2 border-gray-200
	}
	.table tbody tr:hover {
		@apply  bg-gray-100;
	}
	.table tr th {
		@apply bg-gray-400;
	}
	.table tr th, .table tr td {
		@apply px-3 py-3 text-lg text-left px-16;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>