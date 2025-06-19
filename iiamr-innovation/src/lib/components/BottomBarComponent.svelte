<!-- BottomBarComponent.svelte (Blue Bottom Bar) -->
<script>
  let bottomBarStates = $state({
    isPanelLocked: false,
    isContentSharing: false,
    isCameraOn: true,
    isBluetoothPairing: false,
    isMicOn: true,
    isMuted: false,
    volumeLevel: 87
  });

  function updateState(key) {
    bottomBarStates[key] = !bottomBarStates[key];
  }

  function handleVolumeUp() {
    bottomBarStates.volumeLevel = Math.min(100, bottomBarStates.volumeLevel + 5);
  }
  function handleVolumeDown() {
    bottomBarStates.volumeLevel = Math.max(0, bottomBarStates.volumeLevel - 5);
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
        <line x1="2" y1="2" x2="22" y2="22"></line>
        <path d="M18.89 13.23A7.12 7.12 0 0 0 19 12v-2"></path>
        <path d="M5 10v2a7 7 0 0 0 12 5"></path>
        <path d="M15 9.34V5a3 3 0 0 0-5.68-1.33"></path>
        <path d="M9 9v3a3 3 0 0 0 5.12 2.12"></path>
        <line x1="12" y1="19" x2="12" y2="23"></line>
        <line x1="8" y1="23" x2="16" y2="23"></line>
      </svg>`,
      volume: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M11 5 6 9H2v6h4l5 4V5Z"></path>
        <path d="M15.54 8.46a5 5 0 0 1 0 7.07"></path>
        <path d="M19.07 4.93a10 10 0 0 1 0 14.14"></path>
      </svg>`,
      volumeOff: `<svg class="audio-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M11 5 6 9H2v6h4l5 4V5Z"></path>
        <line x1="22" y1="9" x2="16" y2="15"></line>
        <line x1="16" y1="9" x2="22" y2="15"></line>
      </svg>`
    };
    return icons[iconName] || '';
  }
</script>

<div class="bottom-bar">
  <div class="controls-left">
    <button 
      class="control-btn {bottomBarStates.isPanelLocked ? 'active' : ''}"
      onclick={() => updateState('isPanelLocked')}
    >
      {@html getBottomIcon('lock')}
      <span class="btn-label">Lock panel</span>
    </button>
    
    <button 
      class="control-btn {bottomBarStates.isContentSharing ? 'active' : ''}"
      onclick={() => updateState('isContentSharing')}
    >
      {@html getBottomIcon('share')}
      <span class="btn-label">Content sharing</span>
    </button>
    
    <button 
      class="control-btn {bottomBarStates.isCameraOn ? 'active' : ''}"
      onclick={() => updateState('isCameraOn')}
    >
      {@html getBottomIcon('camera')}
      <span class="btn-label">Camera controls</span>
    </button>
    
    <button 
      class="control-btn {bottomBarStates.isBluetoothPairing ? 'active' : ''}"
      onclick={() => updateState('isBluetoothPairing')}
    >
      {@html getBottomIcon('bluetooth')}
      <span class="btn-label">Bluetooth pairing</span>
    </button>
  </div>
  
  <div class="controls-right">
    <button 
      class="icon-btn"
      onclick={() => updateState('isMicOn')}
    >
      {@html getBottomIcon(bottomBarStates.isMicOn ? 'mic' : 'micOff')}
    </button>
    
    <button 
      class="icon-btn"
      onclick={() => updateState('isMuted')}
    >
      {@html getBottomIcon(bottomBarStates.isMuted ? 'volumeOff' : 'volume')}
    </button>
    
    <div class="volume-controls">
      <button class="volume-nudge" onclick={handleVolumeDown}>-</button>
      <span class="volume-level">{bottomBarStates.volumeLevel}%</span>
      <button class="volume-nudge" onclick={handleVolumeUp}>+</button>
    </div>
  </div>
</div>

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
</style>