<script>
    import { onMount } from 'svelte';
    import { discordLink } from '$lib';

    onMount(() => {
        window.location.href = discordLink;
    });
</script>