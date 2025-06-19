<script lang="ts">
  import { qrwcSvelte } from "../qrwc";

  const labelsComponent = qrwcSvelte.useComponent("CustomControls")
  const label1 = labelsComponent.useText("text.1");
  const label2 = labelsComponent.useText("text.2");
  const label3 = labelsComponent.useText("text.3");
</script>

<h1>{label1.string}</h1>
<h3>{label2.string}</h3>
<h2>{label3.string}</h2>