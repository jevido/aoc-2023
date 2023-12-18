<script>
	import { page } from '$app/stores';

	let day = $page.route.id.split('/').pop();
</script>

<h2>Day: {day} <a href="/">go back</a></h2>

<slot />
