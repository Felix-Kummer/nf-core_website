<script lang="ts">
    interface Props {
        name: string;
        count?: number;
        image?: string;
        size?: number;
        circle?: boolean;
        linkClasses?: string;
        wrapperClasses?: string;
        imgClasses?: string;
        containerQuery?: boolean;
        children?: import("svelte").Snippet;
    }

    let {
        name,
        count = 0,
        image = "https://github.com/" + name + ".png",
        size = 60,
        circle = false,
        linkClasses = "",
        wrapperClasses = "",
        imgClasses = "",
        containerQuery = false,
        children,
    }: Props = $props();

    let tooltip = $state(count > 0 ? `${name} (${count} commits)` : name);
    tooltip = count === 1 ? `${name} (1 commit)` : tooltip;
    tooltip = containerQuery ? "" : tooltip;
    const avatar_url = image.match(/\?/) ? `${image}&s=${size}` : `${image}?s=${size}`;
</script>

<div class={"github-profile-wrapper " + wrapperClasses} style="--size:{size};" class:github-profile={containerQuery}>
    <a
        href="https://github.com/{name}"
        class={"text-decoration-none github-link " + linkClasses}
        target="_blank"
        rel="noopener noreferrer"
        style="--size:{size};"
    >
        <img
            src={avatar_url}
            width={size}
            height={size}
            class:rounded-circle={circle}
            data-bs-placement="bottom"
            data-bs-toggle="tooltip"
            title={tooltip}
            alt={`Github user ${name}`}
            style="--size:{size};"
            class={" " + imgClasses}
        />
        <div class="profile-name text-nowrap overflow-x-auto">
            {@render children?.()}
        </div>
    </a>
</div>

<style lang="scss">
    img {
        background-color: var(--bs-light);
    }
    .github-profile-wrapper {
        min-width: fit-content;
        width: calc(var(--size) * 1px + 1rem);
    }
    .github-profile {
        container-type: size;
        container-name: github-profile;
    }
    .github-link {
        max-width: fit-content;
    }
    @container github-profile (width < 13rem) {
        :global(.profile-name) {
            display: none !important;
        }
    }
    :global(.github-profile:hover) {
        z-index: 1000;
        width: 54%; // should cover most cases, but extra long usernames, will still be not completely covered
        :global(.profile-name) {
            display: block !important;
            z-index: 1000;
        }
    }
</style>
