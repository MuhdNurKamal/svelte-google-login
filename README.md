# svelte-google-login

Easy to use Google Login Button for Svelte

# Usage

```html
<script lang="ts">
	import { GoogleLoginButton } from 'svelte-google-login';
	const handleLoginSuccess = (googleUser: gapi.auth2.GoogleUser) => Promise.resolve();
	const handleLoginFailure = () => {};
</script>

<GoogleLoginButton
	clientId="GOOGLE_CLIENT_ID"
	onSuccess="{handleLoginSuccess}"
	onFailure="{handleLoginFailure}"
/>
```
