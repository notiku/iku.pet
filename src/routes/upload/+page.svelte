<script lang="ts">
  import axios from 'axios';

  let file: File | null = null;
  let error = "";
  let imageUrl = "";
  const maxFileSizeMB = 50;

  // Handle file upload using axios
  async function handleUpload() {
    error = "";
    imageUrl = "";

    if (file) {
      const fileSizeMB = file.size / (1024 * 1024);

      if (fileSizeMB > maxFileSizeMB) {
        error = `File size exceeds ${maxFileSizeMB} MB limit.`;
        return;
      }

      const formData = new FormData();
      formData.append('file', file);

      try {
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
        } else {
          error = `Upload failed: ${response.status} - ${response.statusText}`;
        }
      } catch (err) {
        console.error("Upload error:", err);
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
    color: red;
  }
  .image-preview {
    margin-top: 10px;
  }
</style>

<div>
  <h1>Upload Your File</h1>
  <input type="file" on:change="{handleFileSelect}" />
  <button on:click="{handleUpload}">Upload</button>
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
