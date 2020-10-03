<script module="export">
    import {createEventDispatcher} from 'svelte';
    let dispatch = createEventDispatcher();
    
    //variabili bindate
    let title;              
    let text;

    let group = "note";
    let pin = false;

    let placeholder1 = "Scrivi una nota...";
    let placeholder2 = "Titolo"
    
    const handleKeyup = () => {
		if (event.code == 'Enter') {
			createNote();
		}
    }

    function createNote(){
        let color = document.getElementById("note-input").style.backgroundColor;
            const newNote = {
                title,
                text,
                color,
                group,
                pin
            }
			dispatch('addNote', newNote);
			clearFields();
    }
    
    function showNoteEditor(){
		document.getElementById("note-input").style.height= "140px";
		document.getElementById("pin").style.display = "flex";
		placeholder1 = "Titolo";
		placeholder2 = "Scrivi una nota..."
	}

	export function closeNoteEditor(){
		document.getElementById("note-input").style.height = "46px";
		document.getElementById("pin").style.display = "none";
        document.getElementById("note-input").style.backgroundColor="white";
        
        clearFields();
        
		placeholder1 = "Scrivi una nota...";
        placeholder2 = "Titolo";
        pin = false;
    }
    
    function newNoteColorChange(color){
		document.getElementById("note-input").style.backgroundColor = color;
    }
    
    function clearFields(){
        document.getElementById("input-form").reset();
        title=undefined;
        text=undefined;
        group="note";
    }
    
    function pinClicked(){
        pin = !pin;
    }

    function promClicked(){
        if(group == "prom"){
            group = "note";
        } else {
            group = "prom";
        }
    }
</script>

