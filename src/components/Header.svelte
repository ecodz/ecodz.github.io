<script>
    import { MenuIcon, XIcon } from "svelte-feather-icons"

    import { readable } from 'svelte/store';
//export a function that return a readable given a string media query as input
export const useMediaQuery = (mediaQueryString)=>{
  //we inizialize the readable as null and get the callback with the set function
	const matches = readable(null, (set) => {
    		//we match the media query
		const m=window.matchMedia(mediaQueryString);
		//we set the value of the reader to the matches property
		set(m.matches);
		//we create the event listener that will set the new value on change
		const el=e => set(e.matches);
		//we add the new event listener
		m.addEventListener("change", el);
    		//we return the stop function that will clean the event listener
		return () => {m.removeEventListener("change", el)};
	});
	//then we return the readable
	return matches;
}
    let menuMediaQuery = useMediaQuery("(max-width: 600px)")
    let menuOpen = false;

    function toggleMenu() {
        menuOpen = !menuOpen;
    }
</script>

<header>
    <a href="/" class="nonlink">
        <div>
            <img src="/img/me.svg" alt="Website Icon">
            <p>E-Codz' Website</p>
        </div>
    </a>
    <nav>
        {#if $menuMediaQuery}
            <button id="menu" aria-label="Menu" on:click={toggleMenu}>
                {#if menuOpen}
                     <XIcon />
                {:else}
                     <MenuIcon />
                {/if}
                
            </button>
        {/if}
        {#if !$menuMediaQuery || menuOpen}
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/projects">Projects</a></li>
            <li><a href="/links">Links</a></li>
        </ul>
        {/if}
    </nav>
</header>

<style lang="scss">
    header {
        max-width: 800px;
        margin: auto;
        padding: 27px;
    }

    header, ul, div {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        gap: 8px;
    }

    ul {
        list-style-type: none;
        gap: 16px;
    }

    img {
        max-height: 2em;
        border-radius: 50%;
    }

    #menu {
        background: none;
        border: none;
        color: var(--color);
    }

    @media(max-width: 600px) {
        nav > ul {
            position: absolute;
            width: calc(100% - 54px);
            flex-direction: column;
            left: 50%;
            bottom: 0;
            background-color: hsl(237, 16%, 23%);
            padding: 0.5em 0 0.5em 0;
            translate: -50% -100%;
            margin: 0;
            border-radius: 8px;
        }
        header {
            position: fixed;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
        }
    }
</style>
