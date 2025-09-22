<script>
    import Background from "./Background.svelte";
    import Step from "./Step.svelte";
    import { browser } from '$app/environment';
    import { onMount, tick } from "svelte";
    let Carousel = null; // client-only component

    let steps = [
        {name: 'Snake', icon:'fa-solid fa-gamepad', href:'https://github.com/konrad-m5/L031K6_Snake'},
        {name: 'Naive Bayes Replica', icon:'fa-solid fa-brain', href:'https://github.com/konrad-m5/Naives-bayes-replica'},
        {name: 'Weather App', icon:'fa-solid fa-cloud-sun', href:'https://github.com/konrad-m5/weatherapp'}
    ];

    let benefits = [
        {name: 'Passion for Technology', description: 'I am deeply passionate about technology and constantly seek to learn and explore new advancements in the field.'},
        {name: 'Problem-Solving Skills', description: 'I enjoy tackling complex problems and finding innovative solutions through critical thinking and creativity.'},
        {name: 'Continuous Learning', description: 'I am committed to continuous learning and self-improvement, staying updated with the latest industry trends and best practices.'}
    ];
            
// tripled array + carousel index + loop helpers
    const tripled = [...steps, ...steps, ...steps];
    let current = steps.length; // start in middle set
    let transitioning = false;
    let noTransition = false;

    // load Carousel only on client (avoid ResizeObserver SSR error)
    onMount(async () => {
        if (!browser) return;
        const mod = await import("svelte-carousel/src/components/Carousel/Carousel.svelte");
        Carousel = mod.default;
        await tick();
    });

    // handle the infinite-loop jump after a slide transition
    async function handleTransitionEnd() {
        transitioning = false;
        if (current < steps.length) {
            noTransition = true;
            current = steps.length * 2 - 1;
            await tick();
            noTransition = false;
            return;
        }
        if (current >= steps.length * 2) {
            noTransition = true;
            current = steps.length;
            await tick();
            noTransition = false;
            return;
        }
    }

    function handleNextClick() {
      // call the carousel instance method if available
      Carousel?.goToNext?.();
    }

</script>



<main class= "flex flex-col flex-1 p-4 mt-32">
    
    <section id="introPage" 
        class="relative overflow-hidden pt-32 pb-8 sm:pt-40 sm:pb-14 mb-[24vh]"
        style="background: transparent;">

        

        <div class ="flex flex-col lg:justify-center text-center gap-6 md:gap-8">
            
                <h2 class="font-semibold text-4xl sm:text-5xl md:text-6xl">
                    Hi, I'm <span class="font-bold text-red-800 zilla-slab">Konrad</span> Malara<br/>A Computer Science Student
                </h2>

                <p class="text-base sm:text-lg md:text-xl">
                    My favourite technologies are Python, C and Java.
                </p>

                <a class="blueShadow mx-auto lg:mr-auto text-base sm:text-lg md:text-xl zilla-slab
                relative overflow-hidden px-6 py-3 group rounded-full bg-white text-slate-950 cursor-pointer"
                href="#contact">

                    <div class = "absolute top-0 right-full w-full h-full bg-red-400 opacity-20
                                group-hover:translate-x-full z-0 duration-200">
            
                    </div>
                    <h4 class="relative z-9">Get in Touch &rarr;</h4>
                </a>

        </div><!--end div-->

      

    </section><!--end intro section-->




    <!-- Projects Section -->
<section id="projects" class="py-20 lg:py-32 flex flex-col gap-24">
    <div class="flex flex-col gap-2 text-center">
        <h6 class="text-lg sm:text-xl md:text-2xl">
            A few of my projects
        </h6>
        <h3 class="font-semibold text-3xl sm:text-4xl md:text-5xl">
            Curious to see my work?
        </h3>
    </div>

<!-- Carousel of projects -->
{#if Carousel}
  <svelte:component
    this={Carousel}
    bind:this={carouselRef}
    {current}
    on:transitionEnd={handleTransitionEnd}
    transitionDuration={noTransition ? 0 : 500}
    itemsToShow={1}
    itemsToScroll={1}
    autoplay
    autoplayDuration={5000}
    loop={true}
    showArrows={true}
    showIndicators={false}
    pagescount={3}
    class="max-w-4xl mx-auto my-carousel"
  >
    {#each steps as step, i (i)}
      <div class="p-4 sm:p-6 duration-200 mx-2 flex-shrink-0 mx-auto" style="transform: {i === current ? 'scale(1)' : 'scale(0.9)'};">
        <Step {step}>
          {#if i === 0}
            <p class="text-lg sm:text-xl md:text-2xl">A snake game made in a NUCLEO board.</p>
          {:else if i === 1}
            <p class="text-lg sm:text-xl md:text-2xl">A Naive Bayes replica built with Java.</p>
          {:else}
            <p class="text-lg sm:text-xl md:text-2xl">A weather app that provides real-time weather updates built in Java.</p>
          {/if}
        </Step>
      </div>
    {/each}
  </svelte:component>

{:else}
  <div class="w-full h-[260px] flex items-center justify-center text-gray-400">
    Loading projects...
  </div>
{/if}


</section>

    <section id="aboutme" class="py-20 pt-10 lg:pt-16 lg:py-32 flex flex-col
    gap-16 sm:gap-20 md:gap-24 relative">
        <div class="flex flex-col gap-2 text-center relative before:absolute before:top-0 before:left-0 before:w-2/3
        before:h-1.5 before:bg-red-700 after:absolute after:bottom-0 after:right-0 after:w-2/3
        after:h-1.5 after:bg-red-700 py-4">

            <h6 class="text-lg sm:text-xl md:text-2xl">Want to know more?</h6>
            <h3 class="font-semibold text-3xl sm:text-4xl md:text-5xl">a bit about me</h3>
        
        </div>
        <p class="mx-auto zilla-slab font-semibold text-lg sm:text-xl md:text-2xl">I'm a software developer with a passion for creating innovative solutions.</p>
        <div class="flex flex-col gap-20 w-full mx-auto max-w-[880px]">
            {#each benefits as benefit, index}
            <div class="flex gap-6 sm:gap-8">

                <p class="zilla-slab text-4xl sm:text-5xl md:text-6xl text-grey-500
                font-semibold">
                    0{index + 1}
                </p>

                <div class="flex flex-col gap-6 sm:gap-8">

                    <h3 class="text-2xl sm:text-3xl md:text-5xl">
                        {benefit.name}
                    </h3>
                    <p>{benefit.description}</p>

                </div>

            </div>
            {/each}
        </div>

        <!-- ah idk man maybe
        <h5>The complete package</h5>

        <div class="flex flex-col overflow-x-scroll gap-10 max-w-[880px] mx-auto w-full">

            <table class="bg-white text-slate-700 rounded text-center">
                <thead class="border-b border-solid border-slate-200">
                    <tr class="">
                        <th/>
                        <th class="whitespace-nowrap p-2 px-4">Benefit</th>
                        <th class="whitespace-nowrap p-2 px-4">Description</th>
                        <th class="whitespace-nowrap bg-violet-700 text-white p-4 px-8">
                            Me
                        </th>
                    </tr>
                </thead>
            </table>

        </div>
        -->
    </section>

</main>