<div class="new-note-box">
    <div class="note-input" id="note-input">
        <form id="input-form">
            <div class="first-line">
                <input id="title" class="note-input-text" placeholder={placeholder1} style="font-family: Open Sans" on:click={showNoteEditor} on:keyup|preventDefault={handleKeyup} bind:value={title}>
                {#if !pin}
                    <div id="pin" class="pin" on:click={pinClicked}></div>
                {:else}
                    <div id="pin" class="pin" on:click={pinClicked} style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij4KICA8cGF0aCBmaWxsPSJub25lIiBkPSJNMCAwaDI0djI0SDB6Ii8+CiAgPHBhdGggZmlsbD0iIzAwMCIgZD0iTTE3IDRhMiAyIDAgMCAwLTItMkg5Yy0xLjEgMC0yIC45LTIgMnY3bC0yIDN2Mmg2djVsMSAxIDEtMXYtNWg2di0ybC0yLTNWNHoiLz4KPC9zdmc+Cg==);"></div>
                {/if}
            </div>
            <input id="text" class="note-input-text" placeholder={placeholder2} bind:value={text} style="font-family: Open Sans; font-size: .845rem; font-weight:100" on:keyup|preventDefault={handleKeyup}>
        </form>
        <div class="buttons">
            {#if group == "prom"}
                <div class="button-item" tabindex="-1" on:click={promClicked} style="opacity:1; background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSIjMDAwIj4KICA8cGF0aCBkPSJNMTMgOWgtMnYySDl2MmgydjJoMnYtMmgydi0yaC0yeiIvPgogIDxwYXRoIGQ9Ik0xOCAxN3YtNmMwLTMuMDctMS42My01LjY0LTQuNS02LjMyVjRjMC0uODMtLjY3LTEuNS0xLjUtMS41cy0xLjUuNjctMS41IDEuNXYuNjhDNy42NCA1LjM2IDYgNy45MiA2IDExdjZINHYyaDE2di0yaC0yem0tMiAwSDh2LTZjMC0yLjQ4IDEuNTEtNC41IDQtNC41czQgMi4wMiA0IDQuNXY2em0tNCA1YzEuMSAwIDItLjkgMi0yaC00YzAgMS4xLjkgMiAyIDJ6Ii8+Cjwvc3ZnPgo=);"></div>
            {:else}
                <div class="button-item" tabindex="-1" on:click={promClicked} style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSIjMDAwIj4KICA8cGF0aCBkPSJNMTMgOWgtMnYySDl2MmgydjJoMnYtMmgydi0yaC0yeiIvPgogIDxwYXRoIGQ9Ik0xOCAxN3YtNmMwLTMuMDctMS42My01LjY0LTQuNS02LjMyVjRjMC0uODMtLjY3LTEuNS0xLjUtMS41cy0xLjUuNjctMS41IDEuNXYuNjhDNy42NCA1LjM2IDYgNy45MiA2IDExdjZINHYyaDE2di0yaC0yem0tMiAwSDh2LTZjMC0yLjQ4IDEuNTEtNC41IDQtNC41czQgMi4wMiA0IDQuNXY2em0tNCA1YzEuMSAwIDItLjkgMi0yaC00YzAgMS4xLjkgMiAyIDJ6Ii8+Cjwvc3ZnPgo=);"></div>
            {/if}
            <div class="button-item" tabindex="-1" style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSIjMDAwIj4KICA8cGF0aCBkPSJNMTIgMjJDNi40OSAyMiAyIDE3LjUxIDIgMTJTNi40OSAyIDEyIDJzMTAgNC4wNCAxMCA5YzAgMy4zMS0yLjY5IDYtNiA2aC0xLjc3Yy0uMjggMC0uNS4yMi0uNS41IDAgLjEyLjA1LjIzLjEzLjMzLjQxLjQ3LjY0IDEuMDYuNjQgMS42N0EyLjUgMi41IDAgMCAxIDEyIDIyem0wLTE4Yy00LjQxIDAtOCAzLjU5LTggOHMzLjU5IDggOCA4Yy4yOCAwIC41LS4yMi41LS41YS41NC41NCAwIDAgMC0uMTQtLjM1Yy0uNDEtLjQ2LS42My0xLjA1LS42My0xLjY1YTIuNSAyLjUgMCAwIDEgMi41LTIuNUgxNmMyLjIxIDAgNC0xLjc5IDQtNCAwLTMuODYtMy41OS03LTgtN3oiLz48Y2lyY2xlIGN4PSI2LjUiIGN5PSIxMS41IiByPSIxLjUiLz4KICA8Y2lyY2xlIGN4PSI5LjUiIGN5PSI3LjUiIHI9IjEuNSIvPjxjaXJjbGUgY3g9IjE0LjUiIGN5PSI3LjUiIHI9IjEuNSIvPjxjaXJjbGUgY3g9IjE3LjUiIGN5PSIxMS41IiByPSIxLjUiLz4KPC9zdmc+Cg==);"></div>
            <div id='color-select' class="color-select2">
                <div class="color" on:click={() => newNoteColorChange("white")} style="background-color: white; border-color:grey"></div>
                <div class="color" on:click={() => newNoteColorChange("#f28b82")} style="background-color: #f28b82; border-color:#f28b82"></div>
                <div class="color" on:click={() => newNoteColorChange("#fbbc04")} style="background-color: #fbbc04; border-color:#fbbc04"></div>
                <div class="color" on:click={() => newNoteColorChange("#fff475")} style="background-color: #fff475; border-color:#fff475"></div>
                <div class="color" on:click={() => newNoteColorChange("#ccff90")} style="background-color: #ccff90; border-color:#ccff90"></div>
                <div class="color" on:click={() => newNoteColorChange("#a7ffeb")} style="background-color: #a7ffeb; border-color:#a7ffeb"></div>
                <div class="color" on:click={() => newNoteColorChange("#cbf0f8")} style="background-color: #cbf0f8; border-color:#cbf0f8"></div>
                <div class="color" on:click={() => newNoteColorChange("#aecbfa")} style="background-color: #aecbfa; border-color:#aecbfa"></div>
                <div class="color" on:click={() => newNoteColorChange("#d7aefb")} style="background-color: #d7aefb; border-color:#d7aefb"></div>
                <div class="color" on:click={() => newNoteColorChange("#fdcfe8")} style="background-color: #fdcfe8; border-color:#fdcfe8"></div>
                <div class="color" on:click={() => newNoteColorChange("#e6c9a8")} style="background-color: #e6c9a8; border-color:#e6c9a8"></div>
                <div class="color" on:click={() => newNoteColorChange("grey")} style="background-color: grey; border-color:grey"></div>
            </div>
            <div class="button-item" style="background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSIjMDAwIj4KICA8cGF0aCBkPSJNMTkgM0g1Yy0xLjEgMC0yIC45LTIgMnYxNGMwIDEuMS45IDIgMiAyaDE0YzEuMSAwIDItLjkgMi0yVjVjMC0xLjEtLjktMi0yLTJ6bTAgMTZINVY1aDE0djE0em0tNS03bC0zIDMuNzJMOSAxM2wtMyA0aDEybC00LTV6Ii8+Cjwvc3ZnPgo=);"></div>
            <div class="close-button" style="font-family:Open Sans; font-weight:bold" on:click={closeNoteEditor}>Chiudi</div>
        </div>
    </div>
</div>