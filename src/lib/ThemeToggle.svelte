<script>
  let isDark = false;

  // Check current state
  if (typeof document !== 'undefined') {
    isDark = document.documentElement.classList.contains('dark');
  }

  function toggleTheme() {
    isDark = !isDark;
    if (isDark) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }
</script>

<button on:click={toggleTheme} aria-label="Toggle Dark Mode">
  {#if isDark}
    ☀️
  {:else}
    🌙
  {/if}
</button>
