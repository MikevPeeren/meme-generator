<script>
    let image = null;

    function uploadImage(e) {
        const memeCanvas = document.getElementById('memeCanvas');
        const memeCanvasContext = memeCanvas.getContext('2d');

        var reader = new FileReader();
        reader.onload = function(event) {
            var image = new Image();
            image.onload = function() {
                memeCanvas.width = image.width;
                memeCanvas.height = image.height;
                memeCanvasContext.drawImage(image, 0, 0);
            };
            image.src = event.target.result;
        };
        reader.readAsDataURL(e.target.files[0]);
    }
</script>

<style>
    input,
    button {
        margin: 20px;
        text-align: center;
        opacity: 80%;
        border-radius: 5px;
    }
</style>

<div>
    <label for="meme">Upload Your Meme</label>

    <input
        bind:value={image}
        on:change={event => uploadImage(event)}
        type="file"
        id="selectedFile"
        style="display: none;" />
    <input type="button" value="Browse..." onclick="document.getElementById('selectedFile').click();" />
</div>
