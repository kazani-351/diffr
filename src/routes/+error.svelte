<script>
  import { page } from '$app/stores';
  import { onMount } from 'svelte';

  let errorInfo = $page.error || { message: 'Something went wrong' };
  let statusCode = $page.status || 500;

  function resetError() {
    window.location.href = '/';
  }

  // Update error info when page store changes
  $: {
    errorInfo = $page.error || { message: 'Something went wrong' };
    statusCode = $page.status || 500;
  }
</script>

<svelte:head>
  <title>{statusCode} Error | Diffr</title>
  <meta name="robots" content="noindex" />
</svelte:head>

<header>
  <div class="logo">
    Diffr <span>.</span>
  </div>
</header>

<div class="container">
  <div class="content">
    <div class="error-code">{statusCode}</div>
    <h1>{statusCode === 404 ? 'Page Not Found' : 'Oops! Something went wrong'}</h1>
    <p class="error-message">{errorInfo.message}</p>
    <button on:click={resetError}>Back to Home</button>
  </div>
</div>

<style>
  header {
    padding: 1rem 2rem;
    border-bottom: 1px solid var(--border-color);
    background-color: var(--bg-body);
  }

  .logo {
    font-size: 1.5rem;
    font-weight: 800;
    letter-spacing: -0.05em;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .logo span {
    color: var(--primary-color);
  }

  .container {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    background-color: var(--bg-body);
    color: var(--text-main);
  }

  .content {
    text-align: center;
    max-width: 500px;
  }

  .error-code {
    font-size: 8rem;
    font-weight: 800;
    color: var(--primary-color);
    margin: 0;
    line-height: 1;
    opacity: 0.3;
  }

  h1 {
    font-size: 2rem;
    font-weight: 600;
    margin: 1rem 0;
    color: var(--text-main);
  }

  .error-message {
    font-size: 1.1rem;
    color: var(--text-muted);
    margin: 0.5rem 0 2rem 0;
    line-height: 1.6;
  }

  button {
    padding: 0.75rem 1.5rem;
    background-color: var(--primary-color);
    color: white;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  button:hover {
    background-color: var(--primary-hover);
  }

  button:focus {
    outline: 2px solid var(--primary-color);
    outline-offset: 2px;
  }
</style>
