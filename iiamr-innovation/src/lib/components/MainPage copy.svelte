
<script lang="ts">
  //import { qrwcSvelte } from "../qrwc";
  import Gain from './Gain.svelte';
  import Label from './Label.svelte';
  import DateTime from './DateTime.svelte';
</script>


  <div class="main-wrapper">
    <header class="Header">
      <div class="Header-left">
        <span class="header-divider"></span>
        <Label/>
      </div>
      <div class="Header-right">
        <DateTime />
        <img class="logo" src="https://inghaminstitute.org.au/wp-content/uploads/2022/10/InghamInstitute_10yrs_RGB.png" alt="company logo" />
      </div>
    </header>
    <main class="ControlArea">
      <Gain/>
    </main>
    <footer class="Footer">
      <div class="footer-buttons">
        <button class="footer-btn">Home</button>
        <button class="footer-btn">Help</button>
        <button class="footer-btn">Settings</button>
      </div>
      <span class="footer-copyright">UCI Innovation Lab &copy; 2025</span>
    </footer>
  </div>