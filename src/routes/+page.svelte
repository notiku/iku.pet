<script>
    export let name = 'iku';
    export let subtitle = 'i code stuff';
    export let discordLink = 'https://discord.gg/kN4xCcVncj';
    export let githubLink = 'https://github.com/notiku';
    // export let twitterLink = 'https://twitter.com/ikucoder';
    export let email = 'contact@iku.pet';
    // export let discordBotLink = 'https://bot.iku.pet/';
    export let donateLink = 'https://patreon.com/notiku';
    // export let legalNoticeLink = 'https://iku.pet/legal-notice';
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Raleway:wght@200;300;400;500;600;700;800;900&display=swap');

    :global(*) {
        font-family: Raleway, 'sans-serif';
    }

    :global(body) {
        background: lightgray;
        width: 100%;
    }

    .centered {
        display: flex;
        flex-direction: column;
        align-items: center;
        row-gap: 20px;
        position: absolute;
        top: 20%;
        left: 50%;
        transform: translateX(-50%);
        width: 100%;
    }

    .brand {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        row-gap: 10px;
    }

    .name {
        font-size: 7em;
        font-weight: bold;
    }

    .subtitle {
        font-size: 2em;
        opacity: 70%;
        letter-spacing: .1em;
        text-align: center;
    }

    .socials {
        display: flex;
        column-gap: 20px;
    }

    .socials a {
        position: relative;
    }

    .socials a:hover::before {
        color: #fff;
        background: #000;
        content: attr(tooltip);
        position: absolute;
        top: 40px;
        left: 50%;
        transform: translateX(-50%);
        z-index: 100;
        animation: fadeIn .5s ease;
        padding: 5px;
        border-radius: 5px;
    }

    .socials img {
        width: 35px;
        height: 35px;
    }

    .socials img.mail {
        border-radius: 10px;
    }

    .links a {
        color: rgba(0, 0, 0, .7);
        text-decoration: none;
    }

    .links a:hover {
        text-decoration: underline;
        color: rgba(0, 0, 0, .8);
    }

    @keyframes fadeIn {
        0% {
            opacity: 0%;
            transform: translateX(-50%) translateY(5px);
        }
        100% {
            opacity: 100%;
        }
    }
</style>

<div class="centered">
    <div class="brand">
        <div class="name">{name}</div>
        <div class="subtitle">{subtitle}</div>
    </div>
    <div class="socials">
        <a target="_blank" href={discordLink} tooltip="schizo"><img src="discord.svg" class="discord" alt=""></a>
        <a target="_blank" href={githubLink} tooltip="@notiku"><img src="github.svg" class="github" alt=""></a>
        <!-- <a target="_blank" href={twitterLink}><img src="twitter.svg" class="twitter" alt=""></a> -->
        <a target="_blank" href={`mailto:${email}`} tooltip="contact@iku.pet"><img src="mail.svg" class="mail" alt=""></a>
    </div>
    <div class="links">
        <a target="_blank" href={donateLink}>Donate</a>
    </div>
</div>