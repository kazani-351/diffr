<script>
  import { onMount } from 'svelte';
  import { diffLines } from 'diff';
  import DiffViewer from '$lib/DiffViewer.svelte';
  import ThemeToggle from '$lib/ThemeToggle.svelte';

  let leftText = '';
  let rightText = '';
  let diffResult = [];
  let viewMode = 'split'; // 'unified' or 'split'
  let stats = { additions: 0, deletions: 0 };
  let isComputing = false;

  // Debounce function to prevent freezing on huge text inputs
  let timeout;
  const debounce = (func, delay) => {
    return (...args) => {
      clearTimeout(timeout);
      isComputing = true;
      timeout = setTimeout(() => {
        func.apply(this, args);
        isComputing = false;
      }, delay);
    };
  };

  // Core Diff Logic
  const computeDiff = debounce(() => {
    try {
      if (!leftText && !rightText) {
        diffResult = [];
        stats = { additions: 0, deletions: 0 };
        return;
      }

      const diff = diffLines(leftText, rightText);
      diffResult = diff;

      // Calculate stats - improved counting
      stats = diff.reduce(
        (acc, part) => {
          const lines = part.value.split('\n').filter(line => line !== '').length;
          if (part.added) acc.additions += lines;
          if (part.removed) acc.deletions += lines;
          return acc;
        },
        { additions: 0, deletions: 0 }
      );
    } catch (error) {
      console.error('Error computing diff:', error);
      diffResult = [];
      stats = { additions: 0, deletions: 0 };
    }
  }, 300); // 300ms delay

  // Reactively trigger diff on text change - optimized
  $: if (leftText !== undefined && rightText !== undefined) {
    computeDiff();
  }

  // Local Example
  const loadExample = () => {
    leftText = `function helloWorld() {
  console.log("Hello World");
  return true;
}`;
    rightText = `function helloEarth() {
  console.log("Hello Earth");
  console.log("How are you?");
  return false;
}`;
  };

  // Clear all inputs
  const clearAll = () => {
    leftText = '';
    rightText = '';
    diffResult = [];
    stats = { additions: 0, deletions: 0 };
  };

  // Load theme preference
  onMount(() => {
    const savedTheme = localStorage.getItem('theme');
    if (savedTheme === 'dark') {
      document.documentElement.classList.add('dark');
    } else if (savedTheme === 'light') {
      document.documentElement.classList.remove('dark');
    } else if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
      document.documentElement.classList.add('dark');
    }
  });
</script>

<svelte:head>
  <title>Diffr - Text Comparison Tool</title>
  <meta name="description" content="Fast, private, and secure text diff tool." />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</svelte:head>

<header>
  <div class="logo">
    Diffr <span>.</span>
  </div>
  <div class="controls">
    <button on:click={loadExample} aria-label="Load example text">Load Example</button>
    <button on:click={clearAll} aria-label="Clear all text">Clear</button>
    <ThemeToggle />
  </div>
</header>

<main>
  <div class="input-pane">
    <label for="left-input">Original Text</label>
    <textarea 
      id="left-input" 
      bind:value={leftText} 
      placeholder="Paste original text here..."
      aria-label="Original text input"
    ></textarea>
  </div>

  <div class="input-pane">
    <label for="right-input">Modified Text</label>
    <textarea 
      id="right-input" 
      bind:value={rightText} 
      placeholder="Paste modified text here..."
      aria-label="Modified text input"
    ></textarea>
  </div>

  <div class="input-pane" style="flex: 2;">
    <div class="diff-header">
      <label>Comparison Result</label>
      <div class="view-controls">
        <button 
          class:active={viewMode === 'split'} 
          on:click={() => viewMode = 'split'}
          aria-label="Split view"
          aria-pressed={viewMode === 'split'}
        >
          Split
        </button>
        <button 
          class:active={viewMode === 'unified'} 
          on:click={() => viewMode = 'unified'}
          aria-label="Unified view"
          aria-pressed={viewMode === 'unified'}
        >
          Unified
        </button>
      </div>
    </div>
    
    <div class="diff-container">
      <div class="diff-viewer {viewMode === 'split' ? 'split-view' : 'unified-view'}" role="region" aria-label="Diff output">
        {#if isComputing}
          <div class="loading">
            Computing diff...
          </div>
        {:else if diffResult.length > 0}
          <DiffViewer {diffResult} {viewMode} />
        {:else}
          <div class="empty-state">
            Start typing or paste text to see differences.
          </div>
        {/if}
      </div>
      <div class="stats">
        <span class="stat-item"><strong style="color: var(--diff-add-text)">+{stats.additions}</strong> additions</span>
        <span class="stat-item"><strong style="color: var(--diff-del-text)">-{stats.deletions}</strong> deletions</span>
      </div>
    </div>
  </div>
</main>

<style>
  .diff-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .view-controls {
    display: flex;
    gap: 0.5rem;
  }

  .loading {
    padding: 2rem;
    text-align: center;
    color: var(--text-muted);
  }

  .empty-state {
    padding: 2rem;
    text-align: center;
    color: var(--text-muted);
  }

  button[aria-pressed="true"] {
    background-color: var(--primary-color);
    color: white;
    border-color: var(--primary-color);
  }
</style>
