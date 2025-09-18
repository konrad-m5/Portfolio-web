<script>

	import "../app.css";
	import Header from "../components/Header.svelte";
	import Footer from "../components/Footer.svelte";
	import Background from '../components/Background.svelte';

	let y = $state(0);
	let innerHeight = $state(0);
	let innerWidth = $state(0);

	function scrollToTop() {
		window.scrollTo({ top: 0, behavior: 'smooth' });
	}

  let { children } = $props();
</script>

<!--<Background class="absolute top-0 left-0 w-full h-full -z-10 opacity-20"/>-->

<div class="relative flex flex-col mx-auto w-full text-sm
  sm:text-base min-h-screen">
	<div class={"fixed bottom-0 w-full duration-200 flex p-10 z-[10] "+ (
		y > 0 ? 'opacity-100 transition-opacity duration-200 ' : 'opacity-0 pointer-events-none transition-opacity duration-200 '
	)}>
		<button onclick={scrollToTop} class="ml-auto rounded-full bg-slate-900 text-white-400 px-3 sm:px-4
		hover:bg-slate-800 cursor-pointer" aria-label="Scroll to top">
			<i class="fa-solid fa-arrow-up grid place-item-center aspect-square"></i>
		</button>
	</div>
	<Header {y}/>
	{@render children()}
	<Footer/>	

</div>

<svelte:window bind:scrollY={y}  bind:innerHeight={innerHeight} bind:innerWidth={innerWidth}/>
