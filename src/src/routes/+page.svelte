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

  // Debounce function to prevent freezing on huge text inputs
  let timeout;
  const debounce = (func, delay) => {
    return (...args) => {
      clearTimeout(timeout);
      timeout = setTimeout(() => func.apply(this, args), delay);
    };
  };

  // Core Diff Logic
  const computeDiff = debounce(() => {
    if (!leftText && !rightText) {
      diffResult = [];
      return;
    }

    const diff = diffLines(leftText, rightText);
    diffResult = diff;

    // Calculate stats
    stats = diff.reduce(
      (acc, part) => {
        if (part.added) acc.additions += part.count || 1;
        if (part.removed) acc.deletions += part.count || 1;
        return acc;
      },
      { additions: 0, deletions: 0 }
    );
  }, 300); // 300ms delay

  // Reactively trigger diff on text change
  $: if (leftText !== undefined || rightText !== undefined) {
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

  // Check system preference for theme
  onMount(() => {
    if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
      document.documentElement.classList.add('dark');
    }
  });
</script>

<svelte:head>
  <title>Diffr - Text Comparison Tool</title>
  <meta name="description" content="Fast, private, and secure text diff tool." />
</svelte:head>

<header>
  <div class="logo">
    Diffr <span>.</span>
  </div>
  <div class="controls">
    <button on:click={loadExample}>Load Example</button>
    <ThemeToggle />
  </div>
</header>

<main>
  <div class="input-pane">
    <label for="left-input">Original Text</label>
    <textarea id="left-input" bind:value={leftText} placeholder="Paste original text here..."></textarea>
  </div>

  <div class="input-pane">
    <label for="right-input">Modified Text</label>
    <textarea id="right-input" bind:value={rightText} placeholder="Paste modified text here..."></textarea>
  </div>

  <div class="input-pane" style="flex: 2;">
    <div style="display:flex; justify-content:space-between; align-items:center;">
      <label>Comparison Result</label>
      <div style="display:flex; gap:0.5rem;">
        <button class:active={viewMode === 'split'} on:click={() => viewMode = 'split'}>Split</button>
        <button class:active={viewMode === 'unified'} on:click={() => viewMode = 'unified'}>Unified</button>
      </div>
    </div>
    
    <div class="diff-container">
      <div class="diff-viewer {viewMode === 'split' ? 'split-view' : 'unified-view'}">
        {#if diffResult.length > 0}
          <DiffViewer {diffResult} {viewMode} />
        {:else}
          <div style="padding: 2rem; text-align: center; color: var(--text-muted);">
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
