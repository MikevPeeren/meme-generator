<script>
    import { DOWNLOAD_MEME, TOP_TEXT, BOTTOM_TEXT, FONT_SIZE_LABEL, COLOR_IN_HEX } from './constants/general.js';

    let image = null;
    let file = null;

    let textTop = '';
    let textBottom = '';
    let warningText = '';

    let fontSize = 200;
    let textColor = '#ffffff   ';

    const reader = new FileReader();

    let memeCanvas;

    function uploadImage(event) {
        // For future redrawing purposes
        image = new Image();
        file = event.target.files[0];

        drawCanvas(image, event.target.files[0]);
    }

    function drawCanvas(image, file, placeTextTop = false, placeTextBottom = false) {
        const memeCanvasContext = memeCanvas.getContext('2d');

        if (warningText !== '') warningText = '';

        reader.readAsDataURL(file);

        reader.onload = function(event) {
            if (event.target.readyState == FileReader.DONE) {
                image.src = event.target.result;
                memeCanvas.width = image.width;
                memeCanvas.height = image.height;
                memeCanvasContext.drawImage(image, 0, 0);

                if (placeTextTop) placeText('top');
                if (placeTextBottom) placeText('bottom');
            }
        };
    }

    function placeText(placement) {
        if (!image) {
            warningText = 'Please upload an image first';
            return;
        }

        if (warningText !== '') warningText = '';

        const memeCanvasContext = memeCanvas.getContext('2d');

        memeCanvasContext.font = `${fontSize}px Comic Sans`;
        memeCanvasContext.fillStyle = textColor;
        memeCanvasContext.textAlign = 'center';

        if (textTop === '' && textBottom === '') return;

        // Minus 200 because font size is 200px
        const centeredText = memeCanvas.width / 2;

        if (placement === 'top') memeCanvasContext.fillText(textTop, centeredText, 150);
        if (placement === 'bottom') memeCanvasContext.fillText(textBottom, centeredText, memeCanvas.height - 100);
    }

    function saveMeme() {
        const downloadButton = document.getElementById('downloadButton');
        const dataURL = memeCanvas.toDataURL('image/png');
        downloadButton.href = dataURL;
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
        margin-bottom: 15px;
    }

    .button {
        padding: 10px;
        margin: 10px;
        text-align: center;
        opacity: 80%;
        border-radius: 5px;
    }

    .container {
        display: inline-block;
    }

    .containerDivs {
        float: left;
    }
</style>

<div>
    <label>Upload Your Image</label>

    <div>
        <input on:change={event => uploadImage(event)} type="file" id="selectedFile" style="display: none;" />
        <input type="button" value="Browse..." onclick="document.getElementById('selectedFile').click();" />
        <br />
        <a href="#" class="button" id="downloadButton" on:click={() => saveMeme()} download="meme.png">
            {DOWNLOAD_MEME}
        </a>
    </div>

    <canvas bind:this={memeCanvas} class="memeCanvas" id="memeCanvas" />

    <div class="errorMessage">{warningText}</div>

    <div class="container">
        <div class="containerDivs">
            <label for="top-text">{TOP_TEXT}</label>
            <input id="top-text" maxlength="20" bind:value={textTop} on:change={() => placeText('top')} />
        </div>
        <div class="containerDivs">
            <label for="bottom-text">{BOTTOM_TEXT}</label>
            <input id="bottom-text" maxlength="20" bind:value={textBottom} on:change={() => placeText('bottom')} />
        </div>
    </div>
    <div class="container">
        <div class="containerDivs">
            <label for="font-size">{FONT_SIZE_LABEL}</label>
            <input
                id="font-size"
                maxlength="10"
                bind:value={fontSize}
                on:change={() => drawCanvas(image, file, true, true)} />
        </div>
        <div class="containerDivs">
            <label for="text-color">{COLOR_IN_HEX}</label>
            <input id="text-color" maxlength="10" bind:value={textColor} />
        </div>
    </div>
    <br />
    <input type="button" value="Clear Text" on:click={() => drawCanvas(image, file)} />
</div>
