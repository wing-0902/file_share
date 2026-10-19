<script lang="ts">
  import type { Snippet } from 'svelte';

  // fileName: string
  // info: Key-Valueの配列
  // children: 従来の<slot />
  // table: 従来の名前付き<slot />
type InfoItem = {
    key: string;
    value: string | number;
  };
  let {
    fileName = '',
    info,
    children,
    table
  }: { fileName: string; info?: InfoItem; children: Snippet; table?: Snippet } = $props();
</script>

<div class="root">
  <header class="head">
    <div class="chil"><p class="title">{fileName}</p></div>
    <div class="chil">
      {#if info}
        <button title="情報を表示">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="7 7 10 10"
            fill="none"
            stroke="currentColor"
            stroke-width="0.8"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="icon icon-tabler icons-tabler-outline icon-tabler-info-small"
          >
            <path stroke="none" d="M0 0h24v24H0z" fill="none" />
            <path d="M12 9h.01" />
            <path d="M11 12h1v4h1" />
          </svg>
        </button>
        {#if table}
          <button title="目次">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 -3 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="icon icon-tabler icons-tabler-outline icon-tabler-menu-2"
            >
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path d="M4 6l16 0" />
              <path d="M4 12l16 0" />
              <path d="M4 18l16 0" />
            </svg>
          </button>
        {/if}
      {/if}
    </div>
  </header>
  <main class="content">
    {@render children()}
  </main>
</div>

<style lang="scss">
  *,
  *::before,
  *::after {
    box-sizing: border-box;
  }

  .root {
    width: 100%;
    height: 100dvh;
    position: relative;

    $head_height: 44px;
    $margin_len: 9px;

    .head {
      position: fixed;
      top: $margin_len;
      left: $margin_len;
      z-index: 10;

      width: calc(100% - #{$margin_len * 2});
      height: $head_height;
      display: flex;
      justify-content: space-between;

      border: 1px solid var(--foreground);
      border-radius: 10px;
      background-color: transparent;
      padding: 0 8px;

      backdrop-filter: blur(8px);

      .chil {
        height: 100%;
        display: flex;
        align-items: center;
        background-color: transparent;

        .title {
          font-size: 22px;
          margin: 0;
          background-color: transparent;
        }
      }
    }

    .content {
      padding-top: calc(#{$head_height + $margin_len * 2});
      height: 100%;
      overflow-y: auto;
    }
  }
</style>
