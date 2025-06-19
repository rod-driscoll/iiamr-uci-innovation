<script lang="ts">
  let showColon = true;

  function getOrdinal(n: number) {
    if (n > 3 && n < 21) return n + 'th';
    switch (n % 10) {
      case 1: return n + 'st';
      case 2: return n + 'nd';
      case 3: return n + 'rd';
      default: return n + 'th';
    }
  }
  function formatTime(date: Date) {
    const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
    const day = days[date.getDay()];
    const dateNum = getOrdinal(date.getDate());
    const month = months[date.getMonth()];
    let hours = date.getHours();
    const mins = date.getMinutes().toString().padStart(2, '0');
    const ampm = hours >= 12 ? 'pm' : 'am';
    hours = hours % 12;
    if (hours === 0) hours = 12;
    return { day, dateNum, month, hours, mins, ampm };
  }

  let now = new Date();
  let timeParts = formatTime(now);
  let show = true;

  const updateTime = () => {
    now = new Date();
    timeParts = formatTime(now);
    show = !show;
  };
  const interval = setInterval(updateTime, 1000);

  import { onDestroy } from 'svelte';
  onDestroy(() => clearInterval(interval));
</script>

<style>
  .DateTime {
    font-variant-numeric: tabular-nums;
  }
  .colon {
    display: inline-block;
    width: 1ch;
    text-align: center;
    transition: opacity 0.2s;
  }
</style>

<p class="DateTime">
  {timeParts.day} {timeParts.dateNum} {timeParts.month} | {timeParts.hours}<span class="colon" style="opacity: {show ? 1 : 0}">:</span>{timeParts.mins}{timeParts.ampm}
</p>