<script>
  let isDark = false;

  // Check current state and load from localStorage
  if (typeof document !== 'undefined') {
    isDark = document.documentElement.classList.contains('dark');
  }

  function toggleTheme() {
    isDark = !isDark;
    if (isDark) {
      document.documentElement.classList.add('dark');
      localStorage.setItem('theme', 'dark');
    } else {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('theme', 'light');
    }
  }
</script>

<button 
  on:click={toggleTheme} 
  aria-label="Toggle Dark Mode"
  aria-pressed={isDark}
>
  {#if isDark}
    ☀️
  {:else}
    🌙
  {/if}
</button>
