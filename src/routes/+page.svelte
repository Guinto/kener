<script>
    import Monitor from "$lib/components/monitor.svelte";
    import * as Card from "$lib/components/ui/card";
	import { Button, buttonVariants } from "$lib/components/ui/button";
	
    import Incident from "$lib/components/incident.svelte";
    import { Separator } from "$lib/components/ui/separator";
    import { Badge } from "$lib/components/ui/badge";
    export let data;
    let hasActiveIncidents = false;
    for (let i = 0; i < data.monitors.length; i++) {
        if (data.monitors[i].activeIncidents.length > 0) {
            hasActiveIncidents = true;
            break;
        }
    }
</script>
<div class="mt-32"></div>
{#if data.site.hero}
<section class="mx-auto flex w-full max-w-4xl mb-8 flex-1 flex-col items-start justify-center">
    <div class="mx-auto max-w-screen-xl px-4 lg:flex lg:items-center">
        <div class="mx-auto max-w-3xl text-center blurry-bg">
            {#if data.site.hero.image}
            <img src="{data.site.hero.image}" class="h-16 w-16 m-auto" alt="" srcset="" />
            {/if} {#if data.site.hero.title}
            <h1 class="bg-gradient-to-r from-green-300 via-blue-500 to-purple-600 bg-clip-text text-5xl font-extrabold text-transparent leading-snug">{data.site.hero.title}</h1>
            {/if} {#if data.site.hero.subtitle}
            <p class="mx-auto mt-4 max-w-xl sm:text-xl">{data.site.hero.subtitle}</p>
            {/if}
        </div>
    </div>
</section>
{/if} {#if hasActiveIncidents}
<section class="mx-auto bg-transparent mb-4 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center" id="">
    <div class="grid w-full grid-cols-2 gap-4">
        <div class="col-span-2 md:col-span-1 text-center md:text-left">
            <Badge variant="outline">Ongoing Incidents </Badge>
        </div>
    </div>
</section>
<section class="mx-auto backdrop-blur-[2px] mb-8 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center" id="">
    {#each data.monitors as monitor} {#each monitor.activeIncidents as incident, i}
    <Incident {incident} state="close" variant="title+body+comments+monitor" monitor="{monitor}" />
    {/each} {/each}
</section>
{/if} {#if data.monitors.length > 0}
<section class="mx-auto bg-transparent mb-4 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center" id="">
    <div class="grid w-full grid-cols-2 gap-4">
        <div class="col-span-2 md:col-span-1 text-center md:text-left">
            <Badge class="" variant="outline"> Availability per Component </Badge>
        </div>
        <div class="col-span-2 md:col-span-1 text-center md:text-right">
            <Badge variant="outline">
                <span class="w-[8px] h-[8px] inline-flex rounded-full bg-api-up opacity-75 mr-1"></span>
                <span class="mr-3">UP</span>

                <span class="w-[8px] h-[8px] inline-flex rounded-full bg-api-degraded opacity-75 mr-1"></span>
                <span class="mr-3">DEGRADED</span>

                <span class="w-[8px] h-[8px] inline-flex rounded-full bg-api-down opacity-75 mr-1"></span>
                <span class="mr-3">DOWN</span>
            </Badge>
        </div>
    </div>
</section>
<section class="mx-auto backdrop-blur-[2px] mb-8 flex w-full max-w-[890px] flex-1 flex-col items-start justify-center">
    <Card.Root>
        <Card.Content class="p-0 monitors-card">
            {#each data.monitors as monitor}
            <Monitor {monitor} localTz="{data.localTz}" />
            {/each}
        </Card.Content>
    </Card.Root>
</section>
{/if} {#if data.site.categories}
<section class="mx-auto backdrop-blur-[2px] mb-8 w-full max-w-[890px]">
	<h2 class="text-xl mb-2 mt-2 font-semibold">
		Other Monitors
	</h2>
    {#each data.site.categories as category}

    <Card.Root class="w-full mb-2">
        <Card.Header>
            <Card.Title>{category.name}</Card.Title>
            <Card.Description class="relative pr-[100px]">
				{#if category.description}
				{category.description}
				{/if}
				<a href="/category-{category.name}" class="{buttonVariants({ variant: 'secondary' })} absolute right-2 -top-4">
					View
				</a>
			</Card.Description>
        </Card.Header>
    </Card.Root>

    {/each}
</section>
{/if}
