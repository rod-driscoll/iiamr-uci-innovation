<!-- BottomBarComponent.svelte (Blue Bottom Bar) -->
<script lang="ts">
  import { qrwcSvelte } from "../qrwc";
  let { roomController } = $props();

  const roomControlsComponent = qrwcSvelte.useComponent(roomController);
  const muteButton = roomControlsComponent.useButton("ProgramMute");
  const micMuteButton = roomControlsComponent.useButton("MicMute");
  const volumeUpButton = roomControlsComponent.useButton("ProgramVolumeUp");
  const volumeDownButton = roomControlsComponent.useButton("ProgramVolumeDown");
  //const volumeLevel      = roomControlsComponent.useKnob("ProgramVolume");
  const volumeLevelPercent = roomControlsComponent.useKnob("ProgramVolumePercent");
  const roomPIN = roomControlsComponent.useText("RoomPIN");

  let bottomBarStates = $state({
    isPanelLocked: false,
    isContentSharing: false,
    isCameraOn: true,
    isBluetoothPairing: false,
  });

  let showPinModal = $state(false);
  let pinInput = $state("");
  let showPin = $state(false);

  type BottomBarStateKey = 'isPanelLocked' | 'isContentSharing' | 'isCameraOn' | 'isBluetoothPairing';
  function updateState(key: BottomBarStateKey) {
    if (key === "isPanelLocked") {
      showPinModal = true;
    } else {
      bottomBarStates[key] = !bottomBarStates[key];
    }
  }

  function handlePinInput(value: string) {
    if (value === "Del") {
      pinInput = pinInput.slice(0, -1);
    } else if (value === "Clear") {
      pinInput = "";
    } else if (pinInput.length < 10) {
      pinInput += value;
    }
  }

  function togglePinVisibility() {
    showPin = !showPin;
  }

  function closePinModal() {
    showPinModal = false;
    pinInput = "";
    showPin = false;
  }

  function submitPin() {
    if (pinInput === roomPIN.string) {
      bottomBarStates.isPanelLocked = false;
    closePinModal();
    } else {
      alert("Incorrect PIN. Please try again.");
      pinInput = "";
    }
  }

  function getBottomIcon(iconName) {
    const icons = {
      lock: `<svg class="bottom-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
        <circle cx="12" cy="16" r="1"></circle>
        <path d="m7 11V7a5 5 0 0 1 10 0v4"></path>
      </svg>`,
      share: `<svg class="bottom-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M4 12v8a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-8"></path>
        <polyline points="16,6 12,2 8,6"></polyline>
        <line x1="12" y1="2" x2="12" y2="15"></line>
      </svg>`,
      camera: `<svg class="bottom-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M14.5 4h-5L7 7H4a2 2 0 0 0-2 2v9a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-3l-2.5-3z"></path>
        <circle cx="12" cy="13" r="3"></circle>
      </svg>`,
      bluetooth: `<svg class="bottom-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M6.5 6.5 18 18l-5.5-5.5"></path>
        <path d="M6.5 17.5 18 6l-5.5 5.5"></path>
        <line x1="12" y1="2" x2="12" y2="22"></line>
      </svg>`,
      mic: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"></path>
        <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
        <line x1="12" y1="19" x2="12" y2="23"></line>
        <line x1="8" y1="23" x2="16" y2="23"></line>
      </svg>`,
      micOff: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M12 2a3 3 0 0 0-3 3v7a3 3 0 0 0 6 0V5a3 3 0 0 0-3-3Z"></path>
        <path d="M19 10v2a7 7 0 0 1-14 0v-2"></path>
        <line x1="12" y1="19" x2="12" y2="23"></line>
        <line x1="8" y1="23" x2="16" y2="23"></line>
        <line x1="2" y1="2" x2="22" y2="22" stroke="red"></line>
      </svg>`,
      volume: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M11 5 6 9H2v6h4l5 4V5Z"></path>
        <path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path>
        <path d="M19.07 4.93a10 10 0 0 1 0 14.14"></path>
      </svg>`,
      volumeOff: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M11 5 6 9H2v6h4l5 4V5Z"></path>
        <line x1="22" y1="9" x2="16" y2="15" stroke="red"></line>
        <line x1="16" y1="9" x2="22" y2="15" stroke="red"></line>
      </svg>`,
      eye: `<svg class="eye-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
        <circle cx="12" cy="12" r="3"></circle>
        <path d="m2 2 20 20" stroke="red"></path>
      </svg>`,
      eyeOff: `<svg class="eye-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
        <circle cx="12" cy="12" r="3"></circle>
      </svg>`
    };
    if (iconName in icons) {
      return icons[iconName];
    }
    return "";
  }
</script>

