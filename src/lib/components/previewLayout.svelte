<script lang="ts">
  let { fileName = '', children } = $props();
</script>

<div class="root">
  <header class="head">
    <div class="chil"><p class="title">{fileName}</p></div>
    <div class="chil"></div>
  </header>
  <main class="content">
    {@render children()}
  </main>
</div>

<style lang="scss">
  *, *::before, *::after {
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

      width: calc(100% - $margin_len * 2);
      height: $head_height;
      display: flex;
      justify-content: space-between;

      border: 1px solid var(--foreground);
      border-radius: 10px;
      background-color: transparent;
      padding: 0 8px;

      .chil {
        height: 100%;
        display: flex;
        align-items: center;
        .title {
          font-size: 22px;
          margin: 0; 
        }
      }
    }

    .content {
      padding-top: calc($head_height+ $margin_len * 2);
      height: 100%;
      overflow-y: auto;
    }
  }
</style>