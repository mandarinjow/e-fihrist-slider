<script lang="ts">
  export let isactive: boolean;
  export let alt: string;
  export let src: string;
</script>

<div class="mySlides">
  <img alt={alt} src={src} style="width: 100%; display: {isactive ? 'block' : 'none'}"/>
</div>
