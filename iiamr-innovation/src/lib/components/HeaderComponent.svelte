<!-- HeaderComponent.svelte (Top White Section) -->
<script> 
import { qrwcSvelte } from "../qrwc";
import DateTime from './DateTime.svelte';
  let { roomController, mode } = $props();
  //const roomControlsComponent = qrwcSvelte.useComponent(roomController);
  const roomControlsComponent = qrwcSvelte.useComponent("Room 1 controller");
  const roomName = roomControlsComponent.useText("RoomName");
  const subtitle = roomControlsComponent.useText("ProjectName");
</script>

<div class="header">
  <div class="header-left">
    <h1 class="room-name">{roomName.string}</h1>
    <div class="subtitle">{subtitle.string}</div>
    <div class="mode">{mode}</div>
  </div>
  <div class="header-right">
    <DateTime />
    <img class="logo" src="https://inghaminstitute.org.au/wp-content/uploads/2022/10/InghamInstitute_10yrs_RGB.png" alt="company logo" />
  </div>
</div>

<style>
  .header {
    display: flex;
    flex-direction: row;
    background-color: white;
    padding: 1rem;
    color: black;
  }

  .header-left {
    display: flex;
    flex-direction: column;
    width: 50%;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 0.5rem;
  }
  .header-right {
    display: flex;
    flex-direction: column;
    width: 50%;
    justify-content: space-between;
    align-items: flex-end;
    margin-bottom: 0.5rem;
  }

  .room-name {
    font-size: 1.125rem;
    font-weight: 600;
    margin: 0;
  }

  .subtitle {
    font-size: 0.8rem;
    color: #374151;
    margin-bottom: 0.25rem;
  }

  .mode {
    font-size: 0.8rem;
    font-weight: 500;
    color: #1f2937;
  }

  @media (min-width: 768px) {
    .header {
      padding: 1.5rem;
    }
    
    .room-name {
      font-size: 1.5rem;
    }
    
    .timestamp {
      font-size: 0.875rem;
    }
    
    .subtitle, .mode {
      font-size: 0.875rem;
    }
  }

  @media (min-width: 1024px) {
    .room-name {
      font-size: 1.75rem;
    }
  }
</style>