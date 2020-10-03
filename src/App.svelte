<script>
import { onMount } from "svelte";
import NewNote from "./components/NewNote.svelte"
import Sidebar from "./components/Sidebar.svelte"

	let newNoteComponent;
	let sidebarComponent;

	let category;			//categoria attualmente cliccata	
	let searchValue="";		//valore attuale nella barra di ricerca

	var notes = [];			//db delle note

	onMount(() => {
		sidebarComponent.noteClicked();
		sidebarComponent.closeSideBar();
	});

	//check sulle note per eventuali rimpiazzi
	function noteCheck(note){
		if(note.title == undefined && note.text == undefined ||  note.title == "" && note.text == ""){
			note.title = "";
			note.text = "Nota vuota";
		} else if(note.title == undefined || note.title == ""){
			note.title = "Nota senza titolo"
		} else if(note.text == undefined || note.text ==""){
			note.text="";
		}
	}

	//aggiunta di una nuova nota passata dal component 'NewNote'
	const addNote = (e) => {
		const note = e.detail;
		noteCheck(note);
		notes = [...notes,note]
		sortArray();
	}

	//cambio della categoria passata dal component 'Sidebar'
	const categoryChange = (e) =>{
		category = e.detail;
	}

	function colorChange(color,note){
		note.color = color;
		notes = [...notes];
	}

	function groupChange(group,note){
		if(group == category){
			note.group = "note";
		} else {note.group = group;}
		notes = [...notes];
	}

	function pinClicked(note){
		note.pin = !note.pin;
		notes = [...notes];
		sortArray();
	}

	//funzione di ordinamento delle note in base all'attributo 'pin'
	function sortArray(){
		notes.sort((a,b) => b.pin - a.pin);
	}

	function deleteNote(note){	
		let temparr = [];
		notes.forEach(element =>{			//ogni elemento diverso dalla nota da eliminare
			if(element != note)				//viene aggiunto ad un array temporaneo
				temparr.push(element);
			});
		notes = new Array();
		notes = temparr;					//rimpiazzo le vecchie note con quelle nuove senza quella eliminata
	}

	function deleteAll(){
		notes = notes.filter(function(e){
			return e.group != "trash"
		});
	}

</script>

