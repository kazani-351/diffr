<script>
  export let diffResult = [];
  export let viewMode = 'split';

  // Synchronize scrolling for split view
  let leftPane;
  let rightPane;

  function syncScroll(e) {
    const target = e.target;
    if (viewMode === 'split') {
      // Basic synchronization logic can be complex, 
      // for this MVP we let them scroll independently or link percentages
      // Simple linking:
      const percentage = target.scrollTop / (target.scrollHeight - target.clientHeight);
      
      // If user scrolled left
      if (target === leftPane && rightPane) {
        rightPane.scrollTop = percentage * (rightPane.scrollHeight - rightPane.clientHeight);
      }
      // If user scrolled right
      else if (target === rightPane && leftPane) {
        leftPane.scrollTop = percentage * (leftPane.scrollHeight - leftPane.clientHeight);
      }
    }
  }
</script>

<div class="diff-viewer-inner" style="display: contents;">
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
    <div class="split-pane" bind:this={leftPane} on:scroll={syncScroll}>
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
           {#each Array(part.count || part.value.split('\n').length - 1) as _}
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
    <div class="split-pane" bind:this={rightPane} on:scroll={syncScroll}>
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
           {#each Array(part.count || part.value.split('\n').length - 1) as _}
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
