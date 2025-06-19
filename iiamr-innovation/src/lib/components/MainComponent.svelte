<!-- MainComponent.svelte (Main Grid + Bottom Bar) -->
<script>
  import BottomBarComponent from './BottomBarComponent.svelte';
  
  let controlStates = $state({
    teams: true,
    television: false,
    bluetooth: false,
    power: true,
    frontWallPlate: false,
    rearWallPlate: false,
    reception: false
  });
  
  let tiles = $derived([
    { 
      icon: "teams", 
      label: "Teams", 
      isActive: controlStates.teams,
      bgColor: "bg-blue-600",
      type: "single"
    },
    { 
      icon: "wifi", 
      label: "Television", 
      isActive: controlStates.television,
      bgColor: "bg-blue-600",
      type: "single"
    },
    { 
      icon: "bluetooth", 
      label: "Bluetooth", 
      isActive: controlStates.bluetooth,
      bgColor: "bg-blue-600",
      type: "dual",
      subTiles: [
        {
          icon: "bluetooth",
          label: "Bluetooth",
          isActive: controlStates.bluetooth,
          key: "bluetooth"
        },
        {
          icon: "power",
          label: "Power", 
          isActive: controlStates.power,
          key: "power"
        }
      ]
    },
    { 
      icon: "monitor", 
      label: "Front Wall Plate", 
      isActive: controlStates.frontWallPlate,
      bgColor: "bg-blue-600",
      type: "single"
    },
    { 
      icon: "monitor", 
      label: "Rear Wall Plate", 
      isActive: controlStates.rearWallPlate,
      bgColor: "bg-blue-600",
      type: "single"
    },
    { 
      icon: "play", 
      label: "Reception", 
      isActive: controlStates.reception,
      bgColor: "bg-blue-600",
      type: "single"
    }
  ]);

  function handleTileClick(index, subTileKey = null) {
    if (subTileKey) {
      // Handle sub-tile click
      controlStates[subTileKey] = !controlStates[subTileKey];
    } else {
      // Handle regular tile click
      const tileKeys = ['teams', 'television', 'bluetooth', 'frontWallPlate', 'rearWallPlate', 'reception'];
      const key = tileKeys[index];
      if (key) {
        controlStates[key] = !controlStates[key];
      }
    }
  }

  function getMainIcon(iconName) {
    const icons = {
      teams: `<svg class="main-icon" viewBox="0 0 24 24" fill="currentColor">
        <path d="M0,0 L10,1.5 L10,11.5 L0,11.5 Z M12,0 L24,2 L24,11.5 L12,11.5 Z M0,12.5 L10,12.5 L10,22.5 L0,24 Z M12,12.5 L24,12.5 L24,22 L12,24 Z"/>
      </svg>`,
      wifi: `<svg class="main-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M5 12.55a11 11 0 0 1 14.08 0"></path>
        <path d="M1.42 9a16 16 0 0 1 21.16 0"></path>
        <path d="M8.53 16.11a6 6 0 0 1 6.95 0"></path>
        <line x1="12" y1="20" x2="12.01" y2="20"></line>
      </svg>`,
      bluetooth: `<svg class="main-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M6.5 6.5 18 18l-5.5-5.5"></path>
        <path d="M6.5 17.5 18 6l-5.5 5.5"></path>
        <line x1="12" y1="2" x2="12" y2="22"></line>
      </svg>`,
      power: `<svg class="main-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M18.36 6.64a9 9 0 1 1-12.73 0"></path>
        <line x1="12" y1="2" x2="12" y2="12"></line>
      </svg>`,
      monitor: `<svg class="main-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <rect x="2" y="3" width="20" height="14" rx="2" ry="2"></rect>
        <line x1="8" y1="21" x2="16" y2="21"></line>
        <line x1="12" y1="17" x2="12" y2="21"></line>
        <polyline points="9,9 15,15"></polyline>
        <polyline points="15,9 15,15 9,15"></polyline>
      </svg>`,
      play: `<svg class="main-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <rect x="2" y="3" width="20" height="14" rx="2" ry="2"></rect>
        <line x1="8" y1="21" x2="16" y2="21"></line>
        <line x1="12" y1="17" x2="12" y2="21"></line>
        <polygon points="10,8 16,12 10,16"></polygon>
      </svg>`
    };
    return icons[iconName] || '';
  }
</script>