<div class = "navbar">
	<div class = "ham-menu-logo">
		<div class="ham-menu" on:click={sidebarComponent.sideBarControl()}>
			<svg class="svg-ham">
				<path d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"></path>
			</svg>
		</div>
		<div class="keep-logo">
			<img class="keep-image" src="https://www.gstatic.com/images/branding/product/1x/keep_48dp.png" alt="logo">
			{#if category == "note"}
			<span class="keep-span">Keep</span>
			{:else if category=="prom"}
			<span class="keep-span">Promemoria</span>
			{:else}
			<span class="keep-span">Cestino</span>
			{/if}
		</div>
	</div>
	<div class = "search-bar">
			<button class="search-button">
				<svg class="search-svg">
					<path class="svg-icon" d="M20.49,19l-5.73-5.73C15.53,12.2,16,10.91,16,9.5C16,5.91,13.09,3,9.5,3S3,5.91,3,9.5C3,13.09,5.91,16,9.5,16 c1.41,0,2.7-0.47,3.77-1.24L19,20.49L20.49,19z M5,9.5C5,7.01,7.01,5,9.5,5S14,7.01,14,9.5S11.99,14,9.5,14S5,11.99,5,9.5z"></path>
				</svg>
			</button>
			<input id="search-bar-input" class="search-bar-input" type="text" placeholder="Cerca" bind:value={searchValue} style="font-family: Open Sans">
	</div>

</div>

<div class="main-container">
	<Sidebar on:categoryChange = {categoryChange} bind:this={sidebarComponent}/>
	<div id= "notes-container" class="notes-container">
		{#if category == "trash"}
		<div class="trash">
			<p class="trash-text" style="font-family: Open Sans">Le note nel Cestino vengono eliminate dopo 7 giorni</p>
			<div class="empty-trash" on:click={deleteAll} style="font-family: Open Sans">Svuota cestino</div>
		</div>
		{:else}
		<NewNote on:addNote={addNote} bind:this={newNoteComponent}/>
		{/if}
		<div class="note-box">
			{#each notes as note}
				{#if note.group == category}
					{#if note.title.toLowerCase().includes(searchValue.toLowerCase()) || note.text.toLowerCase().includes(searchValue.toLowerCase()) || searchValue == ""}
					<div class="note" style="background-color:{note.color}">
							<div class="note-text-area">
								<div class="note-title" id="note-title" style="font-family: Open Sans">
										{note.title}
								</div>
								<div class="note-text" id="note-text" style="font-family: Open Sans">
									{note.text}
								</div>
							</div>
							<div class="note-buttons">
								{#if category == "trash"}
								<div id="button-item2" tabindex="-1" on:click={() =>deleteNote(note)} class="more" style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNHB4IiBoZWlnaHQ9IjI0cHgiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0iIzAwMDAwMCI+CiAgICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDBWMHoiLz4KICAgIDxwYXRoIGQ9Ik02IDE5YzAgMS4xLjkgMiAyIDJoOGMxLjEgMCAyLS45IDItMlY3SDZ2MTJ6bTIuNDYtNy4xMmwxLjQxLTEuNDFMMTIgMTIuNTlsMi4xMi0yLjEyIDEuNDEgMS40MUwxMy40MSAxNGwyLjEyIDIuMTItMS40MSAxLjQxTDEyIDE1LjQxbC0yLjEyIDIuMTItMS40MS0xLjQxTDEwLjU5IDE0bC0yLjEzLTIuMTJ6TTE1LjUgNGwtMS0xaC01bC0xIDFINXYyaDE0VjR6Ii8+CiAgICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDB6Ii8+Cjwvc3ZnPgo=);"></div>
								<div id="button-item2" tabindex="-1" on:click={() =>groupChange("note",note)} class="more" style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNHB4IiBoZWlnaHQ9IjI0cHgiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0iIzAwMDAwMCI+CiAgICA8cGF0aCBkPSJNMTkgNGgtMy41bC0xLTFoLTVsLTEgMUg1djJoMTR6TTYgN3YxMmMwIDEuMS45IDIgMiAyaDhjMS4xIDAgMi0uOSAyLTJWN0g2em04IDd2NGgtNHYtNEg4bDQtNCA0IDRoLTJ6Ii8+CiAgICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDB6Ii8+Cjwvc3ZnPgo=);"></div>
								{:else}
									{#if note.group == "prom"}
										<div id="button-item2" class="prom" tabindex="-1" on:click={() => groupChange("prom",note)} style="opacity:1"></div>
									{:else}
										<div id="button-item2" class="prom" tabindex="-1" on:click={() => groupChange("prom",note)}></div>
									{/if}
									<div id="button-item2" class="colors" tabindex="-1"></div>
									<div id='color-select' class="color-select">
										<div class="color" on:click={() => colorChange("white",note)} style="background-color: white; border-color:grey"></div>
										<div class="color" on:click={() => colorChange("#f28b82",note)} style="background-color: #f28b82; border-color:#f28b82"></div>
										<div class="color" on:click={() => colorChange("#fbbc04",note)} style="background-color: #fbbc04; border-color:#fbbc04"></div>
										<div class="color" on:click={() => colorChange("#fff475",note)} style="background-color: #fff475; border-color:#fff475"></div>
										<div class="color" on:click={() => colorChange("#ccff90",note)} style="background-color: #ccff90; border-color:#ccff90"></div>
										<div class="color" on:click={() => colorChange("#a7ffeb",note)} style="background-color: #a7ffeb; border-color:#a7ffeb"></div>
										<div class="color" on:click={() => colorChange("#cbf0f8",note)} style="background-color: #cbf0f8; border-color:#cbf0f8"></div>
										<div class="color" on:click={() => colorChange("#aecbfa",note)} style="background-color: #aecbfa; border-color:#aecbfa"></div>
										<div class="color" on:click={() => colorChange("#d7aefb",note)} style="background-color: #d7aefb; border-color:#d7aefb"></div>
										<div class="color" on:click={() => colorChange("#fdcfe8",note)} style="background-color: #fdcfe8; border-color:#fdcfe8"></div>
										<div class="color" on:click={() => colorChange("#e6c9a8",note)} style="background-color: #e6c9a8; border-color:#e6c9a8"></div>
										<div class="color" on:click={() => colorChange("grey",note)} style="background-color: grey; border-color:grey"></div>
									</div>
									{#if !note.pin}
										<div id="button-item2" tabindex="-1" class="pin2" on:click={() => pinClicked(note)} style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDB6Ii8+CiAgPHBhdGggZmlsbD0iIzAwMCIgZD0iTTE3IDR2N2wyIDN2MmgtNnY1bC0xIDEtMS0xdi01SDV2LTJsMi0zVjRjMC0xLjEuOS0yIDItMmg2YzEuMTEgMCAyIC44OSAyIDJ6TTkgNHY3Ljc1TDcuNSAxNGg5TDE1IDExLjc1VjRIOXoiLz4KPC9zdmc+Cg==);"></div>
									{:else}
										<div id="button-item2" tabindex="-1" class="pin2" on:click={() => pinClicked(note)} style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDB6Ii8+CiAgPHBhdGggZmlsbD0iIzAwMCIgZD0iTTE3IDRhMiAyIDAgMCAwLTItMkg5Yy0xLjEgMC0yIC45LTIgMnY3bC0yIDN2Mmg2djVsMSAxIDEtMXYtNWg2di0ybC0yLTNWNHoiLz4KPC9zdmc+Cg==);"></div>
									{/if}
									<div id="button-item2" tabindex="-1" on:click={() => groupChange("trash", note)} class="more" style="background-image: url(https://img.icons8.com/material-outlined/24/000000/trash.png);"></div>
								{/if}
							</div>
						</div>		
					{/if}
				{/if}
			{/each}
		</div>
	</div>
</div>
