<svelte:options accessors/>
<script>
    import {createEventDispatcher} from 'svelte';
    let dispatch = createEventDispatcher();

	export const category = "note";
	
	let sidebar = false;
	let span = document.getElementsByClassName("sidebar-span");
    let item = document.getElementsByClassName("sidebar-item");
    
    function showSpan(){
		for(let i = 0; i < span.length; i++){
			span[i].style.display="flex";
		}
	}

	function hideSpan(){
		for(let i = 0; i < span.length; i++){
			span[i].style.display="none";
		};
	}

	function styleReset(){
		for(let i = 0; i < item.length; i++){
			item[i].style.backgroundColor="";
		}
    }
    
    function categoryChange(category){
        dispatch('categoryChange',category)
    }

	export function sideBarControl(){
		if(sidebar){
			closeSideBar();
		} else { openSideBar()};
	}

	function openSideBar(){
		document.getElementById("sidebar").style.width = "400px";
		document.getElementById("notes-container").style.marginLeft = "10px";
		showSpan();
		sidebar = true;
	}

	export function closeSideBar(){
		document.getElementById("sidebar").style.width = "100px";
		hideSpan();
		sidebar = false;
	}

	export function noteClicked(){
		styleReset();
		openSideBar();
		document.getElementById("notes-item").style.backgroundColor = "#feefc3";
		categoryChange("note");
	}

	export function promClicked(){
		styleReset();
		openSideBar();
		document.getElementById("promemoria-item").style.backgroundColor = "#feefc3";
		categoryChange("prom");
	}

	export function trashClicked(){
		styleReset();
		openSideBar();
		document.getElementById("cestino-item").style.backgroundColor = "#feefc3"
		categoryChange("trash");
	}
</script>

<div id="sidebar" class="sidebar"  on:mouseout={closeSideBar} on:mouseover={openSideBar}>
    <div id="notes-item" class="sidebar-item" on:click={noteClicked} style="cursor: pointer" >
        <div id="notes-icon" class="sidebar-icon">
            <svg class="svg-sidebar" >
                <path d="M9 21c0 .55.45 1 1 1h4c.55 0 1-.45 1-1v-1H9v1zm3-19C8.14 2 5 5.14 5 9c0 2.38 1.19 4.47 3 5.74V17c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-2.26c1.81-1.27 3-3.36 3-5.74 0-3.86-3.14-7-7-7zm2.85 11.1l-.85.6V16h-4v-2.3l-.85-.6A4.997 4.997 0 0 1 7 9c0-2.76 2.24-5 5-5s5 2.24 5 5c0 1.63-.8 3.16-2.15 4.1z"></path>
            </svg>
        </div>
        <span class="sidebar-span">Note</span>
    </div>

    <div id="promemoria-item" class="sidebar-item" on:click={promClicked} style="cursor: pointer">
        <div id="promemoria-icon" class="sidebar-icon" >
            <svg class="svg-sidebar" >
                <path d="M18 17v-6c0-3.07-1.63-5.64-4.5-6.32V4c0-.83-.67-1.5-1.5-1.5s-1.5.67-1.5 1.5v.68C7.64 5.36 6 7.92 6 11v6H4v2h16v-2h-2zm-2 0H8v-6c0-2.48 1.51-4.5 4-4.5s4 2.02 4 4.5v6zm-4 5c1.1 0 2-.9 2-2h-4c0 1.1.9 2 2 2z"></path>
            </svg>
        </div>
        <span class="sidebar-span">Promemoria</span>
    </div>

    <div id="cestino-item" class="sidebar-item" on:click={trashClicked} style="cursor: pointer">
        <div id="cestino-icon" class="sidebar-icon" >
            <svg class="svg-sidebar" >
                <path d="M15 4V3H9v1H4v2h1v13c0 1.1.9 2 2 2h10c1.1 0 2-.9 2-2V6h1V4h-5zm2 15H7V6h10v13zz"></path>
                <path d="M9 8h2v9H9zm4 0h2v9h-2z"></path>
            </svg>
        </div>
        <span class="sidebar-span">Cestino</span>
    </div>
</div>