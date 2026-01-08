<script>
  export let diffResult = [];
  export let viewMode = 'split';

  // Synchronize scrolling for split view
  let leftPane;
  let rightPane;
  let isScrollingLeft = false;
  let isScrollingRight = false;

  function syncScrollLeft(e) {
    if (!isScrollingRight) {
      isScrollingLeft = true;
      const percentage = e.target.scrollTop / (e.target.scrollHeight - e.target.clientHeight);
      if (rightPane) {
        rightPane.scrollTop = percentage * (rightPane.scrollHeight - rightPane.clientHeight);
      }
      setTimeout(() => isScrollingLeft = false, 50);
    }
  }

  function syncScrollRight(e) {
    if (!isScrollingLeft) {
      isScrollingRight = true;
      const percentage = e.target.scrollTop / (e.target.scrollHeight - e.target.clientHeight);
      if (leftPane) {
        leftPane.scrollTop = percentage * (leftPane.scrollHeight - leftPane.clientHeight);
      }
      setTimeout(() => isScrollingRight = false, 50);
    }
  }

  // Helper to get line count from a part
  function getLineCount(part) {
    if (part.count) return part.count;
    return part.value.split('\n').filter(line => line !== '').length;
  }
</script>

<style>
  .diff-viewer-inner {
    display: contents;
  }

  .split-pane {
    overflow-y: auto;
    height: 100%;
  }

  .split-pane::-webkit-scrollbar {
    width: 8px;
    height: 8px;
  }

  .split-pane::-webkit-scrollbar-track {
    background: var(--bg-panel);
  }

  .split-pane::-webkit-scrollbar-thumb {
    background: var(--border-color);
    border-radius: 4px;
  }

  .split-pane::-webkit-scrollbar-thumb:hover {
    background: var(--text-muted);
  }
</style>

<div class="diff-viewer-inner">
  {#if viewMode === 'unified'}
    <!-- Unified View Rendering -->
    {#each diffResult as part}
      {#each part.value.split('\n') as line, idx}
        {#if line !== '' || idx !== part.value.split('\n').length - 1}
          <span class="diff-line {part.added ? 'added' : ''} {part.removed ? 'removed' : ''}">
            {part.removed ? '- ' : part.added ? '+ ' : '  '}{line}
          </span>
        {/if}
      {/each}
    {/each}

  {:else}
    <!-- Split View Rendering -->
    <!-- Left Column -->
    <div class="split-pane" bind:this={leftPane} on:scroll={syncScrollLeft} role="region" aria-label="Original text">
      {#each diffResult as part}
        {#if part.removed}
          {#each part.value.split('\n') as line, idx}
            {#if line !== '' || idx !== part.value.split('\n').length - 1}
              <span class="diff-line removed">
                - {line}
              </span>
            {/if}
          {/each}
        {:else if part.added}
          <!-- Empty placeholder on left for additions -->
          {#each Array(getLineCount(part)) as _}
            <span class="diff-line empty"></span>
          {/each}
        {:else}
          <!-- Unchanged lines -->
          {#each part.value.split('\n') as line, idx}
            {#if line !== '' || idx !== part.value.split('\n').length - 1}
              <span class="diff-line">
                {line}
              </span>
            {/if}
          {/each}
        {/if}
      {/each}
    </div>

    <!-- Right Column -->
    <div class="split-pane" bind:this={rightPane} on:scroll={syncScrollRight} role="region" aria-label="Modified text">
      {#each diffResult as part}
        {#if part.added}
          {#each part.value.split('\n') as line, idx}
            {#if line !== '' || idx !== part.value.split('\n').length - 1}
              <span class="diff-line added">
                + {line}
              </span>
            {/if}
          {/each}
        {:else if part.removed}
          <!-- Empty placeholder on right for deletions -->
          {#each Array(getLineCount(part)) as _}
            <span class="diff-line empty"></span>
          {/each}
        {:else}
          <!-- Unchanged lines -->
          {#each part.value.split('\n') as line, idx}
            {#if line !== '' || idx !== part.value.split('\n').length - 1}
              <span class="diff-line">
                {line}
              </span>
            {/if}
          {/each}
        {/if}
      {/each}
    </div>
  {/if}
</div>
