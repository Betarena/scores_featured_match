<!-- 
=================
    PRE-COMPONENT JS SCRIPT,
    PRE-LOADING CRITICAL COMPONENT DATA,
    #####
    - pre-loading `featured_match` data;
=================
-->
<script lang="ts" context="module">

	/** 
	 * @type {import('@sveltejs/kit').Load} 
	*/
	export async function load({ page, fetch }) {
		// ... DEBUGGING;
		if (dev) console.debug('-- obtaining translations! --');
		// ... GET RESPONSE;
		const response = await fetch('/api/featured_match/cache-seo.json', {
			method: 'GET'
		}).then((r) => r.json());
		// ... DEBUGGING;
		// if (dev) console.debug('-- preloaded_translations_response_qty --', response);
		// ... return, RESPONSE DATA;
		if (response) {
			return {
				props: {
					FEATURED_MATCH_WIDGET_DATA_SEO: response
				}
			};
		}
		// ... otherwise, ERROR;
		return {
			status: response.status,
			error: new Error(`/ page-preloading-error`)
		};
	}
</script>

<!-- ===================
	COMPONENT JS - BASIC 
    [TypeScript Written]
=================== -->
<script lang="ts">
	import { dev } from '$app/env';

	import { onMount } from 'svelte';

	import SvelteSeo from 'svelte-seo';
	import FeaturedMatchWidget from '$lib/components/featured_match/_FeaturedMatch_Widget.svelte';

	import { userBetarenaSettings } from '$lib/store/user-settings';

	export let FEATURED_MATCH_WIDGET_DATA_SEO;


	/**
	 * Description:
	 * ~~~~~~~~~~~~~~~~~
	 * This function loads when all of the
	 * rest of the components have loaded
	 * and rendered, checking via JS the viewport
	 * of the client device and changing between
	 * appropiate components to display the correct
	 * component, tailored to a specifc device.
	 */
	 let mobileExclusive: boolean = false;

	onMount(async () => {
		var wInit = document.documentElement.clientWidth;
		// MOBILE - VIEW
		if (wInit < 475) {
			mobileExclusive = true;
		} else {
			mobileExclusive = false;
		}
		window.addEventListener('resize', function () {
			var w = document.documentElement.clientWidth;
			// MOBILE - VIEW
			if (w < 475) {
				mobileExclusive = true;
			} else {
				mobileExclusive = false;
			}
		});
	});
</script>

<!-- ===================
	SVELTE INJECTION TAGS
=================== -->

<!-- adding SEO title and meta-tags to the /basket page -->
<SvelteSeo
	title="Betarena"
	description="Betarena"
	keywords="Betarena, 
        scores platform"
	noindex={false}
	nofollow={false}
	canonical="https://www.betarena.com/"
	twitter={{
		site: '@username',
		title: 'Betarena',
		description: 'Betarena',
		image: 'https://www.example.com/images/cover.jpg',
		imageAlt: 'Alt text for the card!'
	}}
	openGraph={{
		title: 'Betarena',
		description: 'Betarena',
		url: 'https://www.betarena.com/',
		type: 'website',
		images: [
			{
				url: 'https://www.example.com/images/og-image.jpg',
				width: 850,
				height: 650,
				alt: 'Og Image Alt'
			}
		]
	}}
	jsonLd={{
		'@type': 'Article',
		mainEntityOfPage: {
			'@type': 'WebPage',
			'@id': 'https://example.com/article'
		},
		headline: 'Article headline',
		image: [
			'https://example.com/photos/1x1/photo.jpg',
			'https://example.com/photos/4x3/photo.jpg',
			'https://example.com/photos/16x9/photo.jpg'
		],
		datePublished: '2020-08-03T17:31:37Z',
		dateModified: '2020-08-20T09:31:37Z',
		author: {
			'@type': 'Person',
			name: 'John Doe'
		},
		publisher: {
			'@type': 'Organization',
			name: 'Google',
			logo: {
				'@type': 'ImageObject',
				url: 'https://example.com/logo.jpg'
			}
		}
	}}
/>

<!-- ===================
	COMPONENT HTML
=================== -->

<section id="home-page">
	<!-- ... 3rd ROW ... -->
	<FeaturedMatchWidget {FEATURED_MATCH_WIDGET_DATA_SEO} />

	<!-- ... widget-options ... -->
	<div>
		<!-- ... widget-language-example ... -->
		<div>
			<p class='m-b-15 s-16 w-500'> 
				TRANSLATIONS 
			</p>
			<ul>
				<!-- ... english ... -->
				<li class='m-b-15'>
					<a sveltekit:prefetch 
						href="/">
						<p class='s-16'>
							EN
						</p>
					</a>
				</li>
				<!-- ... spanish ... -->
				<li class='m-b-15'>
					<a sveltekit:prefetch 
						href="/es">
						<p class='s-16'>
							ES
						</p>
					</a>
				</li>
				<!-- ... italian ... -->
				<li class='m-b-15'>
					<a sveltekit:prefetch 
						href="/it">
						<p class='s-16'>
							IT
						</p>
					</a>
				</li>
			</ul>
		</div>
		<!-- ... widget-dark-mode-exaple ... -->
		<div>
			<p class='m-b-15 s-16 w-500'> 
				DARK MODE 
			</p>
			<button class='m-b-15'
				on:click={() => $userBetarenaSettings.theme = 'Dark'}>
				DARK-MODE
			</button>
			<button class='m-b-15'
				on:click={() => $userBetarenaSettings.theme = 'Light'}>
				LIGHT-MODE
			</button>
		</div>
	</div>

</section>

<!-- ===================
	COMPONENT STYLE
=================== -->
<style>
	li {
		box-shadow: 0 0 5px #cccccc;
		background-color: white;
		border-radius: 2.5px;
		list-style: none;
		border: none;
		width: fit-content;
		padding: 5px;
	} li:hover {
		background-color: black;
		color: white;
	} li:hover p {
		color: white;
	}

	section#home-page {
		display: grid;
		max-width: 1430px;
		grid-template-columns: 1fr;
	}

	/* 
    RESPONSIVE FOR TABLET (&+) [768px] */
	@media only screen and (min-width: 768px) {
		section#home-page {
			grid-template-columns: 1fr;
		}
	}

	/* 
    RESPONSIVE FOR DESKTOP ONLY (&+) [1440px] */
	@media only screen and (min-width: 1024px) {
		section#home-page {
			gap: 20px;
			grid-template-columns: minmax(auto, 502px) auto auto;
		}
	}
</style>
