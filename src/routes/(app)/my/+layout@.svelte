<style>
.minimum-width-rem {
	min-width: 360px;
}
</style>

<script lang="ts">
import { Nav, PageTransitions, Footer } from '$lib/components'
import { page } from '$app/stores'
import { slotMarginGeneral, slotMarginGeneralWithHelloBar } from '$lib/config'
import menu from '$lib/config/menu'
import SidebarDashboard from './_SidebarDashboard.svelte'

export let data
$: ({ path, url, sort, isHome, q, currentPage, me, cart, store } = data)

let hellobar = $page.data.store?.hellobar || {}
let openSidebar = false
let showCartSidebar = false
</script>

<div class="minimum-width-rem h-screen overflow-hidden">
	<Nav
		me="{data.me}"
		cart="{data.cart}"
		store="{data.store}"
		q="{data.q}"
		bind:showCartSidebar="{showCartSidebar}"
		bind:openSidebar="{openSidebar}" />

	<div
		class="{hellobar?.active?.val
			? slotMarginGeneralWithHelloBar
			: slotMarginGeneral} flex h-full w-full antialiased">
		{#if menu?.length > 0}
			<div
				class="relative hidden w-44 shrink-0 overflow-y-auto overflow-x-hidden bg-primary-500 py-3 scrollbar-none sm:block">
				<!-- Dashboard -->

				<a href="/my" aria-label="Click to visit dashboard" data-sveltekit-preload-data>
					<button
						type="button"
						class="w-full p-3 text-left text-sm text-zinc-400 focus:outline-none">
						Dashboard
					</button>
				</a>

				<!-- Sidebar Data -->

				{#each menu as s}
					<SidebarDashboard me="{me}" sidebar="{s}" />
				{/each}
			</div>
		{/if}

		<div class="h-[89vh] w-full flex-1 overflow-y-auto p-3 py-5 sm:p-10">
			<PageTransitions url="{data.url}">
				<slot />
			</PageTransitions>
		</div>
	</div>
</div>
