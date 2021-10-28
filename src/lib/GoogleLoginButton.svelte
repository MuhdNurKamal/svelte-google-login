<script lang="ts">
	import asyncScriptLoader from 'async-script-loader';
	import { onMount } from 'svelte';
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

<input on:click={handleClick} type="image" class="w-40" alt="google login button" />

<style>
	input {
		content: url('/img/googleLoginButtons/btn_google_signin_dark_normal_web@2x.png');
	}
	input:hover {
		content: url('/img/googleLoginButtons/btn_google_signin_dark_focus_web@2x.png');
	}
	input:active {
		content: url('/img/googleLoginButtons/btn_google_signin_dark_pressed_web@2x.png');
	}
</style>
