<script lang="ts">
    import axios from 'axios';

    let url = "";
    let error = "";
    let isDownloading = false;
    let videoUrl = "";

    async function handleDownload() {
        error = "";
        videoUrl = "";

        console.log("URL:", url);

        if (url) {
            if (isDownloading) {
                error = "Download in progress. Please wait.";
                return;
            }

            try {
                isDownloading = true;
                const response = await axios.get(
                    `https://api.cleesim.com/v1/youtube/download?url=${encodeURIComponent(url)}`
                );

                if (response.status === 200) {
                    const bytes_string = response.data.bytes;
                    const binary = atob(bytes_string);
                    const bytes = new Uint8Array(binary.length);
                    for (let i = 0; i < binary.length; i++) {
                        bytes[i] = binary.charCodeAt(i);
                    }
                    const blob = new Blob([bytes], { type: 'video/mp4' });
                    const videoBlobUrl = URL.createObjectURL(blob);
                    videoUrl = videoBlobUrl;
                    isDownloading = false;

                    const link = document.createElement('a');
                    link.href = videoBlobUrl;
                    link.download = 'video.mp4';
                    link.click();
                } else {
                    error = `Download failed: ${response.status} - ${response.statusText}`;
                    isDownloading = false;
                }
            } catch (err) {
                console.error("Download error:", err);
                isDownloading = false;
                if (axios.isAxiosError(err)) {
                    error = `An error occurred: ${err.message}`;
                } else {
                    error = "An unknown error occurred.";
                }
            }
        } else {
            error = "Please enter a URL.";
        }
    }
</script>

<style>
    .error {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 50px;
        margin: 0 auto;
        margin-top: 20px;
        background-color: transparent;
        color: red;
        font-size: 18px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .downloading {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 50px;
        margin: 0 auto;
        margin-top: 20px;
        background-color: transparent;
        color: blue;
        font-size: 18px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .video-preview {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 50px;
        margin: 0 auto;
        margin-top: 20px;
        background-color: transparent;
        color: white;
        font-size: 18px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .url-input {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 250px;
        height: 80px;
        margin: 0 auto;
        margin-top: 20px;
        background-color: #007bff;
        color: white;
        font-size: 18px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .download-button {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 50px;
        margin: 0 auto;
        margin-top: 20px;
        background-color: #007bff;
        color: white;
        font-size: 18px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
</style>

<div>
    <h1 style="text-align: center;">Download YouTube Video</h1>
    <input type="text" bind:value="{url}" placeholder="Enter YouTube URL" class="url-input" />
    <button class="download-button" on:click="{handleDownload}">Download</button>
    {#if isDownloading}
        <p class="downloading">Downloading...</p>
    {/if}
    {#if error}
        <p class="error">{error}</p>
    {/if}
    {#if videoUrl}
        <div class="video-preview">
            <p>Downloaded Video URL:</p>
            <a href="{videoUrl}" target="_blank">{videoUrl}</a>
        </div>
    {/if}
</div>
