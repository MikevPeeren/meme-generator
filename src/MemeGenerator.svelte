<script>
    let image = null;
    let file = null;

    let textTop = '';
    let textBottom = '';
    let warningText = '';

    let fontSize = 200;
    let textColour = '#000000';

    const reader = new FileReader();

    let memeCanvas;

    function uploadImage(event) {
        // For future redrawing purposes
        image = new Image();
        file = event.target.files[0];

        drawCanvas(image, event.target.files[0]);
    }

    function drawCanvas(image, file) {
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
        if (!image) {
            warningText = 'Please upload an image first';
            return;
        }

        if (warningText !== '') warningText = '';

        const memeCanvasContext = memeCanvas.getContext('2d');

        memeCanvasContext.font = `${fontSize}px Comic Sans`;
        memeCanvasContext.fillStyle = textColour;
        memeCanvasContext.textAlign = 'center';

        if (textTop === '' && textBottom === '') return;

        // Minus 200 because font size is 200px
        const centeredText = memeCanvas.width / 2;

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
        margin: 10px;
        border: 1px solid #000000;
        border-radius: 5px;
        width: 500px;
        height: 400px;
    }

    .errorMessage {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 1.5em;
        font-weight: 100;
    }
</style>

<div>
    <label>Upload Your Image</label>

    <div>
        <input on:change={event => uploadImage(event)} type="file" id="selectedFile" style="display: none;" />
        <input type="button" value="Browse..." onclick="document.getElementById('selectedFile').click();" />
    </div>

    <canvas bind:this={memeCanvas} class="memeCanvas" id="memeCanvas" />

    <div class="errorMessage">{warningText}</div>

    <div>
        <input placeholder="Top Text" maxlength="20" bind:value={textTop} on:change={() => placeText('top')} />
        <input placeholder="Bottom Text" maxlength="20" bind:value={textBottom} on:change={() => placeText('bottom')} />
    </div>
    <div>
        <label for="font-size">Font Size in pixels</label>
        <input id="font-size" maxlength="10" bind:value={fontSize} />
    </div>
    <div>
        <label for="text-colour">Colour in hex</label>
        <input id="text-colour" maxlength="10" bind:value={textColour} />
    </div>
    <input type="button" value="Clear Text" on:click={() => drawCanvas(image, file)} />
</div>
