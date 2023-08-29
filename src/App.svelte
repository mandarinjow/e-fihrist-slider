<script lang="ts">
    import { onDestroy } from "svelte";
    import Dot from "./lib/components/Dot.svelte";
    import Image from "./lib/components/Image.svelte";

  let slideLength = 0;
  var currentIndex = 0;

  let intervalId: number | undefined;

  onDestroy(() => {
    clearInterval(intervalId);
  });

  async function fetchImages() {
    const images = await fetch(
      `https://unissa.edu.bn/wp-json/wp/v2/media?happyfiles_category=262`
    );
    const data : ImageResponse[] = await images.json();
    const transformed = data.reverse().map((image: ImageResponse, index: number) => {
      return { source_url: image.source_url, id: index };
    });

    slideLength = transformed.length -1 ;
    intervalId = initializeInterval();

    return transformed;
  }

  function handleMessage(event: { detail: number; }) {
    // Stop the interval
    clearInterval(intervalId);

    // Update the current index
    currentIndex = event.detail;

    // Reinitialize the interval
    intervalId = initializeInterval();
  }

  function initializeInterval() {
    return setInterval(() => {
      // check if the current index exceeds the length of the array
      if (currentIndex >= slideLength) {
        currentIndex = 0;
      } else {
        // Update the current index
        currentIndex++;
      }
    }, 6000);
  }

</script>

<center>
  <div class="slideshow-container">
    {#await fetchImages()}
      <h1>Loading...</h1>
    {:then images}
      {#each images as image}
        <Image alt={image.id.toString()} src={image.source_url} isactive={image.id == currentIndex} />
      {/each}
      <br />
      <div style="text-align: center;">
        {#each images as image}
          <Dot on:message={handleMessage} id={image.id} isactive={image.id == currentIndex} />
        {/each}
      </div>
    <!-- svelte-ignore empty-block -->
    {:catch error}
    {/await}
  </div>
</center>
