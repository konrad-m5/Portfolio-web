<script>
    export let y = 0;

    let tabs = [
        {name: "Projects", link: "#projects"},
        {name: "About Me", link: "#aboutme"},
    ];
</script>

<header class="{'sticky z-[10] top-0 duration-200 px-6 flex items-center justify-between border border-solid '
+ (y > 0 ? 'py-4 bg-black border-red-950' : 'py-6 bg-transparent border-transparent')}">

    <h1 class="font-medium">

        <b class="font-bold zilla-slab">Konrad</b> Malara

    </h1>

    <div class="flex items-center gap-4 ">

        <div class=
        {"hidden sm:flex flex items-center gap-4" + 
        (y > 0 ? ' opacity-0 pointer-events-none transition-opacity duration-200' : ' opacity-100 transition-opacity duration-200')}>
        {#each tabs as tab, index}

            <a class="duration-200 hover:text-red-400" href={tab.link} key={index}>
                <p>{tab.name}</p>
            </a>
            
        {/each}    
    
        </div>

    <a href="https://github.com/BFrederickss"
    target="_blank"
    class="blueShadow relative overflow-hidden px-5 py-2 group rounded-full bg-white
    text-slate-950">
        <div class = "absolute top-0 right-full w-full h-full bg-red-900 opacity-20
        group-hover:translate-x-full z-0 duration-200">
    
        </div>

        <h4 class="relative z-9">Get in Touch</h4>

    </a>

    </div>
        
</header>