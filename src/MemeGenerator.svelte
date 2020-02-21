<script>
    let image = null;
    let file = null;
    let textTop = '';
    let textBottom = '';

    const reader = new FileReader();

    function uploadImage(event) {
        // For future redrawing purposes
        const img = new Image();
        image = img;
        file = event.target.files[0];

        drawCanvas(image, event.target.files[0]);
    }

    function drawCanvas(image, file) {
        // TODO: bind somehow ?
        const memeCanvas = document.getElementById('memeCanvas');
        const memeCanvasContext = memeCanvas.getContext('2d');

        reader.onload = function(event) {
            image.onload = function() {
                memeCanvas.width = image.width;
                memeCanvas.height = image.height;
                memeCanvasContext.drawImage(image, 0, 0);
            };
            image.src = event.target.result;
        };

        reader.readAsDataURL(file);
    }

    function placeText(placement) {
        // TODO: bind somehow ?
        const memeCanvas = document.getElementById('memeCanvas');
        const memeCanvasContext = memeCanvas.getContext('2d');

        memeCanvasContext.font = '200px Comic Sans';

        if (textTop === '' && textBottom === '') return;

        // Minus 200 because font size is 200px
        const centeredText = memeCanvas.width / 2 - 200;

        // TODO: Placement needs to be finetuned.
        if (placement === 'top') memeCanvasContext.fillText(textTop, centeredText, 150);
        if (placement === 'bottom') memeCanvasContext.fillText(textBottom, centeredText, memeCanvas.height - 100);
    }
</script>

<style>
    input {
        padding: 10px;
        margin: 10px;
        text-align: center;
        opacity: 80%;
        border-radius: 5px;
    }

    .memeCanvas {
        margin: 15px;
        border: 1px solid #000000;
        border-radius: 5px;
        width: 500px;
        height: 400px;
    }
</style>

<div>
    <label for="meme">Upload Your Meme</label>

    <div>
        <input on:change={event => uploadImage(event)} type="file" id="selectedFile" style="display: none;" />
        <input type="button" value="Browse..." onclick="document.getElementById('selectedFile').click();" />
    </div>
    <canvas class="memeCanvas" id="memeCanvas" />

    <div>
        <input placeholder="Top Text" bind:value={textTop} on:change={() => placeText('top')} />
        <input placeholder="Bottom Text" bind:value={textBottom} on:change={() => placeText('bottom')} />
    </div>
    <input type="button" value="Clear Text" on:click={() => drawCanvas(image, file)} />
</div>
