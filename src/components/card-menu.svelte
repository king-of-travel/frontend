{#if isOpenMenu}
  <div
    bind:this="{contentElement}"
    transition:fly="{{ y: contentElement.offsetHeight, duration: 200 }}"
    class="content"
  >
    <header on:click|stopPropagation="{closeCard}" class="{headerClass}">
      <slot name="header" />
    </header>
    <slot />
  </div>

  <div
    on:click="{closeCard}"
    transition:fade="{{ duration: 200 }}"
    class="background"
  ></div>
{/if}

<script>
  import { createEventDispatcher } from 'svelte';
  import { fly, fade } from 'svelte/transition';

  export let isOpenMenu = false,
    headerClass = undefined;

  let dispatch = createEventDispatcher();

  let contentElement;

  $: {
    let prevScroll = 0;

    /* Block the scroll page */
    if (isOpenMenu) {
      prevScroll = window.scrollY;

      document.body.style.top = `-${prevScroll}px`;
      document.body.classList.add('is-locked');
    } else {
      document.body.style.top = '';
      document.body.classList.remove('is-locked');
    }
  }

  function closeCard() {
    dispatch('closeMenu');
  }
</script>

<style>
  div {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
  }

  .background {
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.05);
    backdrop-filter: blur(2.5px);
    will-change: transform;
  }

  .background::after {
    content: '';
  }

  .content {
    z-index: 101;
    padding: 10px 20px;
    background-color: var(--base-background);
    border-top: 1px solid var(--base-border);
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    will-change: transform;
  }

  header {
    padding-bottom: 10px;
  }
</style>
