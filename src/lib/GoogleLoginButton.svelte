<script lang="ts">
	import asyncScriptLoader from 'async-script-loader';
	import { onMount } from 'svelte';
	import NormalLogo from './icons/btn_google_dark_normal_ios.svelte';

	export let clientId: string;
	export let onSuccess = async (googleUser: gapi.auth2.GoogleUser) => {};
	export let onFailure = (err: unknown) => console.error(err);
	onMount(() => {
		asyncScriptLoader('https://apis.google.com/js/api.js?onload=CALLBACK_PLACEHOLDER', true)
			.then(() => {
				gapi.load('auth2', function () {
					gapi.auth2.init({
						client_id: clientId
					});
				});
			})
			.catch((err: unknown) => console.error(err));
	});
	const handleClick = () => {
		gapi.load('auth2', function () {
			gapi.auth2
				.getAuthInstance()
				.signIn()
				.then(
					function (googleUser) {
						onSuccess(googleUser);
					},
					function (error) {
						onFailure(error);
					}
				);
		});
	};
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div on:click={handleClick} type="image" class="button">
	<NormalLogo />
	<span>Sign in with Google</span>
</div>

<style>
	.button {
		display: flex;
		align-items: center;
		color: white;
		border-radius: 0.1rem;
		cursor: pointer;
		font-family: 'Roboto';
		background: #4285f4;
	}
	.button:hover {
		background: #4285f4;
	}
	.button:active {
		background: #3367d6;
	}
	span {
		flex-grow: 1;
		text-align: center;
	}
</style>