<div class="main-section">
  <div class="grid-container">
    <div class="control-grid">
      {#each tiles as tile, index}
        {#if tile.type === 'dual'}
          <!-- Dual tile container -->
          <div class="dual-tile-container">
            {#each tile.subTiles as subTile}
              <div 
                class="sub-tile {subTile.isActive ? 'bg-blue-600' : 'bg-gray-400'}"
                onclick={() => handleTileClick(index, subTile.key)}
              >
                <div class="sub-icon-container">
                  {@html getMainIcon(subTile.icon)}
                </div>
                <span class="sub-tile-label">{subTile.label}</span>
              </div>
            {/each}
          </div>
        {:else}
          <!-- Single tile -->
          <div 
            class="control-tile {tile.isActive ? tile.bgColor : 'bg-gray-400'}"
            onclick={() => handleTileClick(index)}
          >
            <div class="icon-container">
              {@html getMainIcon(tile.icon)}
            </div>
            <span class="tile-label">{tile.label}</span>
          </div>
        {/if}
      {/each}
    </div>
  </div>
  
  <BottomBarComponent />
</div>

<style>
  .main-section {
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  .grid-container {
    flex: 1;
    padding: 0.75rem;
    background-color: #f3f4f6;
  }

  .control-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 0.75rem;
    height: 100%;
  }

  .control-tile {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    padding: 1rem;
    border-radius: 0.5rem;
    cursor: pointer;
    transition: all 0.2s;
    color: white;
    min-height: 80px;
    touch-action: manipulation;
  }

  .control-tile:hover {
    opacity: 0.8;
    transform: scale(0.98);
  }

  .control-tile:active {
    transform: scale(0.95);
  }

  /* Dual tile container */
  .dual-tile-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.5rem;
    height: 100%;
  }

  .sub-tile {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    padding: 0.75rem;
    border-radius: 0.375rem;
    cursor: pointer;
    transition: all 0.2s;
    color: white;
    min-height: 60px;
    touch-action: manipulation;
  }

  .sub-tile:hover {
    opacity: 0.8;
    transform: scale(0.98);
  }

  .sub-tile:active {
    transform: scale(0.95);
  }

  .bg-blue-600 { 
    background-color: #2563eb; 
  }
  
  .bg-gray-400 { 
    background-color: #9ca3af; 
  }

  .icon-container {
    flex-grow: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .sub-icon-container {
    flex-grow: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .tile-label {
    font-size: 1rem;
    font-weight: 500;
    text-align: center;
    line-height: 1.2;
    margin-top: 0.75rem;
  }

  .sub-tile-label {
    font-size: 0.875rem;
    font-weight: 500;
    text-align: center;
    line-height: 1.2;
    margin-top: 0.5rem;
  }

  :global(.main-icon) {
    width: 56px;
    height: 56px;
  }

  :global(.sub-tile .main-icon) {
    width: 40px;
    height: 40px;
  }

  @media (min-width: 768px) {
    .grid-container {
      padding: 1.5rem;
    }
    
    .control-grid {
      gap: 1.25rem;
    }
    
    .control-tile {
      padding: 1.5rem;
      min-height: 120px;
    }

    .sub-tile {
      padding: 1rem;
      min-height: 80px;
    }

    .dual-tile-container {
      gap: 0.75rem;
    }
    
    .icon-container {
      margin-bottom: 1rem;
    }

    .sub-icon-container {
      margin-bottom: 0.75rem;
    }
    
    .tile-label {
      font-size: 1.25rem;
    }

    .sub-tile-label {
      font-size: 1rem;
    }

    :global(.main-icon) {
      width: 72px;
      height: 72px;
    }

    :global(.sub-tile .main-icon) {
      width: 52px;
      height: 52px;
    }
  }

  @media (min-width: 1024px) {
    .control-tile {
      padding: 2rem;
      min-height: 140px;
    }

    .sub-tile {
      padding: 1.25rem;
      min-height: 100px;
    }

    .dual-tile-container {
      gap: 1rem;
    }
    
    .tile-label {
      font-size: 1.5rem;
      margin-top: 1.25rem;
    }

    .sub-tile-label {
      font-size: 1.125rem;
      margin-top: 1rem;
    }

    :global(.main-icon) {
      width: 88px;
      height: 88px;
    }

    :global(.sub-tile .main-icon) {
      width: 64px;
      height: 64px;
    }
  }

  @media (max-width: 320px) {
    :global(.main-icon) {
      width: 48px;
      height: 48px;
    }

    :global(.sub-tile .main-icon) {
      width: 36px;
      height: 36px;
    }

    .tile-label {
      margin-top: 0.75rem;
    }

    .sub-tile-label {
      font-size: 0.75rem;
      margin-top: 0.5rem;
    }
  }
</style>