<div class="bottom-bar">
  <div class="controls-left">
    <button
      class="control-btn {bottomBarStates.isPanelLocked ? 'active' : ''}"
      onclick={() => updateState("isPanelLocked")}
    >
      {@html getBottomIcon("lock")}
      <span class="btn-label">Lock panel</span>
    </button>

    <button
      class="control-btn {bottomBarStates.isContentSharing ? 'active' : ''}"
      onclick={() => updateState("isContentSharing")}
    > 
      {@html getBottomIcon("share")}
      <span class="btn-label">Content sharing</span>
    </button>

    <button
      class="control-btn {bottomBarStates.isCameraOn ? 'active' : ''}"
      onclick={() => updateState("isCameraOn")}
    >
      {@html getBottomIcon("camera")}
      <span class="btn-label">Camera controls</span>
    </button>

    <button
      class="control-btn {bottomBarStates.isBluetoothPairing ? 'active' : ''}"
      onclick={() => updateState("isBluetoothPairing")}
    >
      {@html getBottomIcon("bluetooth")}
      <span class="btn-label">Bluetooth pairing</span>
    </button>
  </div>

  <div class="controls-right">
    <button class="icon-btn" onclick={() => micMuteButton.toggle()}>
      {@html getBottomIcon(micMuteButton.state ? "mic" : "micOff")}
    </button>

    <button class="icon-btn" onclick={() => muteButton.toggle()}>
      {@html getBottomIcon(muteButton.state ? "volumeOff" : "volume")}
    </button>

    <div class="volume-controls">
      <button
        class="volume-nudge"
        onmousedown={() => (volumeDownButton.state = true)}
        onmouseup={() => (volumeDownButton.state = false)}>-</button
      >
      <span class="volume-level">{Math.round(volumeLevelPercent.value)}%</span>
      <button
        class="volume-nudge"
        onmousedown={() => (volumeUpButton.state = true)}
        onmouseup={() => (volumeUpButton.state = false)}>+</button
      >
    </div>
  </div>
</div>

