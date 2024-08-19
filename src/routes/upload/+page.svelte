<script lang="ts">
  import axios from 'axios';

  let file: File | null = null;
  let error = "";
  let imageUrl = "";
  let isUploading = false;
  const maxFileSizeMB = 250;

  // Handle file upload using axios
  async function handleUpload() {
    error = "";
    imageUrl = "";

    if (file) {
      if (isUploading) {
        error = "Upload in progress. Please wait.";
        return;
      }

      const fileSizeMB = file.size / (1024 * 1024);

      if (fileSizeMB > maxFileSizeMB) {
        error = `File size exceeds ${maxFileSizeMB} MB limit.`;
        return;
      }

      const formData = new FormData();
      formData.append('file', file);

      try {
        isUploading = true;
        const response = await axios.post(
          'https://api.cleesim.com/v1/media/upload',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
        );

        if (response.status === 200) {
          imageUrl = response.data.image_url || "File uploaded successfully, but no image URL returned.";
          isUploading = false;
        } else {
          error = `Upload failed: ${response.status} - ${response.statusText}`;
          isUploading = false;
        }
      } catch (err) {
        console.error("Upload error:", err);
        isUploading = false;
        if (axios.isAxiosError(err)) {
          error = `An error occurred: ${err.message}`;
        } else {
          error = "An unknown error occurred.";
        }
      }
    } else {
      error = "Please select a file to upload.";
    }
  }

  // Handle file selection
  function handleFileSelect(event: Event) {
    const target = event.target as HTMLInputElement;
    if (target && target.files && target.files.length > 0) {
      file = target.files[0];
    } else {
      file = null;
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
  .uploading {
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
  .image-preview {
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
  .file-input {
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
  .upload-button {
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
  <h1 style="text-align: center;">Upload Your File</h1>
  <button class="upload-button" on:click="{handleUpload}">Upload</button>
  <input type="file" on:change="{handleFileSelect}" class="file-input" />
  {#if isUploading}
    <p class="uploading">Uploading...</p>
  {/if}
  {#if error}
    <p class="error">{error}</p>
  {/if}
  {#if imageUrl}
    <div class="image-preview">
      <p>Uploaded File URL:</p>
      <a href="{imageUrl}" target="_blank">{imageUrl}</a>
    </div>
  {/if}
</div>
