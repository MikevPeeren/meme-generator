<script>
    import ImageUploader from './components/ImageUploader.svelte';
</script>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }

    .memeCanvas {
        margin: 20px;
        border: 1px solid #000000;
        border-radius: 5px;
        width: 500px;
        height: 400px;
    }
</style>

<main>
    <h1>Meme Generator!</h1>

    <ImageUploader />

    <canvas class="memeCanvas" id="memeCanvas" />
</main>