<!-- PIN Modal -->
{#if showPinModal}
  <div class="pin-modal-overlay" onclick={closePinModal}>
    <div class="pin-modal" onclick={(e) => e.stopPropagation()}>
      <h2 class="pin-title">Enter access code</h2>
      
      <div class="pin-input-container">
        <div class="pin-input">
          {#if showPin}
            <span class="pin-text">{pinInput}</span>
          {:else}
            <span class="pin-text">{'*'.repeat(pinInput.length)}</span>
          {/if}
        </div>
        <button class="eye-button {showPin ? 'eye-active' : ''}" onclick={togglePinVisibility}>
          {@html getBottomIcon(showPin ? 'eyeOff' : 'eye')}
        </button>
      </div>
      
      <div class="pin-keypad">
        <button class="pin-key" onclick={() => handlePinInput('1')}>1</button>
        <button class="pin-key" onclick={() => handlePinInput('2')}>2</button>
        <button class="pin-key" onclick={() => handlePinInput('3')}>3</button>
        <button class="pin-key" onclick={() => handlePinInput('4')}>4</button>
        <button class="pin-key" onclick={() => handlePinInput('5')}>5</button>
        <button class="pin-key" onclick={() => handlePinInput('6')}>6</button>
        <button class="pin-key" onclick={() => handlePinInput('7')}>7</button>
        <button class="pin-key" onclick={() => handlePinInput('8')}>8</button>
        <button class="pin-key" onclick={() => handlePinInput('9')}>9</button>
        <button class="pin-key pin-special" onclick={() => handlePinInput('Del')}>Del</button>
        <button class="pin-key" onclick={() => handlePinInput('0')}>0</button>
        <button class="pin-key pin-special" onclick={() => handlePinInput('Clear')}>Clear</button>
      </div>
      
      <div class="pin-actions">
        <!--
        <button class="pin-action-btn pin-cancel" onclick={closePinModal}>Cancel</button>
        -->
        <button class="pin-action-btn pin-submit" onclick={submitPin}>Submit</button>
      </div>
    </div>
  </div>
{/if}


<style>
  .bottom-bar {
    background-color: #1e3a8a;
    color: white;
    padding: 0.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .controls-left {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    flex-wrap: wrap;
  }

  .controls-right {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .control-btn {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0.375rem;
    border: none;
    background: none;
    color: white;
    cursor: pointer;
    border-radius: 0.25rem;
    transition: background-color 0.2s;
    touch-action: manipulation;
  }

  .control-btn.active {
    background-color: #1e40af;
  }

  .control-btn:hover {
    background-color: rgba(255, 255, 255, 0.1);
  }

  .btn-label {
    font-size: 0.625rem;
    margin-top: 0.125rem;
    text-align: center;
    line-height: 1.1;
  }

  .icon-btn {
    padding: 0.375rem;
    border: none;
    background: none;
    color: white;
    cursor: pointer;
    border-radius: 0.25rem;
    touch-action: manipulation;
  }

  .icon-btn:hover {
    background-color: rgba(255, 255, 255, 0.1);
  }

  .volume-controls {
    display: flex;
    align-items: center;
    gap: 0.375rem;
  }

  .volume-level {
    font-size: 1.125rem;
    font-weight: bold;
  }

  .volume-nudge {
    font-size: 1.125rem;
    font-weight: bold;
    border: none;
    background: none;
    color: white;
    cursor: pointer;
    padding: 0.125rem 0.375rem;
    border-radius: 0.25rem;
    touch-action: manipulation;
  }

  .volume-up:hover {
    background-color: rgba(255, 255, 255, 0.1);
  }

  :global(.bottom-icon) {
    width: 18px;
    height: 18px;
  }

  :global(.audio-icon) {
    width: 20px;
    height: 20px;
  }

  @media (min-width: 768px) {
    .bottom-bar {
      padding: 0.75rem 1.5rem;
      gap: 1rem;
    }

    .controls-left {
      gap: 1.5rem;
    }

    .controls-right {
      gap: 1rem;
    }

    .control-btn {
      padding: 0.5rem;
    }

    .btn-label {
      font-size: 0.75rem;
      margin-top: 0.25rem;
    }

    .icon-btn {
      padding: 0.5rem;
    }

    .volume-controls {
      gap: 0.5rem;
    }

    .volume-level {
      font-size: 1.5rem;
    }

    .volume-up {
      font-size: 1.5rem;
      padding: 0.25rem 0.5rem;
    }

    :global(.bottom-icon) {
      width: 22px;
      height: 22px;
    }

    :global(.audio-icon) {
      width: 26px;
      height: 26px;
    }
  }

  @media (min-width: 1024px) {
    .bottom-bar {
      padding: 1rem 2rem;
    }

    .controls-left {
      gap: 2rem;
    }

    .volume-level {
      font-size: 1.75rem;
    }

    .volume-up {
      font-size: 1.75rem;
    }

    :global(.bottom-icon) {
      width: 24px;
      height: 24px;
    }

    :global(.audio-icon) {
      width: 30px;
      height: 30px;
    }
  }

  @media (max-width: 320px) {
    .controls-left {
      gap: 0.5rem;
    }

    .btn-label {
      font-size: 0.5rem;
    }

    .volume-level {
      font-size: 1rem;
    }

    .volume-up {
      font-size: 1rem;
    }

    :global(.bottom-icon) {
      width: 16px;
      height: 16px;
    }

    :global(.audio-icon) {
      width: 18px;
      height: 18px;
    }
  }

  /* PIN Modal Styles */
  .pin-modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }

  .pin-modal {
    background-color: #c5c5c5;
    border-radius: 8px;
    padding: 2rem;
    width: 90%;
    max-width: 400px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  }

  .pin-title {
    color: white;
    font-size: 1.5rem;
    font-weight: 600;
    text-align: center;
    margin: 0 0 1.5rem 0;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  }

  .pin-input-container {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 1.5rem;
  }

  .pin-input {
    flex: 1;
    background-color: white;
    border: 2px solid #999;
    border-radius: 4px;
    padding: 0.75rem;
    font-size: 1.5rem;
    text-align: center;
    min-height: 3rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .pin-text {
    color: black;
    font-family: monospace;
    letter-spacing: 0.2em;
  }

  .eye-button {
    background-color: #666;
    border: none;
    border-radius: 4px;
    padding: 0.75rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    min-width: 3rem;
  }

  .eye-button:hover {
    background-color: #555;
  }
  
  .eye-button.eye-active {
    background-color: #2563eb;
  }

  .eye-button.eye-active:hover {
    background-color: #1d4ed8;
  }

  :global(.eye-icon) {
    width: 24px;
    height: 24px;
  }

  .pin-keypad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.5rem;
    margin-bottom: 1.5rem;
  }

  .pin-key {
    background-color: #a8a8a8;
    border: 1px solid #888;
    border-radius: 4px;
    padding: 1rem;
    font-size: 1.5rem;
    font-weight: 600;
    color: black;
    cursor: pointer;
    transition: all 0.2s;
    min-height: 4rem;
    touch-action: manipulation;
  }

  .pin-key:hover {
    background-color: #999;
    transform: scale(0.98);
  }

  .pin-key:active {
    background-color: #777;
    transform: scale(0.95);
  }

  .pin-special {
    font-size: 1.1rem;
    font-weight: 500;
  }

  .pin-actions {
    display: flex;
    gap: 1rem;
    justify-content: center;
  }

  .pin-action-btn {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    touch-action: manipulation;
  }

  .pin-cancel {
    background-color: #666;
    color: white;
  }

  .pin-cancel:hover {
    background-color: #555;
  }

  .pin-submit {
    background-color: #2563eb;
    color: white;
  }

  .pin-submit:hover {
    background-color: #1d4ed8;
  }

  @media (min-width: 768px) {
    .pin-modal {
      padding: 2.5rem;
      max-width: 450px;
    }

    .pin-title {
      font-size: 1.75rem;
    }

    .pin-input {
      padding: 1rem;
      font-size: 1.75rem;
      min-height: 3.5rem;
    }

    .eye-button {
      padding: 1rem;
      min-width: 3.5rem;
    }

    .pin-key {
      padding: 1.25rem;
      font-size: 1.75rem;
      min-height: 4.5rem;
    }

    .pin-special {
      font-size: 1.25rem;
    }

    :global(.eye-icon) {
      width: 28px;
      height: 28px;
    }
  }

  @media (min-width: 1024px) {
    .pin-modal {
      max-width: 500px;
    }

    .pin-key {
      min-height: 5rem;
    }
  }
</style>
