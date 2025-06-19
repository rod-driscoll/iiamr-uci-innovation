<script lang="ts">
  import { qrwcSvelte } from "../qrwc";

  const gainControlsComponent = qrwcSvelte.useComponent("MyGain")
  const muteButton = gainControlsComponent.useButton("mute");
  const gainControl = gainControlsComponent.useKnob("gain");
</script>

<button
  style="background-color: {muteButton.state ? 'green' : '#777'};"
  onclick={() => muteButton.toggle()}>Toggle!
</button>
<input type="range" 
    bind:value={gainControl.value}
    min={gainControl.valueMin}
    max={gainControl.valueMax}
/>