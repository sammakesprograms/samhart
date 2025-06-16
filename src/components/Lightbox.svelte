<script>
    export let images = [];

    let currentIndex = 0;
    let isOpen = false;

    function openLightbox(index) {
        currentIndex = index;
        isOpen = true;
        window.addEventListener('keydown', handleKey);
    }

    function closeLightbox() {
        isOpen = false;
        window.removeEventListener('keydown', handleKey);
    }

    function next() {
        currentIndex = (currentIndex + 1) % images.length;
    }

    function prev() {
        currentIndex = (currentIndex - 1 + images.length) % images.length;
    }

    function handleKey(event) {
        if (!isOpen) return;
        if (event.key === 'Escape') closeLightbox();
        if (event.key === 'ArrowRight') next();
        if (event.key === 'ArrowLeft') prev();
    }
</script>

<style>
    .gallery {
        display: flex;
        gap: 1rem;
        flex-wrap: wrap;
    }
    .gallery button {
        all: unset;
        cursor: pointer;
        max-width: 150px;
        border-radius: 8px;
        transition: transform 0.2s;
    }
    .gallery button:hover img {
        transform: scale(1.05);
    }
    .gallery img {
        display: block;
        width: 100%;
        border-radius: 8px;
    }
    .lightbox {
        position: fixed;
        inset: 0;
        background: rgba(0,0,0,0.85);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 1000;
    }
    .lightbox-content {
        position: relative;
        max-width: 90vw;
        max-height: 90vh;
        display: flex;
        flex-direction: column;
        align-items: center; /* centers content horizontally */
        justify-content: center;
        text-align: center; /* centers caption text */
        gap: 0.5rem; /* space between image and caption */
    }
    .lightbox-content img {
        max-width: 100vw;
        max-height: 60vh;
        border-radius: 8px;
        display: block;
    }
    .caption {
        color: white;
        font-size: 1rem;
        background: rgba(0,0,0,0.5);
        padding: 0.25rem 0.5rem;
        border-radius: 4px;
        max-width: 90vw;
    }
    .close, .prev, .next {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        background: rgba(255 255 255 / 0.2);
        color: white;
        border: none;
        padding: 0.5rem 1rem;
        cursor: pointer;
        border-radius: 4px;
        font-size: 2rem;
        user-select: none;
    }
    .close {
        top: 1rem;
        right: 1rem;
        transform: none;
        font-size: 2.5rem;
    }
    .prev {
        left: -3rem;
    }
    .next {
        right: -3rem;
    }
</style>

<div class="gallery">
    {#each images as image, index}
        <button
                on:click={() => openLightbox(index)}
                aria-label={`Open image: ${image.alt}`}
        >
            <img src={image.src} alt={image.alt} />
        </button>
    {/each}
</div>

{#if isOpen}
    <div
            class="lightbox"
            role="dialog"
            aria-modal="true"
            tabindex="-1"
            on:click={closeLightbox}
    >
        <div class="lightbox-content" on:click|stopPropagation>
            <button class="close" on:click={closeLightbox} aria-label="Close lightbox">&times;</button>
            <button class="prev" on:click={prev} aria-label="Previous image">&#10094;</button>
            <img src={images[currentIndex].src} alt={images[currentIndex].alt} />
            {#if images[currentIndex].caption}
                <div class="caption">{images[currentIndex].caption}</div>
            {/if}
            <button class="next" on:click={next} aria-label="Next image">&#10095;</button>
        </div>
    </div>
{/if}
